************
Installation
************

Shell
=====

Historically, we used tcsh. However, recently, we have moved over to bash shell (astroconda is in bash). The pipeline is shell agnostic, with the only exception being the 'sourceme' files that initialize a pipeline sesssion. Therefore we recommend using bash mode going forward

Astroconda
==========

We recommend using `astroconda to install <http://astroconda.readthedocs.io/en/latest/installation.html>`_ an appropriate environment. If you are installing photpipe on Mac, then an Anaconda (or miniconda) installation is *required*. 

- If you use the WCSNONLIN stage (nonlinear WCS), you need to install the `Legacy Software Stack with Iraf <https://astroconda.readthedocs.io/en/latest/installation.html#iraf-install>`_ and python 2.7
- Otherwise we use either the `standard astroconda installation <https://astroconda.readthedocs.io/en/latest/installation.html#standard-install>`_ with the latest python 3.X, or the `STScI pipeline software stack <https://astroconda.readthedocs.io/en/latest/installation.html#pipeline-install-jump>`_
- Make sure astropy and matplotlib are available

Photpipe
========

Photpipe is on bitbucket, make an account and ask A. Rest to add you to the repository. Then you can clone it with::

   git clone https://arest@bitbucket.org/arest/photpipe.git

This command will put Photpipe into the current directory ``<pipedir>/photpipe``. 

Inititalization
===============

Each telescope/project has its own initialization 'sourceme' file. Any time one of these sourceme files are called, they overwrite the previous photpipe initialization, i.e. you can switch from one telescope/project to another. You can set up aliases to make this quick and easy::

   iraf27) plhstproc1:/home/arest $ alias ledecamnoao
   alias ledecamnoao='source  /ifs/cs/projects/armin1/pipe/v20.0/photpipe/config/DECAMNOAO/LE/LE.bash.sourceme'

All of these sourceme files look very similar. First, the instrument (PIPE_INSTRUMEN), project (PIPENAME), and version (PIPE_VERSION) are defined::

   # define project and pipeversion
   export PIPE_INSTRUMENT=DECAMNOAO
   export PIPENAME=LE
   export PIPE_VERSION=v20.0

These are used to define the two main directories, depending on which host system you are on::

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

If you are on a new system, just add once an elif statement, and you are set. From then on, you just need to call the sourceme file, and all paths and environments are set! If you use astroconda, set ``LOCALPYTHONFLAG=1``, otherwise it tries to use the python within the pipeline, which is how we did it 10 years ago in order to avoide python version issues with the code. With astroconda, we have abandoned the photpipe python installation...

All source code is in ``$PIPE_SRC=<photdir>/photpipe``, and the config files for the given instrument+projects are in::

   $PIPE_SRC/config/$PIPE_INSTRUMENT/$PIPENAME

The data will be in ``$PIPE_DATA=<datadir>/$PIPE_VERSION/$PIPE_INSTRUMENT/$PIPENAME``

There are a number of alias for convenience, which moves you around the source code and data directories::

   cdconfig
   cddata
   cdraw
   cdwork
   cdperl
   cdpy
   cdsrc

For DECam light echoes using NOAO reduced images, we set up aliases for the individual targets::

   cr, ch, kp, wb, ks, lmc, smc

alias example for LMC light echoes::

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

The first thing you must make sure of is that you have a ``gcc`` compiler installed. On linux this will likely be the case, but you can check with::

   gcc --version

If ``gcc`` is not installed, follow the directions `here <https://linuxize.com/post/how-to-install-gcc-compiler-on-ubuntu-18-04/>`_. 

On Mac, you can check for ``gcc`` in the same way as above. On newer systems, you will not get an error regardless but you may find that ``gcc`` has been aliased to ``clang``, which will not succeed. If this is the case, using ``homebrew`` to install gcc is recommended. Simply ``cd`` to the location you would like ``homebrew`` installed (maybe ``$HOME``, assumed below), and run the following::

   mkdir homebrew && curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
   cd $HOME/homebrew/bin
   brew install gcc
   ln -s gcc-11 gcc
   ln -s gcc-11 cc
   export PATH=$HOME/homebrew/bin:$PATH

This assumes you've installed gcc version 11, you can replace 11 with the version you've installed if this is not the case. It's recommended that you add the last line of the above to your ``~/.bashrc`` file, but it's not required. With the above complete, enter the c code directory and install the code::

    cdc
    make install

Photpipe should now be installed!







