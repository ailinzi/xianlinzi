# Basic information about Chia to get started

Chia is a new type of crypto currency. Here is some basic information to get started.   

A concept that is repeated a few times below-- Chia is based on capacity of pre-stored random looking data that the user creates and stores--making the blockchain consensus extremely fast/green.  It an improvement over proof of work blockchains, which rely on fast graphic cards and custum machines doing millions of calculations per second, and wasting a lot of electricity.

## How it works

You can load Chia software on Windows, Mac or Linux. The Windows version automatically starts on installation and the Mac just needs to be opened from the Applications directory - it loads and starts to sync with rest of network and blockchain. Fully syncing takes 4-6 hrs, although this increases over time. Basically you are syncing with everyone on the network, downloading the whole blockchain, which includes all transactions ever processed on the network. The chia blockchain database will be stored on your PC.

**Full Node Tab**—this shows the blockchain movement.  It shows that you are sync with the blockchain.  A copy of the blockchain is stored on your pc.  You have a live copy that is in sync with everyone else.

- Blocks — that is the blockchain working
- Connections—those are the connections to you and other users--  those connections make Chia much faster to process transactions—part of the design, and make it more secure

**Wallet tab** – you will see your Chia coins as they are won-- this area is still under development

* History—you can see the time/date you earned coins or partial coins

**Plots tab**-- This is where you create plots.   The accepted plot size starts at 101 GiB each.  Called a 32 k- 101 gig plot.    When you plot—your computer goes and creates these large 101 gig  files.  Inside are large tables (kind of like excel sheets) where each cell has a random pointer to another cell in the table. This is what the computer is doing, why it takes so long to create the plot.  It’s running formula calculations and putting the “answers” in these millions of cells.   That is why it is 101 gigs.  The expected life for a k32 plot to be eligible on mainnet is about 7 to 15 years of usage, for now. You can think of each plot as a collection of many lottery tickets, which have a chance of winning blocks.

**Farm tab**- this will show you how many plots you have created.  On the top it will show how many Chias have been farmed.  It also shows how much gigs of plots you have on the network.  If you have 2 plots of 101 gig created. Then it shows on Top left “ Total size of Plots”  .2 TIB, means you offer this much storage of formulas to the chia network.  Means you have 101 gigs x 2 = 202 gigs.  The counts are in terabytes so you need at least 10 plots to pass 1 terabyte to equal 1TIB

* Latest Block Challenges—this shows the latest challenges and signage points, which can be thought of as mini lotteries. Every 9 seconds, there is a new signage point, which means there is a new opportunity for you to check your plots and see if you've won. 

* “ Last Attempted Proof”_ — ok this is important.    There are 5 lines.  Currently I have 157 plots – so each of the lines read  0/157.
On every signage point (9 seconds), all of your plots are checked to see which ones passes the plot filter. Approximately 1/512 of all plots will pass the plot filter, so here you can see how many of your plots passed. If a plot is selected  or a 2nd plot is selected that is good news and the number changes to 1/157 or 2/157 maybe 3/157.  

It just means that plot was a close fit and the software is deciding if your hash formulas inside are a match for Chia to process the transaction and have the account earn chia.  A match does not mean money.  Once that match shows on the first line--  it will move down to lines 2-5 , as the block chain moves.   If for some reason those lines top moving--- that is another indication you are not in sync with database and need to resync—see below



\* Note

**Create a plot**

Click on green button- top right “Add a Plot”

- Starting size plot is 101 gigs.  You need a temp storage location of at least 340 gigs to create the plot.
- Chose number of plots—you can select qty to create
- “ Plot to Queue” means if (5) is selected it will plot #1, then when finished will start #2
- Plot in Parallel—means running multiple plots at same time.  Make sure you have enough temp storage for combined total.
- Under “number of plots” – click drop down.  One item to change is “buckets” select qty-128 manually.  It makes the process run slightly faster by about 20 minutes.
- #3 Select Temp Directory—select your temporary directory.  This is where plots are created. About 100 temp files will be created, then compacted to 1 plot file.    This creation grows to 330 gigs in size and then when finished will erase all files. They were temp files.  1 Plot file is created at 101 gigs.

