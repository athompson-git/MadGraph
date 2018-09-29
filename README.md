# MadGraph


## Prerequisites
* ROOT (install from here: [https://root.cern.ch/building-root](https://root.cern.ch/building-root))
* gcc 4.8 or later
* Python 2.7 or later


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
You can use the MadGraph interface to install programs that interface with MadGraph and the Madevent generator. You can just type
```
MG5_aMC> install
```
to see what programs are available. For now, the most relevant programs to install are the latest Pythia and the latest Delphes. Please run

```
MG5_aMC> install pythia8
MG5_aMC> install Delphes
```
During the installation, MG5 will prompt you to install various dependencies of Pythia and Delphes, e.g. `LHAPDF` and `boost` that's okay, just say yes (or "y", whatever the prompt asks) when that comes up.

After the installation is complete you can exit MadGraph with
```
MG5_aMC> exit
```

All the programs you installed should now exist within the `/MG5_aMC_vX_X_X/HEPTools/` folder and Delphes will be installed in `/MG5_aMC_vX_X_X/Delphes/`.

At this stage I find it useful to add an alias for MG5 into your .bashrc. Something like

`alias mg5='/home/<yourname/MG5_aMC_v2_6_3/bin/mg5_aMC`

## Tutorial (Optional)
Go through the tutorial built-in to MG5. When you run MG5 you can just type
```MG5_aMC> tutorial```

For this step, the goal is to learn how to put in a particle process and generate a data sample with it.


## Installing a Model
It is common to want to test a new physics (e.g., BSM) model in monte-carlo simulation using MadGraph, Pythia, and Delphes software chain. In order to be able to generate Feynman diagrams using the Feynman rules of the BSM, we have to install a model file. This is easy to do.

For example, let's say you want to install the following Dark-Matter model to study monojet pheno: http://uregina.ca/~kolev20n/monotop.html

The model file is linked at the bottom of the page as a zipped folder. All that has to be done is to extract the zipped folder inside the `/models/` directory of MadGraph:
```
cd ~/MG5_aMC_vX_X_X/models/
wget http://uregina.ca/~kolev20n/BaryogenX2N1Maj_withLeft.tar.gz
tar xvzf BaryogenX2N1Maj_withLeft.tar.gz
```
Once it's placed there, MadGraph should be able to load it. You can load a model other than the SM (which is the default) by doing `import model <modelname>`, e.g.

```
MG5_aMC> import model BaryogenX2N1Maj_withLeft
```

### Other resources (bookmark these!):
* http://www.physics.sjtu.edu.cn/madgraphschool/sites/www.physics.sjtu.edu.cn.madgraphschool/files/Tutorial_shangai_basic_mg5.pdf
* https://www.physics.uci.edu/~tanedo/files/notes/ColliderMadgraph.pdf

