# FindTheMag
**IMPORTANT: SECURITY UPDATE APR 2023. If you used FindTheMag to enable RPC on your Gridcoin client, it set an incorrect flag in your gridcoinresearch.conf file. Edit this file to remove the line starting with allowip. This line allowed other machines on your LAN (or the wider internet if your machine is not behind a NAT/firewall) to talk to your wallet's RPC port. It did NOT enable access to your wallet since commands were only able to be issued from localhost, but that port never should have been open in the first place. My apologies for the mixup. In my defense, the documentation for this on the Gridcoin site was really unintuitive and I will be putting in a pull request to fix this asap.**

FindTheMag is a powerful utility which prints statistics from your BOINC client and offers suggestions to optimize your crunching. If you ask it to, it will also control BOINC to crunch projects according to your
preferences. It even has options to only crunch according to profitability and/or temperature.

You can group BOINC projects into two groups: 
 * _Preferred_ projects are ones which you want to crunch regardless of how much GRC they get you. 
 * _Mining_ projects are projects which you are willing to crunch, but only if they get you the maximum 
amount of GRC possible. In the event that your preferred project(s) happen to be the most efficient to mine, FindTheMag will 
assign all project weight to them.

FindTheMag uses your own BOINC client's stats to determine which projects get the most credit per hour on your machine. 
It then figures out which projects earn you the most Gridcoin. [Quickmag](http://quickmag.ml/cgi-bin/script.cgi) is a similar tool which will estimate the most profitable projects for your hardware based on the performance of other crunchers with similar hardware. It's useful and elegant. You should check it out.

<b>If you find this tool useful</b>, consider sidestaking or donating
some GRC to me at RzUgcntbFm8PeSJpauk6a44qbtu92dpw3K

This tool's accuracy improves the longer you have been running BOINC. It will automatically set all Gridcoin-approved 
projects to a weight of one (if you have BOINC control enabled and are attached to all projects) so they will continue to use a small portion of your processing power (total of 1% of all crunching time), each time increasing the accuracy of the tool.

It is strongly suggested to have the Gridcoin wallet running on your machine (fully synced). If you do not have it installed, it will try to fetch statistics from gridcoinstats.eu. This is less reliable in case of network issues and is limited to once per day. 
The wallet doesn't need any coins in it, just an up-to-date copy of the blockchain. Your machine also requires **python 3.8 or higher**,
though earlier versions may work they are unsupported. If you just installed BOINC, this tool takes about 48 hours to start working as we have to wait for projects to grant you credit for your work. Though really a week's worth of data is about the minimum you'd need to get useful information from this tool.

Special thanks to <a href="https://github.com/nielstron/pyboinc">PyBOINC</a> which this program uses extensively

<b>If you only want to use this tool to get stats and suggested project weights but not control BOINC directly:</b>
 * If you use an account manager like BAM:
   * Put each host into its own host group
   * Assign weight to projects in the host group
   * Both [Arikado pool](https://grc.arikado.ru/) and [GRCPool](http://grcpool.com) can set project weight on a per-host basis.


 * If you manage your BOINC client locally
   * There is no way to set project weight outside of logging in to each project manually and setting it there. 
   * You can still use this tool to find out the mag/hr of various projects and "no new tasks" projects you have no interest in crunching either because of low mag/hr or because they are not your preferred projects. This can be done locally on your BOINC client.

<b>If you use this utility to control your BOINC crunching:</b>
 * **5% of your processing power will be donated to development**. I hope you will see that this is only a small portion of the efficiency gain that this tool brings you. And that this tool represents many hundreds of hours of work. You may set a custom dev fee in the config, but it cannot be smaller than 1%. This donated processing power is accomplished by crunching projects under the developer's login. Don't worry, all crunching power still goes towards science!
 * You will see most of your project set to "no new tasks" except one, this is how FTM controls BOINC. Your "highest priority project" determined by expected crunch time vs actual (determined by project weight) will have tasks requested, and FTM will continue requesting tasks for each project until the work cache is full.
 * You can continue to use tools like BOINC Manager or BOINCTUI to monitor your crunching
 * You can stop the script at any time and regain manual control of your BOINC client, it will be reset to the settings which existed before FTM was run
 * It will respect all pre-existing BOINC settings you have such as when to crunch or use the network. All this tool really does is set projects to "no new tasks" and request work from projects.
 * It is **strongly suggested** to have at least 10GB of free space for BOINC and that you allow BOINC to download at least 1 day of "extra" work. This helps project servers even out load and prevents this tool from getting confused. If you have a very small work cache and have profitability control enabled, you may occasionally run dry on work which is no fun. Otherwise, everything should be fine.

<b>How dev fee/crunching for dev works</b>
 * The dev fee by design does not kick in until you already have 100 hours of FTM usage under your belt. So it does come with a free trial! The revenue of the dev fee also supports Gridcoinstats which graciously hosts the stats needed for this tool to run, and to the Gridcoin foundation/community wallet.
 * The dev fee portion is skipped if you have elected to sidestake to the dev instead. This is suggested as it is simpler and will save you the disk space of a secondary BOINC install
 * Crunching for dev works by making a seperate BOINC install contained in the FindTheMag folder, set to take up no more than 10GB space. This BOINC install is attached to projects using the developer's username.
 * The secondary BOINC installed used to crunch for dev will respect the same settings your normal BOINC install is set to (whether or not to use GPU, # of cores, etc)

## Quickstart instructions
<b>For all platforms</b>: Open the `config.py` file in a text editor and change the settings you want.
<h4>Windows</h4>

 - Download the latest version of python from python.org. Enable the "install to system path" option while installing.
 - In command prompt, run the command `python -m pip install --upgrade pip`
 - In command prompt, run the command `pip install -r "C:\Users\user\Downloads\FindTheMag-master\requirements.txt"` (or wherever you saved this tool)
 - Double-click on main.py or run `python "C:\path\to\main.py"` from command prompt. We suggest the second method as it will display errors if the program exits unexpectedly

<h4>Linux</h4>

 - Open a terminal and go to the folder you downloaded this tool into using `cd /home/user/Downloads/FindTheMag` or wherever you put it
 - Run `pip3 install -r requirements.txt` (Note that you need pip installed, if you don't have it, you might need to run a `sudo apt install python3-pip`)
 - If you will be using FTM to control BOINC's crunching, give it permissions to edit BOINC settings with `sudo chown $USER /var/lib/boinc/global_prefs_override.xml`
 - Run `python3 main.py`

<h4>OS X</h4>

 - <b>Important</b> - If you want this tool to *control* BOINC on OS X: You must have the Gridcoin wallet installed and sidestaking to developer (script will offer to set this up for you) as the "crunching for dev" option does not work on OS X due to BOINC <a href="https://github.com/BOINC/boinc/issues/5069">not supporting</a> seperated installs on this platform.
 - Open a terminal and go to the folder you downloaded this tool into using `cd "/home/user/Downloads/FindTheMag"` or wherever you put it
 - Run `pip3 install -r requirements.txt`
 - Run `python3 main.py`

### Notes on profitability:
This tool can be configured to only crunch when "profitable" based on Gridcoin's latest price. In order for this calculation to work correctly, you must input 
your local electric rates and rig wattage information into the file named `config.py`. 

When you first run the tool, it will dedicate a minimum of 10 hours (or 5 WUs whichever comes first) of crunching to each project in "benchmarking"
mode. This is crucial to determining the profitability of a project since the amount of credit you earn on
differing hardware can vary significantly. Once each project has finished benchmarking, if it determines
the project is not profitable, it will stop requesting new work. Once six months have passed, it will again
request a new batch of work from the project to see if credit assignment, app efficiency, etc. has changed.
This prevents you ending up in a situation where all project have been determined to be "unprofitable" but
it never checks again to see if this is still true. 

### FAQ:
<b>I'm not getting as many coins as I expect or I haven't received my rewards?</b>

Consult the official Gridcoin help channels. If you are a solo miner, be sure you
have enabled GDPR export on projects which require it. See https://gridcoin.us/guides/whitelist.htm

<b>I'm getting some error while running the tool or need to contact you</b>

Open an issue here on github or email makeasnek{at}gmail.com.

<b>My BOINC client doesn't seem to be crunching according to the weight I assigned?</b>

Changing your resource share doesn't ensure that your machine will immediately start crunching according to those
resource shares, instead your BOINC client will gradually "catch up". For example, let's say you have a project which
you have recently assigned a weight of "one" to while your other projects add up to 100. Even though 1 is a very small
weight, if you have been crunching for six months and the project previously had a weight of zero, BOINC may fetch a
bunch of work-units to catch up so that it will have crunched that amount of weight over that entire time period. So for
that day, it may appear as if the project has a weight of 1000, but that's because it's average weight is still <1 and
it needs to catch up. You can configure the size of this window in the config file if you are directly controlling BOINC with FTM.

<b>How does it determine the most profitable project?</b>

Each project assigns credit for your work, and BOINC keeps track of how long that work took. The formula to figure out
the profitability of a project is below:

First it calculates the mag ratio: 

mag ratio =  project total magnitude / average total project RAC from last 30 days

Then you can get profitability (mag/hr): 

profitability = (total credits earned / time those credits took) * mag ratio 

<b>Why is it setting all these projects weights to 1?</b>

So that it can gradually accumulate enough stats to determine the profitability of these projects, and so those stats
continue to update as projects change how their applications and credit assignment work. Note that 1 = .1% of your machine's processing time since the total weight is 1000 not 100.

<b>What if there's a project I <i>never</i> want to crunch?</b>

If you have a grudge against a particular project, you can add it to the ignore list in the config file.

<b>What if there is more than one "most profitable" project?</b>

If they are within 10% of each other, project weight will be split evenly between them. This insures crunching will
continue even if one project runs out of work.

<b>What if I only want to crunch the most profitable project within a subset of projects, for example "only crunch the most profitable medical research project?"</b>

- Method 1: Only attach to projects in your desired category. Leave "preferred" projects blank
- Method 2: Attach to as many projects of any kind, set to "ignore" all projects outside set category

<b>Why don't you just say "crunch this project and if there's no work available, crunch this other project?"</b>

This isn't possible, because the BOINC client doesn't have this ability, nor the ability to have projects be in groups. It's open source
though so feel free to contribute to the BOINC project. This is actually how the BOINC control function of this tool
works though, so if you use FTM to directly control BOINC, it will always prioritize the "most wanted" project depending on whether most wanted is most profitable,
in your preferred projects list, etc.

<b>What about CPUs or GPUs? Or different applications?</b>

This tool doesn't know about GPUs or different apps, it just calculates an average credits/hour over all work units sent
to you by the project. It is generally adviseable to disable CPU tasks in your preferences on the project site 
if you are able to crunch GPU workunits since they generally grant more credit per watt and more credit per hour. But
ultimate the efficiency of crunching GPU vs CPU is up to the project and how they make their WUs. If you are using this
tool to directly control BOINC, it will work to make sure your CPU and GPU are always as fully utilized as possible.

<b>What about if my hardware changes? If I add/remove a CPU/GPU?</b>

If you add hardware, FTM's estimate of each project's profitability will gradually approach the "new" number. If you remove hardware, there is a chance it never will, particularly if you have removed a GPU and not replaced it with another. This is because FTM will keep trying to request from GPU-only projects, get no new WUs, and therefore never update its stats. For this reason, if you remove a GPU, I would suggest re-installing BOINC and FTM to start fresh with your stats.

<b>How often should I re-check my stats with this?</b>

It's up to you, but it uses a 30-day average to calculate RAC:MAG ratios, so running it very frequently won't get you
much benefit. It's lightweight and doesn't hammer the BOINC project servers though, so feel free to run it as often as
you want!

<b>How do I interpret the table printed to console?</b>

Most columns are fairly self-explanatory, here's the ones that might be a bit confusing:
 - HOURSOFF - Hours we are off from our target # of hours crunching this project based on given project weight. Positive numbers mean we have crunched this project more than desired, negative numbers means we need to crunch this project more. Project w/ the highest negative number is assigned highest priority and the list proceeds from there.
 - WTIME/CPUTIME - Wall time is how long a WU takes to complete, CPU time is how much time the CPU logs doing it. The same WU will take different amount of time depending on clock speed, what other work is being processed, etc. Walltime is used for all calculations 
 - USD/HR R/P - USD earned per hour crunching this project, followed by gross revenue and profit.
 - WEIGHT - Crunching weight assigned to project. Weight of 1 means it will only be occasionally assigned weight for benchmarking according to user prefs
 - R-WTIME - Recent walltime within window defined in config.py (default 60 days). This is time used to calculate difference between "intended" crunching hours and actual crunching hours to set project priority.
 - ATIME - Average walltime per workunit
 - ACTIME - Average CPU time per workunit
 - ACPT - Average credit per task

<b>What are you going to name the pony?</b>

I don't know, current candidates are Jeffrey and Stargazer.

<b>What does "The following projects do not have enough stats to be calculated accurately" mean?</b>

It means you have completed less than 10 tasks for this project. At ten tasks, under ideal circumstances, we could get within 10% of the correct "credits per hour" estimation which is needed to determine mag per hour. So if less than 10 WUs have been completed, we don't even try to make an estimate.

<b>What if I can't run the Gridcoin wallet on my machine? For example, due to space limitations on my Raspberry Pi?</b>

As of FindTheMag 2.0, it will fetch the latest stats from Gridcoinstats.eu. You can also generate stats from non-local BOINC clients by gathering their log files and pointing the tool at them. You can copy your BOINC data directory to a machine that has the wallet running and point the script to it. Note that you don't need to copy any subfolders in the BOINC directory, just the root directory and files directly beneath it.

<h3>Legal</h3>

- This software comes with no warranty and is provided as-is. Be wise when running software from some random github account. It may calculate suggested weights wrong. It may crash your computer, it may even steal all your GRC. By using it, you agree to hold the developers harmless for any damage it may cause whether through negligence, accident, or malice to the fullest extent legally possible. You also agree to allow yourself to have a wonderful day today or you are not allowed to use this software.
- If you submit any changes or pull requests to this repository or its developer, you agree to have the code ownership transferred to the repository owner and licensed under the same license as the other code in the repository is licensed under.
- This software is produced independently of the Gridcoin and BOINC projects without their approval or endorsement.
- This software is released under the license specified in the LICENSE file. It also incorporates components of other software, whose licenses are noted where imported in the directory structure.

<h3>Privacy</h3>

- This software will make web requests to several websites listing Gridcoin's current price. These requests include your IP address but do not include any other information outside of what loading a standard web page would include.
- If you do not have the Gridcoin wallet installed and accessible, this software will make a web request to Gridcoinstats.eu or any other Gridcoin stats provider. Again, these requests include your IP address but do not include any other information outside of what loading a standard web page would include.
- This program also checks this Github repository for updates. Again, these requests include your IP address but do not include any other information outside of what loading a standard web page would include.
- After 100 hours of crunching using this tool, it will start using a set % of processing power in "dev mode" to mine GRC for the developer. This connects to BOINC projects under the dev account. These projects publish statistics about which hosts are used under the dev's account which includes your hostname, a list of projects crunched, how much crunching has been done, and some system hardware specs. This inherently will reveal to others that you use the FindTheMag tool if you have a unique hostname
- No other information is provided to the developer or any third parties. All stats gathered by the tool are only used locally.