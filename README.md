# MadGraph


## Prerequisites
ROOT
gcc 4.8 or later
Python 2.7 or later


If you don’t already have it installed, you can get and build ROOT from here:
https://root.cern.ch/building-root

This guide will help you install the following programs:
* MadGraph 5
* Pythia 8 and its dependencies;
* * LHAPDF
* * Boost
* * etc
* Delphes

## MadGraph Installation
(1) Take the the most recent installation file for MG5 from here https://launchpad.net/mg5amcnlo. For example,

```
cd <your_madgraph_installation_directory>
wget https://launchpad.net/mg5amcnlo/2.0/2.6.x/+download/MG5_aMC_v2.6.3.tar.gz
tar xvzf MG5_aMC_v2.6.3.tar.gz
```

Of course the above commands will have to be modified according to whatever the latest MG version is.

(2) Once the installation folder has been untared, we can run MadGraph right away;

```
cd MG5_aMC_v2_6_3/
./bin/mg5_aMC
```

This launches the madgraph interface and it is the program you will launch every time you want to generate a given process. Therefore it would be a good idea later on to add the full path to mg5_aMC as an alias. 
MadGraph will prompt you to install other programs that Pythia and Delphes depend on; that's okay, just say yes (or "y", whatever the prompt asks) when that comes up:

```
MG5_aMC> install pythia8
MG5_aMC> install Delphes
```

At this stage I find it useful to add an alias for MG5 into your .bashrc. Something like
`alias mg5='/home/<yourname/MG5_aMC_v2_6_3/bin/mg5_aMC`

(3) Go through the tutorial built-in to MG5. When you run madgraph you can just type
```MG5_aMC> tutorial```

For this step, the goal is to learn how to put in a particle process and generate a data sample with it. I recommend following along with steps 3 and 4 in this doc: http://www.phys.ufl.edu/~matchev/StandardModel/mg5_tutorial.pdf
(but where they say "./bin/mg5" instead use "./bin/mg5_aMC". The former is deprecated.


### Other resources (bookmark these!):
* http://www.physics.sjtu.edu.cn/madgraphschool/sites/www.physics.sjtu.edu.cn.madgraphschool/files/Tutorial_shangai_basic_mg5.pdf
* https://www.physics.uci.edu/~tanedo/files/notes/ColliderMadgraph.pdf