- Its recommend to use a SSD drive or NVME drive for this work but make sure you are aware of [SSD Endurance](https://github.com/Chia-Network/chia-blockchain/wiki/SSD-Endurance).

- #3 Select Permanent directory—once the plot is created—it will go to this location to be farmed to earn chia coins.    Storage will fill quickly due to size of plot. Storage can be internal or usb connected drives.  Plan ahead—storage fills quickly
- Click create plot to start process

**When a plot is created.   There are 2 directories.  Temp storage and then permanent file.**
Over 100 files will be created all labled.tmp-. (example  agjpgoeporig.tmp file).  The temp storage will grow to approx size 340 gigs per created plot.    Once the plot is created, software creates (1) file with 101 gigs with a name and end will say .plot (example agljglhjaw[jufpoierh[wjrgpoeiwh.plot )- the software will transfer it to the permanent file and its size will be 101 gigs.  This is what will be farmed to earn chia coins. Chia software then erases all the temporary files and starts over for next plot.

**How Plots are created and 7 steps Process**

Creating a plot is time consuming.  Average 10-20 hours.  Here are the approx. 7 steps/ tables are created to create a plot.  
Table 1 is quickly created within 2 minutes.

On table 2 you will start to see it create the buckets.    Most of the programmers are telling us to select 128 buckets as it uses the least amount of ram.  Plot size is still 101 gigs.   I have noticed it also helps create the plot faster, then selecting 32 buckets or leaving the selection at 0.

Table 4 takes some time.

After 7 tables are created it starts to “backpropogate” again tables 1-7.  I think this is a double check
Then it start a compression stage of each table manually 1-7

Then it shows it’s completed- 101 gigs but nothing happens.  At this stage it is transferring your 101 gigs of plot to your permanent drive area—takes a few minutes 5-10 minutes.   It will delete all the files in your temp storage and there you go- 10 hrs. later.


Notes-
If using a Mac -- the suggestion is to use an Added SSD or NVME storage to create plots and not the primary hard drive.
The Chia software is evolving-- if for some reason a plot has to be deleted-- under status it does allow to delete the plot.  It may stop all the plotting and software to restart and all new plots created. That component is evolving with revisions

**Word definitions and Explanations**

"Proof"  is what is inside the PLOT.  Millions of "excel blocks" with formulas called proofs of space.
The chia software is designed to work on a lottery system.  A key element to winning the lottery of earning coins is that the software wants a “winner” to have a lot of plots made so it can go thru all the data quickly to answer the proof.  Proofs are the acceptance of the transaction and then move on.

The chia software gets a proof number lets say 2021.  It’s going to look thru lookup tables on the front of the plots.  Find the closest to 2021 and this is where the time comes into play-- That proof (excel block hash) comes with a time calculator and the computer figures out to run this transaction with that proof takes 2-3 seconds to do the calculations.   If another block is 2025- but has a time calculation of 5 hours it skips it over. The first one won the transaction.

What makes Chia different from block chain— is the verification called proof of space and proof of time.  Basically as the transaction first goes to the block chain the  transaction gets a timestamp in the beginning- time codes it.  Then another computer called a timelord—goes and puts a proof stamp on the transaction to say—every digit that passed thru—was all within this time slot, and similar to a gps location stamp also—so its verified and then the computer puts another time stamp on the end.  So each transaction has 3 verifications—making it extremely hard to crack or make changes, making it very secure

As a transaction goes to a computer with lot of plots—since all the plots have all the calculation formulas already done—the entire transaction is done quickly and moves on.

Bitcoin on the other hand- goes to a winning computer—and it stops and does all the transactions—using a lot of electricity and computer resources.  Here is the green part of the Chia network.  No more 1000 computers to process 1 transaction.


**Computer Hardware**

SSD drivers—many people are using SSD drives to create plots— or NVME drives.  Minimum of 1 Tb each is recommend if you want to plot more than 1 plot at a time

On an older computer you can purchase a pcie adapter card-to take NVME/SSD drive as the new temp folder.   This is internal to the PC.  Some have tried to use a usb or firewire attached NVME/ SSD with some success.  The first plot is sometimes created with 6 hrs each, but doing multiple plots slows to 8 hrs each. This pertains to creating the temp files.

One item to plan for is storage of plots as they fill storage quickly.  As more  plots are created, the discussions start to turn to terabytes of data storage not gigs of storage.  Many use usb attached external drives for storage, or internal drives or external rack storage with many drives.

**Sync Issues – look on Full Node tab**

Look at the date/time indicated compared to your PC.  If there is a 30 min difference and it has not caught up—on the windows menu—click View/Force reload.   It will take 5 minutes and should restart the cynic mode.  It will make you click your key code button again to get in.  It does not affect the plotting you are doing.

**Groups to join on Keybase chat**
\# announcements   -- most important as it tells you of new versions
\# beginner  -- read this one from the start -- many questions are answered
\# farming hardware
\# general
\# plotting-hardware
\# random
\# Testnet

*\* Important*

When first plotting --   and your first plot is created and now being farmed for chia.  On the farming tab—“Time to Win a coin”— it may say 2 days.   It may take really upto 5 days to earn the first coin or partial coin.  Its normal.  Your computer is being linked up to all the other pc’s.   As you add more plots—especially after about 50 plots, then the “time to Win” gets a little closer to estimated time

Common Answers to Questions
If power goes out what happens.   Answer-  In the chia software, the plots that currently being built  are now non operational.  When you start Chia they will be gone and need to be restarted.  One item to do is go into your temporary storage and delete all those .tmp files it created.  This is the one time it will not self delete those files if a new plot is started.  Not deleting them may cause that storage device to run out of room when it starts 1 or multiple plots.   All your existing plots are "safe" in their existing storage. 

Can I use USB 3.0  cable connected to SSD/NVME running the Temp files.  Answer- On Windows - it has not worked well- the communication speed is not fast enough, sometimes the usb turns off, then the plot is not useable. Its possible to run 1 plot, but limiting when trying to process multiple plots.  Most are installing pcie adapters to SSC/NVME and that solves the issue.  The mac has very fast communication to do the first plot, - many others are saying that they can do 2 plots but process time increases dramatically.  Technology is constantly changing so continue to do research and ask in the chat rooms

Everyone is very helpful to answer questions.  The group does ask for questions to be in the selected chat room.  Beginner questions in Beginner etc
