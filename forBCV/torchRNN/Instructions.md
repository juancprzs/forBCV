## Instructions for torch-RNN
This is some code we found about LSTMs, written by jcjohnson (thank you jcjohnson!), whose git can be found at https://github.com/jcjohnson/torch-rnn, so, first of all:

```bash
git clone https://github.com/jcjohnson/torch-rnn
cd torch-rnn/
```

He has quite come instructions in his git, so we'll make a simple summary of that, regarding the code he wrote (in torch) and how we can use it with different inputs to produce different outputs for our purposes.
In this case, the network works with language, so the input is always text.

*The most remarkable thing about all of this is that the network starts from 0, meaning that everything you'll see was learned from scratch. Pay attention to the fact that it divides what it writes into words, the usage of markers like '.',',' (and the apostrophe itself) and the syntax of the language. Depending on what we feed it, it can learn what a play looks like, a wikipedia article (even with the citations and everything) or even code!*

The next is, almost word by word, taken from his github:
## System setup
You'll need to install the header files for Python 2.7 and the HDF5 library. On Ubuntu you should be able to install
like this:

```bash
sudo apt-get -y install python2.7-dev
sudo apt-get install libhdf5-dev
```

## Python setup
The preprocessing script is written in Python 2.7; its dependencies are in the file `requirements.txt`.
You can install these dependencies like this:

```bash
pip install -r requirements.txt  # Install Python dependencies
```

## Install torch (taken from http://torch.ch/docs/getting-started.html#_) if you are OSX user, go to the 'OSX Installation' section 
```
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; bash install-deps;
./install.sh
```

and try this to verify installation
```
luarocks install image
luarocks list
```

After installing torch, you can install / update these packages by running the following:

```bash
# Install most things using luarocks
luarocks install torch
luarocks install nn
luarocks install optim
luarocks install lua-cjson

# We need to install torch-hdf5 from GitHub
git clone https://github.com/deepmind/torch-hdf5
cd torch-hdf5
luarocks make hdf5-0-0.rockspec
```

