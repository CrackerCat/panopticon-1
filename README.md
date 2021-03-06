# Panopticon

A YARA rule performance measurement tool

## What it does 

It runs a YARA rule set against a set of samples and measures the duration of a set of cycles over that sample set. 

The number of iterations over the sample set gets evaluated automatically by providing the number of seconds each rule should be tested against the sample set (default: 15). 

## Usage

```bash
usage: panopticon.py [-h] [-f yara files [yara files ...]]
                     [-d yara files [yara files ...]] [-l logfile]
                     [-i iterations] [-s seconds]

YARA RULE PERFORMANCE TESTER

optional arguments:
  -h, --help            show this help message and exit
  -f yara files [yara files ...]
                        Path to input files (YARA rules, separated by space)
  -d yara files [yara files ...]
                        Path to input directory (YARA rules folders, separated
                        by space)
  -l logfile            Log file (default: panopticon.log)
  -i iterations         Number of iterations (default: auto)
  -s seconds            Number of seconds to spend for each rule's measurement
```

## Prerequisites

You need to find a good sample set that reflects best the use case in which you plan to use your YARA rules. 

If you have no idea where to start use ReactOS and download the [Live CD](https://reactos.org/download/). Mount the ISO and copy the contents of the folder `./reactos` into the `./samples` sub folder of panopticon. 

Plan which YARA rules you want to test. I've played around with sets of 10-100 rules. 

## Considerations 

The measurements are influenced by the system load. If you run this on your Desktop system with many different running and active processes, the results can be distorted.

The more seconds you give the measurements, the better are the results. 

## Getting Started 

1. Clone this repo and cd to it `git clone https://github.com/Neo23x0/panopticon.git && cd panopticon`
2. Install the requirements `pip3 install -r requirements.txt`
3. Place your samples into the `./samples` sub folder (see section "Prerequisites" for help on that matter) 
4. Start a measurement with `python3 panopticon.py -f your-yara-rules.yar`

## Expected Output

A successful run shows a test duration for the calibration rule (here: 7.25) and scores for all tested rules with their deviation from the calibration rule. 

```bash
(python) fubar:panopticon florian$ python3 panopticon.py -f ~/signatures/new_rules.yar 
    ___                      __  _              
   / _ \___ ____  ___  ___  / /_(_)______  ___  
  / ___/ _ `/ _ \/ _ \/ _ \/ __/ / __/ _ \/ _ \ 
 /_/   \_,_/_//_/\___/ .__/\__/_/\__/\___/_//_/ 
 by Florian Roth    /_/ v0.2.0                 
 
 YARA Rule Performance Testing
[INFO ] Processing /Users/neo/code/Workspace/signature-sources/new_rules.yar ...
[INFO ] Parsed 7 rules from /Users/neo/code/Workspace/signature-sources/new_rules.yar
[INFO ] Scanning sample set with rule: Calibration_Rule
[INFO ] Auto-evaluation calculated that the defined 10 seconds per rule could be accomplished by 12 cycles per rule over the given sample set of 1437 samples
[INFO ] Running 12 cycles over the sample set
[INFO ] Now the benchmarking begins ...
[INFO ] Scanning sample set with rule: Calibration_Rule
Duration: 7.25
[INFO ] Scanning sample set with rule: HKTL_Meterpreter_inMemory
Duration: 7.37 (0.12)
[INFO ] Scanning sample set with rule: APT_MAL_RU_Zekapab_Malware_Jul20_1
Duration: 7.90 (0.65)
[INFO ] Scanning sample set with rule: SUSP_GIF_Anomalies
Duration: 6.82 (-0.43)
[INFO ] Scanning sample set with rule: APT_MAL_Unknown_Agent_AUS_Campaign_Jul20_1
Duration: 7.58 (0.33)
[INFO ] Scanning sample set with rule: APT_MAL_PowerKatz_AUS_Campaign_Jul20_1
Duration: 7.04 (-0.21)
[INFO ] Scanning sample set with rule: HKTL_PowerKatz_Jul20_1
Duration: 9.09 (1.84)
[INFO ] Scanning sample set with rule: SUSP_Encoded_Casing_Modified_CMD
Duration: 8.60 (1.35)
```

## Improvements 

I am sure that someone finds ways to improve the measurement process. (maybe by measuring the pure cpu time instead) 

I consider this project as a proof of concept and good starting point for your own development. 

## Contact 

Follow me on [twitter](https://twitter.com/cyb3rops).