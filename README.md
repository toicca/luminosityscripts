Lightweight scripts for running brilcalc for luminosity and pileup calculations for given HLT paths.

These are written for early 2022-23 JES studies, for later developement mostly selection of run jsons and possibly pileup and normtags need to be edited.

# PILEUP
- Information from [here](https://twiki.cern.ch/twiki/bin/view/CMS/PileupJSONFileforData) and [here](https://twiki.cern.ch/twiki/bin/view/CMS/LumiRecommendationsRun3#Pileup_reweighting). Also in commands check that you use your own CERN username.
- Source brilcalc

    `export BRILWS_VERSION=3.7.4` (newer version has a bug with per bunch crossing)

    `source /cvmfs/cms-bril.cern.ch/cms-lumi-pog/brilws-docker/brilws-env`
    
- Per bunch crossing lumi

    `brilcalc lumi --normtag /cvmfs/cms-bril.cern.ch/cms-lumi-pog/Normtags/normtag_BRIL.json -i /eos/user/c/cmsdqm/www/CAF/certification/Collisions22/DCSOnly_JSONS/Cert_Collisions2022_352416_362760_eraABCDEFG_DCSOnly_TkPx.json --byls --xing --xingTr 0.1 -o /tmp/ntoikka/lumi_DCSONLY.csv`
    - "Note that the --xingTr 0.1 option will strip empty bunches and bunches with a very small luminosity from afterglow; this produces a somewhat smaller output file (which speeds up the next step) and avoids incorrectly counting afterglow in the pileup calculation, but be warned that this is only suitable for pp runs at nominal energy -- for special runs you may want to omit this option to avoid accidentally dropping real bunches"
    - Possibly add -b "STABLE BEAMS"

- PileupJSON:

    `cmsrel CMSSW_12_4_6` This version of CMSSW is required for makePileupJSON.py

    `cd CMSSW_12_4_6/src`

    `cmsenv`

    `cd -`

    `makePileupJSON.py /tmp/ntoikka/lumi_DCSONLY.csv pileupJSON.txt`

- Pileup root file

    `pileupCalc.py -i /eos/user/c/cmsdqm/www/CAF/certification/Collisions22/DCSOnly_JSONS/Cert_Collisions2022_352416_362760_eraABCDEFG_DCSOnly_TkPx.json --inputLumiJSON pileupJSON.txt --calcMode true --minBiasXsec 69200 --maxPileupBin 100 --numPileupBins 100 MyDataPileupHistogram.root`


- Other files in `/eos/user/c/cmsdqm/www/CAF/certification/Collisions22/DCSOnly_JSONS/`:
Cert_Collisions2022_352416_357900_eraABCD_DCSOnly_TkPx.json     Cert_Collisions2022_355100_362760_eraBCDEFG_13p6TeV_DCSOnly_TkPx.json  dailyDCSOnlyJSON  test
Cert_Collisions2022_352416_362760_eraABCDEFG_DCSOnly_TkPx.json  Cert_Collisions2022_Era_ABCD_DCSOnly_TkPx.json                         specialRequests