### CUDA support (Optional) -> I did not test this since I have OSX :(
To enable GPU acceleration with CUDA, you'll need to install CUDA 6.5 or higher and the following Lua packages:
- [torch/cutorch](https://github.com/torch/cutorch)
- [torch/cunn](https://github.com/torch/cunn)

You can install / update them by running:

```bash
luarocks install cutorch
luarocks install cunn
```
## OSX Installation
Jeff Thompson has written a very detailed installation guide for OSX that you [can find here](http://www.jeffreythompson.org/blog/2016/03/25/torch-rnn-mac-install/).


# Using this thing
We have to:
* Pre-process data
* Train the model

and then we can *Take samples from the model*

## Pre-process
You have to be on the ~/torch/torch-rnn folder and then we can run
```bash
python scripts/preprocess.py \
  --input_txt ./vernedata/MERGED.txt \
  --output_h5 ./vernedata/MERGED.h5 \
  --output_json ./vernedata/MERGED.json
```
This will take the text file in --input_txt and produce two files: an h5 and a json file, which are needed by the network to train.

## Train the model
Using `train.lua` script. Run it like this:
```bash
time th train.lua -input_h5 ./vernedata/MERGED.h5 -input_json ./vernedata/MERGED.json -print_every 100 -checkpoint_name checkPython/BCVcheckpoint -checkpoint_every 200
```
This, by default, runs with GPU, if you don't have a gpu, you *must* use another flag: `-gpu -1`, so that it runs on CPU-only mode. `time` is to, at the end, know how much time it took for the training process. The `-print_every` flag is for the verbose output and the `-checkpoint_name` flag creates a checkpoint (.t7) with the name we give that saves the weights of the net every `-checkpoint_every` iterations. (A checkpoint is about 10 MB so it's not that much).

## Sample things!
After training a model, you can generate new text by sampling from it using the script `sample.lua`. Run it like this:

```bash
th sample.lua -checkpoint ./checkVerne/checkpoint_52350.t7 -length 2000
```
Again, if you don't have a gpu so should use `-gpu -1`. A parameter to handle here is the 'temperature' (`-temperature X`, where X is between 0 and 1) of the output, which is kind of a certainty for the output that we impose over the net. Then, a higuer temperature (closer to 1) will give the net some 'room' for trying new things, meaning that it doesn't have to be so sure of things, while a lower temperature asks it to be really sure about its output, so it's kind of repetitive.  

# The actual training of this thing
First of all, we provide data for training the net in three 'contexts': Shakespeare plays, some Julio Verne's books, and python code! 

The data (.txt files) can be found at `shakespearedata/`, `vernedata/` and `pythondata/`. Nevertheless, I already trained it for you (it took several hours in my machine, but maybe it takes much much less in yours with a GPU), and the checkpoint files, which are the ones you need to actually generate the samples, can be found at `checkShakespeare`, `checkVerne` and `checkPython`. Given that the 'datasets' are of different size, it took different number of iterations, so:
* Shakespeare: 17800
* Verne: 52350
* Python: 80750

If you want the 'best' results from the sample, you should run the `sample.lua` with the `checkpoint_X.t7` with the highest number of the experiment.

## Some results
To illustrate the learning of these things, we'll show the output of the net at various iterations. For simplicity we'll use the net that learned english from the books from Verne, since his english is understandable. Next, a list of iteration;code;output:
* Iteration 100;
```bash 
th sample.lua -checkpoint checkVerneEvery100/checkpoint_100.t7 -length 1000 -gpu -1
```
WT,l
ag“ , sennl the rrenaovey frbegheagrepu﻿l
 s iny. 
as
  shirhaas, udenrenld wof wo hes wanry to min the ny
rhas xea he me, 8in anreledn fotg," wmt
tereett
 hot etsu"s orrit] izr, cin tats Ot n3
 thhe hed amed tosd rg aniius fbauiFs ibet ravets th pose uCbed ond sarnqchewnh  saco frand oicinr
 ofd
aOt;cwe numaweQ waf"ns sawe thulive t
mbigarrlipge
1od iergeu fto whas bomelcy Mhe to ewor Theucy arisn cweimXeint, uiatr ud swepd womawe f oan, fa nisd E

cThu citoart
th bo he th fr wyer
hen chot coos apic dib thinr Wentn.esadeg amurlon itk.

FIle huod b0at keund
”rwmplecg mud Tuj porj t
iche than fsehelf. ?"con, sant
t
nt apim
 posd hpeet peghsirE cined cfey many nfecCe ti somcleerr dinmat pacreglesunlerd
thad in ceas xilerm she syecing pofrnt poded.
ag incynon.s her. eced .h oos c'hi
fes.
 ah-
 rcf nyt fedem rpetho
welt
sirdidiod it" tndrevetrimot p the ttacon” the neriwd te nicer ans npecot ams  ^einif

* Iteration 200;
```bash 
th sample.lua -checkpoint checkVerneEvery100/checkpoint_200.t7 -length 1000 -gpu -1
```
£_b!o nigluson plasille
s ancas natheve conen gimes ta.”

1oncly kut de£ft blame mang gabegerles in olrfistt the sleuaglley.
n
The
fonce ttuned chacfert of hst"s
I thomed. l0t yog this the thave cory a perut cacwrrouscetsuvoll ith, Saal, in lolralasey id nows ontthe< monnenred sioe famlarle gwasthe coom La
depomerterent polisanged datted watert fo rate nhe I the “noipe tlosbeafumiched hejetin ejerpjasbere to nabsued suttend cepbouger cdoute patur nouch Ihine, the and als prwef0 then beoxg
Sokptime
inssed ake Mippatey roalbbastamire parjeng plow whale tith haclaticoon
thor the it boich actiy amed twhale the wet timers woM in rivent hou hivars axtrerlleb rigighat Bat. Thust samaty.

Vasded hat
”in soft thy facty oniste sfuod toy soned his in if coptercy whom cinend hizam cinl of thor
on to tadin of of the to srimit all asen; tham thionishever oid?
”Icundes an, this ta critith.
whou? thered Pylacueng gidy fint hit beile proudy recaon ain Eofsakace sericermere
on the P

* Iteration 1000;
```bash 
th sample.lua -checkpoint checkVerneEvery100/checkpoint_1000.t7 -length 1000 -gpu -1
```
O@C Cast Jlew, the could now a on ow med they thea day, bescakate in the sing,” buts dest of the sagement; and been a, withous more in from
were now a
dogn over
 My a casberged to the
corpose san
aning the bettery., I peridenity.
Digate afflow to is his go, reasted had in pheslesssabten afters, had twince wal, but my was, has stelling edwel. Hone, in the paty clater, and no receemore.  There heat curnoubs, been the preventry, roge. Lifing
thruem, pertouse and.
This my
went, bo-rects of hay three fired of a seg, in with reavided in Islonk had of beap on that nop!"

“At glomctmed, and well two the pieved was errors of is lajed is!"

"Aying a mittmed survanding when the us,
my
agate, and sitation of thim. Able, and Herbert, foresan and in the great Hownad to
rihe vanizen a have uson sauptian was from the crarojest to more--wa oterant of the forutn, “the searted the you horshementofungades walch,” repliongo theresten of the became, rushed eadic theas, was the

* Iteration 10000;
```bash
th sample.lua -checkpoint checkVerne/checkpoint_10000.t7 -length 1000 -gpu -1
```
'.  By the paight of
Sir.  Exeed
was carred
without sufficient as it as at the glowlication resember her in Auttakoua in a stream.

It was much
conlighted, but we eBolion. At a fatice.  I begun about it would not trapped and
a day was living a remark passently, so and through the head sercule yet we have the crater down there.”

“That?" returned the engineer, “if any decton, Neb pleared that then
the warric of wait by wholly the operation of the winds; but _you jumping Passepartout Professor, "Hause?"

"And wish, this toil, and with
as the sloves of cemercy into his feely a mile in some lubblers, recotrois runted a vessing Lossitatine received that or considerations and honor of the open which studden having tails his table since to youst, and nature regions. The room back.  Mand.

The end from had know they tone."

"The pathest in them, more through o'clock.

"Perheast, rus, the creflly in hunters as after viaries, successive; they wints formed to say and islancise;	

* Iteration 20000
```bash
th sample.lua -checkpoint checkVerne/checkpoint_20000.t7 -length 1000 -gpu -1
```
VEUDIRqU1$$$&D^$&m^NEFAN CRECTE PRIEMCEIR,

   XXIS     IN WHICH PASSEPARTOUR, Bl. Cllumbs any bay, would know now him recognized by enough.  If his beginner
hunger alsoon than every
leaving her sonagers!

"And brind live that there, when been Irrohamined in a street as copped to dead them elobing waves.
Never you send that, which is the
curse from the princy still had noted
anyted of him.  It was proved that warm, it will I have to writfled the matter.  Aladous the most.
They much resoinent miltterns, londed
grave still as they deliessed.

At lasts."

But at that time no
open
a sort more at the great publisurinal former more for the manufacture; soon after.”

“Duct, your known wheno gave long never gound. Barbicane, Neb and Fix would just the dead; his master himself,-little of companish
upon ideaure, from the projectile, dark, with them,
produced him but or excition, awore.

"Certains’ remarked. He would numbory.

When the sufferest, was counted the day of th

* Iteration 52350
```bash
th sample.lua -checkpoint checkVerne/checkpoint_52350.t7 -length 1000 -gpu -1
```

CHAPTER IV


NHAST WETLERY


Phileas Fogg, which traveled to the sun of Mount Frankland the ninefo bustrially mostres. Herbert and duty as with reppesition; and they simply anxious with a curiosity.

Everywhelest tiseous two below this nature was excetting in order.
Look, and horid than my sens opened him-likely determined out of the island, and a short, and Nichilliguor of
their convicts, Bob of Zearth; the some steamer. It
could be only tried
leaves for gentlemen other carcy.”

“Well, Pencroft” friends.”

“Let the courdes an except to be batter by morting others reply, and resumed favor off," replied Michel Ardan.

"<yints no fire was made steep to way was with the person of its
dropper. "Wely, open the explored, and which was away.

Cyrus Harding and his clory of a work of the fam.  It served through thee engineer triouse?
And were ease only a light in the lake you.  Captain Neb werewfores the most
life of time to sael of lava, comeled he g	
