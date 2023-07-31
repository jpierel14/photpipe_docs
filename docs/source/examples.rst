********
Examples
********

DECamNOAO
----------

There are three steps to reduce DECamNOAO data: downloading the data, the photometric step, & the difference step. The following will take you through the steps to run the stages. Please note that these examples are for the YSE collaboration, which you may not be a part of. If you are not a part of YSE, contact Armin Rest to learn what the correct commands for your collaboration are.

1. Downloading Data

To start, you need to download your data which is as follows ::
    
    YSE_download.py --IDs <Field>

If you are using data that is not public yet, you will need an account on NoirLab. At this point, you will have your username and password. Before running the command below, you need to add ``export NOIR_API_TOKEN='blablabla'`` to your bashrc file for the API link to work. The steps to do this are as follows. (Please note that the steps involved may vary from operating systems). ::

    vim ~/.bashrc (this opens the file)
    Paste export NOIR_API_TOKEN='blablabla'
    run: shift+!+q (this exits the file)
    source ~/.bashrc (this runs the bashrc and updates it based on new edits)

Now you have added the API call for NoirLab and can move on to download private data for your collaboration. You will use the following command, or else you will be restricted from using the aforementioned data. ::
    
    YSE_download.py --IDs <Field> --login <NoirLab Username> <NoirLab Password>

YSE_download.py has many tags that you can use that might be useful to you. To see the complete list of tags, run the following. :: 
    
    YSE_download.py —-help

Once you run the script, it will get all the data related to your object. Once it is finished, you can now move on to the next steps.

2. Photometric Step

DECamNOAO has 62 CCDs, and thus your data can be in any of them. Typically your data will reside in CCD 35, but if it is not specified, you will need to look through all the CCDs until you find it.

Below is the list of the photometric steps. You must follow the steps in order otherwise, your data will not be able to be reduced. ::

    Stage           Actions             Prestage
    ---------------------------------------------------
    FINDNEWIM       findnewimages       Start
    CPFIX           cpfixdecamnoao      FINDNEWIM
    FIXAMPOFFSET    fixampoffset        CPFIX
    MKSATMASK       mksatmask           FIXAMPOFFSET
    SWARP           swarp               MKSATMASK  
    DOPHOTFIM       dophotflattenedim   MKSATMASK
    ABSPHOTFIM      absphotflattenedim  DOPHOTFIM
    SWARPSTACK      swarpstack          ABSPHOTFIM
    DOPHOT          dophot              SWARP,SWARPSTACK
    ABSPHOT         absphot             DOPHOT

The command to go through the photometric step is as follows. ::

    pipeloop.pl -red FIELD AMP -stage STAGENAME 

And an example of this (YSE example) ::

    pipeloop.pl -red 2022zut 35 -stage FINDNEWIM
    pipeloop.pl -red 2022zut 35 -stage CPFIX
    ....
    pipeloop.pl -red 2022zut 35 -stage ABSPHOT

Once you ensure you can reduce your data successfully, you can combine the command to go through all the stages simultaneously. ::
    
    pipeloop.pl -red FIELD AMP -stage FINDNEWIM-ABSPHOT 

If you go through this step multiple times, you will need to add a -redo tag at the end of the command. This will delete the previous run's data and then restart. For example :: 
    
    pipeloop.pl -red 2022zut 35 -stage FINDNEWIM -redo 
    OR 
    pipeloop.pl -red 2022zut 35 -stage FINDNEWIM-ABSPHOT -redo 

After the success of each stage, the information will be stored in ``photpipe/v20.0/DECAMNOAO/YSE/workspace/FIELD/AMP``

3. Difference Step

For the difference step, you need two files.

a) ``getorder_templates.py``
b) ``pdastro.py``

If you do not have them, ask Armin Rest for these files. Once you receive them, put them in ``photpipe/v20.0/DECAMNOAO/YSE/workspace``

At this point, you are trying to identify a template for your data. A template is essentially a before comparing your data. There needs to be a template per band selected.
To select your template, you need to run: ::
    
    python getorder_templates.py <PATH_TO_DATA_FROM_PHOTOMETRIC_STEPS> 

