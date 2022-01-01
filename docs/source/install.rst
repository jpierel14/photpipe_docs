************
Installation
************

Shell
=====

Historically, we used tcsh. However, recently, we have moved over to bash shell (astroconda is in bash). The pipeline is shell agnostic, with the only exception being the 'sourceme' files that initialize a pipeline sesssion. Therefore we recommend using bash mode going forward

Astroconda
==========

We recommend using `astroconda to install <http://astroconda.readthedocs.io/en/latest/installation.html>`_ an appropriate environment. If you are installing photpipe on Mac, then an Anaconda (or miniconda) installation is *required*, and you should make sure that your environment variable ``CONDA_PREFIX`` is set to the location of the base anaconda directory, for example::

   echo $CONDA_PREFIX
   /opt/miniconda3 

- If you use the WCSNONLIN stage (nonlinear WCS), you need to install the `Legacy Software Stack with Iraf <https://astroconda.readthedocs.io/en/latest/installation.html#iraf-install>`_ and python 2.7
- Otherwise we use either the `standard astroconda installation <https://astroconda.readthedocs.io/en/latest/installation.html#standard-install>`_ with the latest python 3.X, or the `STScI pipeline software stack <https://astroconda.readthedocs.io/en/latest/installation.html#pipeline-install-jump>`_
- Make sure astropy and matplotlib are available

Here is an example of a astroconda installation that works:

   conda config --add channels http://ssb.stsci.edu/astroconda
   conda create -n conda_photpipe python=3.7 stsci notebook astropy matplotlib
   #conda install --name conda_photpipe astropy matplotlib pandas
   pip install ipython jupyter matplotlib pylint pandas
   conda install -c conda-forge astroplan

   conda create -n sextractor -c https://ssb.stsci.edu/astroconda sextractor
   #conda activate sextractor
   #see https://stsci.service-now.com/stars?id=kb_article&sys_id=9b9073ab1b6be410e4c6edb0604bcb95

   condor:
   pip install --force git+https://github.com/jhunkeler/htc_utils
   pip install --upgrade htc_utils
   export PATH=$PATH:/usr/sbin
   (add path to .bashrc or .myalias)

   conda activate conda_photpipe
   if this doesn't work, do a 
   conda init bash


Photpipe
========

Photpipe is on bitbucket, make an account and ask A. Rest to add you to the repository. Then you can clone it with::

   git clone https://<yourprofile>@bitbucket.org/arest/photpipe.git

This command will put Photpipe into the current directory ``<pipedir>/photpipe``. 

Inititalization
===============

Each telescope/project has its own initialization 'sourceme' file. Any time one of these sourceme files are called, they overwrite the previous photpipe initialization, i.e. you can switch from one telescope/project to another. All of the sourceme files look very similar. First, the instrument (PIPE_INSTRUMEN), project (PIPENAME), and version (PIPE_VERSION) are defined such as the following::

   # define project and pipeversion
   export PIPE_INSTRUMENT=DECAMNOAO
   export PIPENAME=LE
   export PIPE_VERSION=v20.0

These are used to define the two main directories, depending on which host system you are on; defined as the following::

   if [[ $HOSTNAME =~ arminmac* ]] || [[  $HOSTNAME =~ lswlan* ]] || [[  $HOSTNAME =~ danis-iph* ]]; then
   export PIPE_SRC=/Users/arest/pipes/$PIPE_VERSION/photpipe
   export PIPE_DATA=/Users/arest/data/$PIPE_VERSION/$PIPE_INSTRUMENT/$PIPENAME
   export PIPE_WEB=$PIPE_DATA/web
   export LOCALPYTHONFLAG=1
   export IRAFHOME=/home/$USER/iraf
   export PIPE_BATCH_SYSTEM=NONE
   elif [[ $HOSTNAME =~ plhstproc1.stsci.edu ]] || [[ $HOSTNAME =~ plhstproc2.stsci.edu ]]; then
      export PIPE_SRC=/ifs/cs/projects/armin1/pipe/$PIPE_VERSION/photpipe
      export PIPE_DATA=/ifs/cs/projects/armin1/data/$PIPE_VERSION/$PIPE_INSTRUMENT/$PIPENAME
      export PIPE_WEB=$PIPE_DATA/web
      export LOCALPYTHONFLAG=1
      export IRAFHOME=/home/$USER/iraf
      export PIPE_BATCH_SYSTEM=Condor
   else
      echo "Hostname $HOSTNAME is not defined yet in the sourceme file!"
      return 1;
   fi

