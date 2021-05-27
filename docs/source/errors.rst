******
Errors
******

This is a very typical error message::

	arest@plhstproc1(KPmosaic,2,bash)% pipestats.pl -red 20140923 1
	ERROR: Parameter SW_UPDATEFIELDCENTERSFILE not found
	Execution stopped

This means that the current parameter file is missing `SW_UPDATEFIELDCENTERSFILE`. This is a safeguard to ensure that the current `pipeline.params`::

	arest@plhstproc1(KPmosaic,,bash)% echo $PIPE_PARAMS
	/ifs/cs/projects/armin1/pipe/v20.0/photpipe/config/KPmosaic1.1/LEGAL/pipeline_1.params

Try to identify one of the recent config files that have this keyword::

	arest@plhstproc1(KPmosaic,,bash)% cdconfig
	/ifs/cs/projects/armin1/pipe/v20.0/photpipe/config/KPmosaic1.1/LEGAL
	arest@plhstproc1(KPmosaic,,bash)% cd ../../
	/ifs/cs/projects/armin1/pipe/v20.0/photpipe/config
	arest@plhstproc1(KPmosaic,,bash)% grep SW_UPDATEFIELDCENTERSFILE */*/pipeline.pa*
	DECAM/DEFAULT/pipeline.params:# if SW_UPDATEFIELDCENTERSFILE=1, then the fieldcenters file
	DECAM/DEFAULT/pipeline.params:SW_UPDATEFIELDCENTERSFILE 1
	DECAM/K2/pipeline.params:# if SW_UPDATEFIELDCENTERSFILE=1, then the fieldcenters file
	DECAM/K2/pipeline.params:SW_UPDATEFIELDCENTERSFILE  1
	DECAMNOAO/LE/pipeline.params:# if SW_UPDATEFIELDCENTERSFILE=1, then the fieldcenters file
	DECAMNOAO/LE/pipeline.params:SW_UPDATEFIELDCENTERSFILE  1

I know that `DECAMNOAO` was the last one I edited, so I use `DECAMNOAO/LE/pipeline.params`::

	arest@plhstproc1(KPmosaic,,bash)% pwd
	/ifs/cs/projects/armin1/pipe/v20.0/photpipe/config
	arest@plhstproc1(KPmosaic,,bash)% compare_params.pl DECAMNOAO/LE/pipeline.params KPmosaic1.1/LEGAL/pipeline_1.params
	 
	###################################
	keys only in DECAMNOAO/LE/pipeline.params:
	###################################
	 
	ALERTWEBDIR
	C2E_EVENTLIST
	C2E_HOSTLIST
	C2E_OUTDIR
	DFB_BINSIZE
	DFIG_FIGSUFFIX
	DFIG_MK_BINNED
	DFIG_MK_IMAGE
	DFIG_MK_TMPL
	DFIG_TRIM_X
	DFIG_TRIM_Y
	DIFFCHECK_CGIACTIONS
	DIFFCHECK_CMDDIR
	DIFFCHECK_DIR
	DIFFCHECK_HTMLADDRESS
	DIFFCHECK_SINGLEBUTTPAGE
	DIFFIM_HTMLADDRESS
	ELC_ADJUSTOFFSET
	ELC_DMJD_MINUS
	ELC_DMJD_PLUS
	ELC_EVENTLIST
	ELC_MKCUTOUTSPLOTS
	ELC_MKLCSPLOTS
	ELC_OUTDIR
	ELC_SAVELCS
	ELC_SKIPIFEXISTS
	ELC_SKIP_FORCEPHOT_IFEXISTS
	ELC_STACKONLY
	ELC_USERECENTER_NMIN
	ELC_USERECENTER_XY
	ERC_DMJD_MINUS
	ERC_DMJD_PLUS
	ERC_EVENTLIST
	ERC_MKLCSPLOTS
	ERC_MKXYPLOTS
	ERC_OUTDIR
	ERC_SAVEALLSTATS
	ERC_SAVELCS
	ERC_SEARCHRAD
	ERC_USE_MJDDISC
	EWP_EVENTLIST
	EWP_EVENT_FILTERFIGSUFFIXES
	EWP_EVENT_FILTERS
	EWP_EVENT_WEBPAGE
	EWP_FIGSUMMARYTABLE_FILTERS
	EWP_FIGSUMMARYTABLE_SUFFIXES
	EWP_FIGSUMMARYTABLE_WEBPAGE
	EWP_OUTDIR
	EWP_STACKONLY
	EWP_SUMMARYTABLE_COLS
	EWP_SUMMARY_WEBPAGE
	EWP_TEMPLATETYPE
	MT_DELTA_M5SIGMA
	MT_SKIPPHOTCODETEST
	SS_FSCALE_KEYWORD
	SS_SKY_SUBTRACT_BACK_FILTERSIZE
	SW_UPDATEFIELDCENTERSFILE
	WSLE_DIR
	WSLE_FIGSUFFIX
	WSLE_HTMLTEMPLATE
	WSLE_ROOTWEBADDRESS
	WSLE_USEBINNED
	 
	###################################
	keys only in KPmosaic1.1/LEGAL/pipeline_1.params:
	###################################
	 
	DOPHOT_DIVIDEBYCHI2

A good fraction of these parameters (WSLE*, EWP*,ERC*,ELC*,C2E*) are downstream alerts pages that the main pipeline does not use, and they don't have to be added.





