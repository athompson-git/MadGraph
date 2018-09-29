# MadGraph

This README is just intended as a starting point for installing MG5. I highly recommend taking a look at other guides, namely these two:
* http://www.physics.sjtu.edu.cn/madgraphschool/sites/www.physics.sjtu.edu.cn.madgraphschool/files/Tutorial_shangai_basic_mg5.pdf
* https://www.physics.uci.edu/~tanedo/files/notes/ColliderMadgraph.pdf

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

`alias mg5='<your_madgraph_installation_directory>/MG5_aMC_vX_X_X/bin/mg5_aMC`



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




# Writing Macros to Analyze Data
I will outline two main MC formats that are commonly analyzed in a prototypical pheno project; LHEF and Delphes. 
### LHEF Event Format
LHEF (Les-Houches Event Format) is what we call a "generator-level" (GEN-level) event class that, in the case of MG5, contains kinematic information for a columnated list of particles in each event that corresponds to the direct output from MadEvent, *before they have decayed into showers and jets of particles and before detector-level reconstruction*. Essentially, a GEN-level event has the most primordial information about the event and can be cleanly represented by the Feynman diagrams of the model; for this reason it is sometimes called the "truth-level". GEN-level isn't unique to the LHEF format (Pythia also keeps GEN-level data in its output), but sometimes we want to analyze LHEF output directly from MadGraph to see a clean picture of the phenomenology before the kinematics have to be resolved in the reconstruction level. This will require knowledge of how to write a ROOT macro that interfaces with the LHEF event class.

Each event looks like the one below, with columns

| PID  | Status  | D1  |  D2 |   |   | E  |  Px |  Py | Pz  | Mass  | | Spin  |

```
<event>
        21 -1    0    0  501  502 +0.00000000000e+00 +0.00000000000e+00 +9.96530253350e+02  9.96530253350e+02  0.00000000000e+00 0.0000e+00 -1.0000e+00
        21 -1    0    0  502  503 +0.00000000000e+00 +0.00000000000e+00 -1.89929930800e+02  1.89929930800e+02  0.00000000000e+00 0.0000e+00 -1.0000e+00
        6  2    1    2  501    0 +1.51742474854e+02 +5.45362634894e+01 +3.94618788524e+02  4.61721809716e+02  1.77378292361e+02 0.0000e+00 0.0000e+00
        5  1    3    3  501    0 +1.31408528239e+02 +8.86207898192e+00 +3.02820439959e+02  3.30256031880e+02  4.70000000000e+00 0.0000e+00 -1.0000e+00
       24  2    3    3    0    0 +2.03339466145e+01 +4.56741845075e+01 +9.17983485645e+01  1.31465777835e+02  7.97289999143e+01 0.0000e+00 0.0000e+00
       16  1    5    5    0    0 +2.60539374398e+01 -1.60510754079e+01 +1.98329588490e+01  3.64662986136e+01  0.00000000000e+00 0.0000e+00 -1.0000e+00
      -15  1    5    5    0    0 -5.71999082534e+00 +6.17252599154e+01 +7.19653897155e+01  9.49994792217e+01  1.77700000000e+00 0.0000e+00 1.0000e+00
       -6  2    1    2    0  504 -2.00428799507e+02 -1.20595686083e+02 +5.16020590964e+02  5.92230522252e+02  1.72466570717e+02 0.0000e+00 0.0000e+00
       -5  1    8    8    0  504 +2.17541578910e+01 -2.78488402717e+01 +4.75401213254e+01  5.94218345716e+01  4.70000000000e+00 0.0000e+00 1.0000e+00
      -24  2    8    8    0    0 -2.22182957398e+02 -9.27468458111e+01 +4.68480469638e+02  5.32808687680e+02  8.02739264585e+01 0.0000e+00 0.0000e+00
      -16  1   10   10    0    0 -2.02056599155e+02 -5.25670455751e+01 +3.88886445902e+02  4.41387393736e+02  0.00000000000e+00 0.0000e+00 1.0000e+00
       15  1   10   10    0    0 -2.01263582430e+01 -4.01798002360e+01 +7.95940237361e+01  9.14212939443e+01  1.77700000000e+00 0.0000e+00 -1.0000e+00
       21  1    1    2  504  503 +4.86863246532e+01 +6.60594225934e+01 -1.04039056937e+02  1.32507852183e+02  0.00000000000e+00 0.0000e+00 -1.0000e+00
</event>
```
  
where the PIDs (particle identification) for each SM particle (and some BSM particles) are listed according to the PDG convention:

* http://pdg.lbl.gov/2007/reviews/montecarlorpp.pdf

The status codes in LHEF are simple;
* initial states = -1
* intermediate states = 2
* final states = 1

### Delphes Event Format
Secondly, we have the Delphes event classes which are said to be at "detector-level" or "reco-level". This may be more common for pheno analyses, since it is often more telling to get a glimpse as to how the model's kinematics and topology actually get represented in a real experiment. See `Example1.C` to see how a ROOT file with Delphes events inside can be analyzed.