All new users of photpipe must add their system to the sourceme file; just add another elif statement with your hostname and directory set up. If you use astroconda, set ``LOCALPYTHONFLAG=1``, otherwise it tries to use the python within the pipeline, which is how we did it 10 years ago in order to avoide python version issues with the code. With astroconda, we have abandoned the photpipe python installation...  

All source code is in ``$PIPE_SRC=<photdir>/photpipe``, hence the directory where you installed photpipe. The config files for the given instrument+projects are in::

   $PIPE_SRC/config/$PIPE_INSTRUMENT/$PIPENAME

The data will be in ``$PIPE_DATA=<datadir>/$PIPE_VERSION/$PIPE_INSTRUMENT/$PIPENAME``.

From then on, you just need to call the sourceme file, and all paths and environments are set! To call the sourceme file easily, set up aliases in your ./bashrc or other bash file where you want your aliases. The following is an example for creating the alias for the telescope/project ledecamnoao::

   alias ledecamnoao='source  <photdir>/photpipe/config/DECAMNOAO/LE/LE.bash.sourceme'

There are a number of aliases in photpipe used for convenience, which moves you around the source code and data directories. After initializing a telescope/project, the following are aliases that can be used to navigate photpipe with more ease::

   cdconfig
   cddata
   cdraw
   cdwork
   cdperl
   cdpy
   cdsrc

For DECam light echoes using NOAO reduced images, we set up the following aliases for the individual targets::

   cr, ch, kp, wb, ks, lmc, smc

The following is an alias example for LMC light echoes::

   (iraf27) plhstproc1:/home/arest $ alias lelmc
   alias lelmc='source  /ifs/cs/projects/armin1/pipe/v20.0/photpipe/config/DECAMNOAO/LE/LE.bash.sourceme lmc'
   arest@plhstproc1(le LMC,noao,bash)% lelmc
   pipedata=/ifs/cs/projects/armin1/data/v20.0/DECAMNOAO/LElmc
   arest@plhstproc1(le LMC,noao,bash)% cdconfig
   /ifs/cs/projects/armin1/pipe/v20.0/photpipe/config/DECAMNOAO/LE
   arest@plhstproc1(le LMC,noao,bash)% cddata
   /ifs/cs/projects/armin1/data/v20.0/DECAMNOAO/LElmc


Compiling C Code
================

The first thing you must make sure of is that you have a ``gcc`` compiler installed. 

**Linux**

On linux this will likely be the case, but you can check with::

   gcc --version

If ``gcc`` is not installed, follow the directions `here <https://linuxize.com/post/how-to-install-gcc-compiler-on-ubuntu-18-04/>`_. Photpipe compiles on ``gcc`` version 7, but not on version 11, so you should get something like the following::

   $ gcc --version
   gcc (Homebrew GCC 7.5.0_4) 7.5.0

**Mac**

On Mac, you can check for ``gcc`` in the same way as above. On newer systems, you will not get an error regardless but you may find that ``gcc`` has been aliased to ``clang``, which will not succeed. If this is the case, using ``homebrew`` to install gcc is recommended. Simply ``cd`` to the location you would like ``homebrew`` installed (maybe ``$HOME``, assumed below), and run the following::

   cd $HOME
   mkdir homebrew && curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
   cd $HOME/homebrew/bin
   ./brew install gcc@7
   ln -s gcc-7 gcc
   ln -s gcc-7 cc
   export PATH=$HOME/homebrew/bin:$PATH

**Make sure to add the last line of the above to your** ``~/.bashrc`` **file**. Check that ``gcc`` was installed correctly by moving to a new directory and running the following::

   $ gcc --version
   gcc (Homebrew GCC 7.5.0_4) 7.5.0

Finally, you need `XQuartz <https://www.xquartz.org/>`_ installed. 

With the above complete, enter the c code directory and install the code::

    cdc
    make install

Photpipe should now be installed! Check to make sure the installation was successful by searching for one of the binary files created during the make using something like ``which hotpants`` ::

   $ which hotpants
   /Users/jpierel/CodeBase/pipes/v20.0/photpipe/Cfiles/bin/darwin/hotpants









