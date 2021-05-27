*******************************
Photpipe blurb for publications
*******************************

We processed the data using an implementation of the photpipe pipeline
modified for DECam images. Photpipe is a robust pipeline used by
several time-domain surveys (e.g., SuperMACHO, ESSENCE, Pan-STARRS1;
see Rest et al. 2005; Rest et al. 2014), designed to perform
single-epoch image processing including image calibration (e.g., bias
subtraction, cross-talk corrections, flat-fielding), astrometric
calibration, warping and image coaddition (SWarp, Bertin et al. 2002),
and photometric calibration. Additionally, photpipe performs
difference imaging using hotpants (Alard 2000; Becker 2015) to compute
a spatially varying convolution kernel, followed by photometry on the
difference images using an implementation of DoPhot (Schechter et
al. 1993) optimized for point spread function (PSF) photometry on
difference images (Rest et al. 2005). We use photpipe to perform
initial candidate searches by specifying a required number of
spatially coincident detections over a range of time. Once candidates
are identified photpipe performs “forced” PSF photometry on the
subtracted images at the fixed coordinates of an identified candidate
in each available epoch.::

	@ARTICLE{2005ApJ...634.1103R,
   author = {{Rest}, A. and {Stubbs}, C. and {Becker}, A.~C. and {Miknaitis}, G.~A. and
    {Miceli}, A. and {Covarrubias}, R. and {Hawley}, S.~L. and {Smith}, R.~C. and
    {Suntzeff}, N.~B. and {Olsen}, K. and {Prieto}, J.~L. and {Hiriart}, R. and
    {Welch}, D.~L. and {Cook}, K.~H. and {Nikolaev}, S. and {Huber}, M. and
    {Prochtor}, G. and {Clocchiatti}, A. and {Minniti}, D. and {Garg}, A. and
    {Challis}, P. and {Keller}, S.~C. and {Schmidt}, B.~P.},
    title = "{Testing LMC Microlensing Scenarios: The Discrimination Power of the SuperMACHO Microlensing Survey}",
  journal = {\apj},
   eprint = {astro-ph/0509240},
 keywords = {Cosmology: Dark Matter, Galaxies: Halos, Galaxies: Structure, Galaxy: Structure, Cosmology: Gravitational Lensing, Galaxies: Magellanic Clouds},
     year = 2005,
    month = dec,
   volume = 634,
    pages = {1103-1115},
      doi = {10.1086/497060},
   adsurl = {http://adsabs.harvard.edu/abs/2005ApJ...634.1103R},
  adsnote = {Provided by the SAO/NASA Astrophysics Data System}
	}

	@ARTICLE{2014ApJ...795...44R,
	   author = {{Rest}, A. and {Scolnic}, D. and {Foley}, R.~J. and {Huber}, M.~E. and
	    {Chornock}, R. and {Narayan}, G. and {Tonry}, J.~L. and {Berger}, E. and
	    {Soderberg}, A.~M. and {Stubbs}, C.~W. and {Riess}, A. and {Kirshner}, R.~P. and
	    {Smartt}, S.~J. and {Schlafly}, E. and {Rodney}, S. and {Botticella}, M.~T. and
	    {Brout}, D. and {Challis}, P. and {Czekala}, I. and {Drout}, M. and
	    {Hudson}, M.~J. and {Kotak}, R. and {Leibler}, C. and {Lunnan}, R. and
	    {Marion}, G.~H. and {McCrum}, M. and {Milisavljevic}, D. and
	    {Pastorello}, A. and {Sanders}, N.~E. and {Smith}, K. and {Stafford}, E. and
	    {Thilker}, D. and {Valenti}, S. and {Wood-Vasey}, W.~M. and
	    {Zheng}, Z. and {Burgett}, W.~S. and {Chambers}, K.~C. and {Denneau}, L. and
	    {Draper}, P.~W. and {Flewelling}, H. and {Hodapp}, K.~W. and
	    {Kaiser}, N. and {Kudritzki}, R.-P. and {Magnier}, E.~A. and
	    {Metcalfe}, N. and {Price}, P.~A. and {Sweeney}, W. and {Wainscoat}, R. and
	    {Waters}, C.},
	    title = "{Cosmological Constraints from Measurements of Type Ia Supernovae Discovered during the First 1.5 yr of the Pan-STARRS1 Survey}",
	  journal = {\apj},
	archivePrefix = "arXiv",
	   eprint = {1310.3828},
	 keywords = {cosmological parameters, cosmology: observations, dark energy, supernovae: general},
	     year = 2014,
	    month = nov,
	   volume = 795,
	      eid = {44},
	    pages = {44},
	      doi = {10.1088/0004-637X/795/1/44},
	   adsurl = {http://adsabs.harvard.edu/abs/2014ApJ...795...44R},
	  adsnote = {Provided by the SAO/NASA Astrophysics Data System}
	}



	INPROCEEDINGS{Bertin02,
	   author = {{Bertin}, E. and {Mellier}, Y. and {Radovich}, M. and {Missonnier}, G. and
	    {Didelon}, P. and {Morin}, B.},
	    title = "{The TERAPIX Pipeline}",
	 keywords = {astronomy: optical, astronomy: software, pipelines: data reduction, software: package, software: development, software: data analysis, data analysis, databases, distributed processing},
	booktitle = {Astronomical Data Analysis Software and Systems XI},
	     year = 2002,
	   series = {Astronomical Society of the Pacific Conference Series},
	   volume = 281,
	   editor = {{Bohlender}, D.~A. and {Durand}, D. and {Handley}, T.~H.},
	    pages = {228},
	   adsurl = {http://adsabs.harvard.edu/abs/2002ASPC..281..228B},
	  adsnote = {Provided by the SAO/NASA Astrophysics Data System}
	}

	@ARTICLE{2000A&AS..144..363A,
	   author = {{Alard}, C.},
	    title = "{Image subtraction using a space-varying kernel}",
	  journal = {\aaps},
	 keywords = {METHODS: NUMERICAL, METHODS: STATISTICAL, STARS: VARIABLES: GENERAL, COSMOLOGY: GRAVITATIONAL LENSING},
	     year = 2000,
	    month = jun,
	   volume = 144,
	    pages = {363-370},
	      doi = {10.1051/aas:2000214},
	   adsurl = {http://adsabs.harvard.edu/abs/2000A%26AS..144..363A},
	  adsnote = {Provided by the SAO/NASA Astrophysics Data System}
	}

	@MISC{2015ascl.soft04004B,
	   author = {{Becker}, A.},
	    title = "{HOTPANTS: High Order Transform of PSF ANd Template Subtraction}",
	 keywords = {Software},
	howpublished = {Astrophysics Source Code Library},
	     year = 2015,
	    month = apr,
	archivePrefix = "ascl",
	   eprint = {1504.004},
	   adsurl = {http://adsabs.harvard.edu/abs/2015ascl.soft04004B},
	  adsnote = {Provided by the SAO/NASA Astrophysics Data System}
	}

	ARTICLE{1993PASP..105.1342S,
	   author = {{Schechter}, P.~L. and {Mateo}, M. and {Saha}, A.},
	    title = "{DOPHOT, a CCD photometry program: Description and tests}",
	  journal = {\pasp},
	 keywords = {Astronomical Photometry, Charge Coupled Devices, Computer Programs, Computerized Simulation, Data Reduction, Image Processing, Point Spread Functions, Comparison, Globular Clusters, Stellar Color, Stellar Magnitude},
	     year = 1993,
	    month = nov,
	   volume = 105,
	    pages = {1342-1353},
	      doi = {10.1086/133316},
	   adsurl = {http://adsabs.harvard.edu/abs/1993PASP..105.1342S},
	  adsnote = {Provided by the SAO/NASA Astrophysics Data System}
	}



