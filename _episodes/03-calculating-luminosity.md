---
title: "Calculating luminosity"
teaching: 15
exercises: 10
questions:
- "How do I calculate luminosity?"
- "How do I calculate luminosity for only 'good' data?"
objectives:
- "Know how to calculate luminosity and understand how to do this for only 'good' data"
keypoints:
- "One can calculate luminosity offline using the `brilcalc` tool"
- "By using the lists of validated runs one can calculate luminosity for data which is certified as 'good for physics'"
---
> ## Important note
> It is important that you use the `-c web` option when running `brilcalc`.
> This specifies that you use indirect access to BRIL servers via web cache.
> For users of CMS open data outside CERN and CMS this is the only option that will work.
{: .callout}

> ## Important note
> There is a useful help option for `brilcalc` and its commands:
> ~~~
> brilcalc --help
> ~~~
> {: .bash}
> ~~~
> usage:
>    brilcalc (-h|--help|--version)
>    brilcalc [--debug|--warn] <command> [<args>...]
>
> commands:
>   lumi Luminosity
>   beam Beam
>   trg Trigger configurations
> See 'brilcalc <command> --help' for more information on a specific command.
> ~~~
> {: .output}
> Of particular use will be the `brilcalc lumi` command.
{: .callout}

## Calculating luminosity for a particular run
Let's calculate the integrated luminosity for a particular run found in the CMS Open Data.
~~~
brilcalc lumi -c web -r 160431
~~~
{: .bash}
~~~
#Data tag : 19v3 , Norm tag: onlineresult
+-------------+-------------------+-----+------+----------------+----------------+
| run:fill    | time              | nls | ncms | delivered(/ub) | recorded(/ub)  |
+-------------+-------------------+-----+------+----------------+----------------+
| 160431:1615 | 03/14/11 03:08:07 | 243 | 218  | 8781.334977241 | 7879.289611261 |
+-------------+-------------------+-----+------+----------------+----------------+
#Summary:
+-------+------+-----+------+-------------------+------------------+
| nfill | nrun | nls | ncms | totdelivered(/ub) | totrecorded(/ub) |
+-------+------+-----+------+-------------------+------------------+
| 1     | 1    | 243 | 218  | 8781.334977241    | 7879.289611261   |
+-------+------+-----+------+-------------------+------------------+
~~~
{: .output}


## Luminosity and validated data
During data taking only those runs in which all the subdetectors, triggers, luminosity, and physics objects are found to be performing as expected
are certified as "good for physics". For the physics analyst the list of certified luminosity sections in these runs is provided in the form of a
JSON (JavaScript Object Notation) file. To ensure that we are calculating the luminosity for certified data one must fetch these files and pass them to `brilcalc` on the command line.
First let's fetch the JSON file for 2011 data:
~~~
wget http://opendata.cern.ch/record/1001/files/Cert_160404-180252_7TeV_ReRecoNov08_Collisions11_JSON.txt
~~~
{: .bash}
Then pass this file to `brilcalc` on the command line and pipe it to a text file `2011lumi.txt`:
~~~
brilcalc lumi -c web -i Cert_160404-180252_7TeV_ReRecoNov08_Collisions11_JSON.txt > 2011lumi.txt
~~~
{: .bash}
The beginning of the output in the file should look like this:
~~~
#Data tag : v1 , Norm tag: onlineresult
+-------------+-------------------+------+------+----------------+---------------+
| run:fill    | time              | nls  | ncms | delivered(/ub) | recorded(/ub) |
+-------------+-------------------+------+------+----------------+---------------+
| 160431:1615 | 03/14/11 03:14:43 | 200  | 200  | 7366.884       | 7366.875      |
| 160577:1622 | 03/16/11 01:44:38 | 53   | 53   | 1719.888       | 1631.386      |
| 160578:1622 | 03/16/11 02:20:03 | 175  | 175  | 5201.105       | 4952.155      |
| 160871:1636 | 03/19/11 02:43:57 | 141  | 141  | 109739.586     | 106354.581    |
| 160872:1636 | 03/19/11 03:50:05 | 38   | 38   | 28346.738      | 23977.109     |
| 160873:1636 | 03/19/11 04:20:20 | 147  | 147  | 104914.534     | 103031.004    |
~~~
{: .output}
With a summary at the end of the file:
~~~
#Summary:
+-------+------+--------+--------+-------------------+------------------+
| nfill | nrun | nls    | ncms   | totdelivered(/ub) | totrecorded(/ub) |
+-------+------+--------+--------+-------------------+------------------+
| 194   | 470  | 159872 | 159872 | 5253793895.474    | 5100199528.945   |
+-------+------+--------+--------+-------------------+------------------+
~~~
{: .output}
You may notice at the end of the output after the summary a list of runs and lumi sections. These are runs and lumi sections that are listed in the JSON quality file but do not have any luminosity values corresponding to them. These correspond to sections that are left-overs at the end of a run, which where still tagged as STABLE RUN, but actually did not provide any luminosity. These are safe to ignore as they do not contain any events.