For example, when I select the templates for supernova objects I run: ::
    
    python getorder_templates.py /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/FIELD/AMP/*_BAND_*.dcmp

You can also run: ::
    
    python getorder_templates.py /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/FIELD/AMP/*.dcmp 

so it will generate the list of possible templates for all bands for that object

After this command, a text file will appear in photpipe/v20.0/DECAMNOAO/YSE/workspace/template_order/FIELD/AMP. In there, there will be anywhere from 4-6 files. If you open a file, you will see something like the below: ::

                                                                                                                    dcmpfile PHOTCODE      FWHM  M5SIGMA EXPTIME  SKYADU     SKYSIG  FOM_dist
    /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/2022aczp/35/2022aczp.190217.824415_ooi_z_ls9_S24.sw.dcmp 0x235016  2.466073  22.7186    56.0   948.0  14.399902 -1.672596
    /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/2022aczp/35/2022aczp.190217.824415_ooi_z_ls9_S23.sw.dcmp 0x235016  2.476049  22.6983    56.0   944.0  14.399902 -1.646249
    /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/2022aczp/35/2022aczp.190217.824422_ooi_z_ls9_N25.sw.dcmp 0x235016  2.69066  22.6158    56.0   907.0  14.100098 -1.394304
    /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/2022aczp/35/2022aczp.190217.824422_ooi_z_ls9_N29.sw.dcmp 0x235016  2.732832   22.584    56.0   907.0  14.399902 -1.332994
    /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/2022aczp/35/2022aczp.190714.873092_ooi_z_v1_N18.sw.dcmp 0x235016   2.76427  22.4747    57.0  1510.0  17.800049   -1.2135
    /projects/caps/uiucsn/DECamNOAO/photpipe/v20.0/DECAMNOAO/YSE/workspace/2022aczp/35/2022aczp.230202.1169992_ooi_z_v1_N4.sw.dcmp 0x235016   2.606629  21.9348    15.0   248.0   7.699951 -0.873439

The command will go through all the files based on the band, write out the files that fit the command, and display the photcode, a unique indicator to band, filter, and telescope. It then lists a couple of metrics, the most important being FOM_dist. The more negative the number, the better it is. However, it is not a clear-cut decision. In the dcmpfile name, the six digits after the FIELD name are when the data was observed. You want to consider this when selecting your template, as you want a relevant one. Choosing a very old template will impact the ability of the later differencing commands to extract valuable data. For supernovae, you want templates within the last two years of the object detection to identify you can gain pre-explosion data. Once you identify which row you want to be the template, you need to extract the expnum, which are the digits directly after the six digits date of observation. Once you repeat this for all relevant bands to your object, you can move on to the next step.

You then need to download the template via: ::

    YSE_download.py --pointing_s <POINTING_NAME> -s -l <TEMPLATE_ID> --tmpl --deeplinkcheck

Sometimes the template is properity data, so to access them, you need to add your prop ID: ::

    YSE_download.py --pointing_s <POINTING_NAME> -s -l <SOMEID> --propID <PROP-ID> --tmpl --deeplinkcheck --login <NoirLab Username> <NoirLab Password>

It is important to note that Pointing Name is nearly always the same as the FIELD name except for fields; in that case, consult the "pointing" column in ``<Program>.pointing.txt``

Once that is complete, you then need to do: ::

    pipeloop.pl -red tmpl <AMP> -redo

Notice here I preemptively added the ``-redo`` tag; I suggest always having it since it prevents any issues in the future.

Then you need to do the difference stages: ::

    Stage           Actions             Prestage
    ---------------------------------------------------
    MATCHTEMPL      matchtemplates      ABSPHOT,ABSPHOT 
    DIFFIM          diffim              MATCHTEMPL      
    DIFFIMSTATS     diffimstats         DIFFIM              
    DIFFDOPHOT      diffdophot          DIFFIMSTATS     
    PIXCHK          pixchk              DIFFDOPHOT      
    DIFFCUT         diffcut             PIXCHK              
    IMINFO2         iminfo              DIFFCUT            
    CLUSTEROBJ      clusterobj          IMINFO2            
    YSE2CLUSTER     yse2cluster         CLUSTEROBJ
    FORCEDOPHOT     forcedophot         YSE2CLUSTER
    WEBSNIFF        websniff            FORCEDOPHOT

And you run it as below; 2022zut is a YSE object, so you may be unable to reproduce the result. ::

    pipeloop.pl -diff 2022zut tmpl 35 -stage MATCHTEMPL -redo
    pipeloop.pl -diff 2022zut tmpl 35 -stage DIFFIM -redo
    pipeloop.pl -diff 2022zut tmpl 35 -stage DIFFIMSTATS -redo
    pipeloop.pl -diff 2022zut tmpl 35 -stage DIFFDOPHOT -redo
    pipeloop.pl -diff 2022zut tmpl 35 -stage PIXCHK -redo
    pipeloop.pl -diff 2022zut tmpl 35 -stage DIFFCUT -redo
    pipeloop.pl -diff 2022zut tmpl 35 -forcestage IMINFO2 -redo
    pipeloop.pl -diff 2022zut tmpl 35 -forcestage CLUSTEROBJ -k CO_DELTA_MJD_BACK 300 -redo -k CO_NMIN_INSIDE 6 -redo
    Recent → 300
    OLDER → 2000
    pipeloop.pl -diff 2022zut tmpl 35 -forcestage YSE2CLUSTER -redo
    pipeloop.pl -diff 2022zut tmpl 35 -forcestage FORCEDOPHOT -redo
    pipeloop.pl -diff 2022zut tmpl 35 -forcestage WEBSNIFF -redo

Notes about the above command,

a) For IMINFO2 you have to do the -forcestage tag to run it
b) For ``CLUSTEROBJ``, ``CO_DELTA_MJD_BACK`` is how far you look back; for "recent objects," use 300, and objects that have been occurring for a "long" time, use 2000 or whatever is best for your object
c) For ``CLUSTEROBJ``, ``CO_NMIN_INSIDE`` refers to how many data points with an SNR 3+ to be displayed. The lower the value, the more "possible" objects will be displayed. For supernovae, 5-7 is ideal

Once ``WEBSNIFF`` is completed, you will have a new folder in ``photpipe/v20.0/DECAMNOAO/YSE/`` called web. You open web, open sniff, and then click the index.html file. This will open a webpage that is local to your computer. There it will list all the reduced objects. You click on the object of interest, click on the AMP, and it will open a page to see the light curve and what bands have data points with an SNR of 3 or more. Not all objects will have data for all bands; the majority should be r and i, but depending on when objects were being observed, there can be g and z bands as well. If you see more than one band, you might have chosen a bad template and thus need to redo the steps and select a better one. As of 07/21/2023, there has yet to be an exact method to determine the template; it is a work in progress.

SOMETHING ELSE
--------------