> ## Note
> Information on the validated runs for CMS Open Data can be found on the [CERN Open Data Portal](http://opendata.cern.ch/)
> using [this search](http://opendata.cern.ch/search?page=1&size=20&q=&experiment=CMS&subtype=Validation&type=Supplementaries&type=Environment).
{: .callout}

> ## Validated data JSON files
> The files used here are the same JSON files used in the `python` configuration files as in the [CMSSW lesson](https://cms-opendata-workshop.github.io/workshop-lesson-cmssw/). They are included in the `python` like so:
> ~~~
> import FWCore.PythonUtilities.LumiList as LumiList
> goodJSON = 'Cert_160404-180252_7TeV_ReRecoNov08_Collisions11_JSON.txt'
> myLumis = LumiList.LumiList(filename = goodJSON).getCMSSWString().split(',')
>
> # your process.source defined
>
> process.source.lumisToProcess = cms.untracked.VLuminosityBlockRange()
> process.source.lumisToProcess.extend(myLumis)
> ~~~
> {: .language-python}
> What's in the file? Example content looks like this:
> ~~~
> {"160431": [[19, 218]], "160577": [[254, 306]], "160578": [[6, 53], [274, 400]]
> ~~~
> {: .output}
> Which says, for example, that luminosity sections 19-218 in run 160431 are certified as valid and "good for physics".
{: .callout}

> ## Luminosity over a range of runs
> Using the `brilcalc lumi --help` and the information found from [this page](http://opendata.web.cern.ch/record/1001) on the CERN Open Data Portal, how does one calculate the
> integrated luminosity for certified lumi sections for RunA of the 2011 data release?
>> ## Solution
>> RunA of 2011 proton-proton data comprises runs 160431 to 173692 (inclusive) so to calculate the integrated luminosity for this era run the command (where we pipe the output to a text file):
>> ~~~
>>> brilcalc lumi -c web --begin 160431 --end 173692 -i Cert_160404-180252_7TeV_ReRecoNov08_Collisions11_JSON.txt > RunA2011lumi.txt
>> ~~~
>> {: .bash}
>> The range of runs for a primary dataset can be found from the primary dataset record page, for example [here](http://opendata.cern.ch/record/272) for the `DoubleMu` primary dataset from RunB of 2011.
> {: .solution}
{: .challenge}

> ## Luminosity separated by luminosity sections
> Calculate the luminosity for validated runs and luminosity sections, separated by luminosity sections,
> outputting your results to a `csv` (comma-separated-variable) file.
>
> Hint: check out the options in `brilcalc lumi --help`
>> ## Solution
>> ~~~
>>> brilcalc lumi --byls --output-style csv -c web -i Cert_160404-180252_7TeV_ReRecoNov08_Collisions11_JSON.txt > my2011lumibyls.csv
>> ~~~
>> {: .bash}
>> The contents of your `csv` file will appear as below:
>> ~~~
>>  #Data tag : v1 , Norm tag: None
>> #run:fill,ls,time,beamstatus,E(GeV),delivered(/ub),recorded(/ub),avgpu,source
>> 160431:1615,19:19,03/14/11 03:14:43,STABLE BEAMS,3500,39.312,39.312,3.4,hfoc
>> 160431:1615,20:20,03/14/11 03:15:07,STABLE BEAMS,3500,39.368,39.368,3.4,hfoc
>> 160431:1615,21:21,03/14/11 03:15:30,STABLE BEAMS,3500,39.305,39.305,3.4,hfoc
>> 160431:1615,22:22,03/14/11 03:15:53,STABLE BEAMS,3500,35.843,35.843,3.1,hfoc
>> 160431:1615,23:23,03/14/11 03:16:17,STABLE BEAMS,3500,37.093,37.093,3.2,hfoc
>> 160431:1615,24:24,03/14/11 03:16:40,STABLE BEAMS,3500,39.620,39.620,3.4,hfoc
>> 160431:1615,25:25,03/14/11 03:17:03,STABLE BEAMS,3500,34.396,34.396,3.0,hfoc
>> 160431:1615,26:26,03/14/11 03:17:27,STABLE BEAMS,3500,39.874,39.874,3.4,hfoc
>> ~~~
>> {: .output}
>> You may notice the `hfoc` in the `source` field. This indicates that the Hadron Forward Zero Counting value for luminosity was used.
>> When available, more precise values can be obtained with the Pixel Luminosity Telescope, given by `pxl`. These values are preferred. You can select them
>> using the `brilcalc lumi --type` option. For example: `brilcalc lumi -c web -r 208686 --type pxl`.
> {: .solution}
{: .challenge}

 
## Calculating luminosity for Run 2
Luminosity information for Run 2 open data is available at [CMS luminosity information, for 2015 CMS open data](https://opendata.cern.ch/record/1055).
  
  
> ## Further Information
> More information can be found in the [Reference](../reference.html) section, and more information on how to conduct common queries is in the [CMS Guide on how to calculate luminosity](https://opendata.cern.ch/docs/cms-guide-luminosity-calculation).
{: .callout}

{% include links.md %}
