#Horizontal Gene Transfer Finder (**HGTF**).


# What is it? #

  * **HGTF** is a Horizontal Gene Transfer (HGT) Finding program.
  * Currently **HGTF** could only find individual HGT corresponding to a single subtree-prune-and-regraft (SPR) between a given species tree and a gene tree.
  * The program reads maximum likelihood trees with bootstrap replicates, and outputs the predicted HGT.


# Files #

  * HGTF1.0/HGTF40.exe
> > Executable file
  * HGTF1.0/HGTF40.dll
> > Library for HGTF1.0/HGTF40.exe
  * HGTF1.0/example/thermotoga.aln.phylip\_phyml\_tree.txt
> > species tree
  * HGTF1.0/example/HGT/thermotoga.aln.phylip\_phyml\_tree.phylip.SPR.out.500.1\_phyml\_tree.txt
> > simulated gene tree with one HGT
  * HGTF1.0/example/HGT/thermotoga.aln.phylip\_phyml\_tree.phylip.SPR.out.500.1\_phyml\_boot\_tree.txt
> > the bootstrap replicates of simulated gene tree with one HGT
  * HGTF1.0/example/control/thermotoga.aln.phylip\_phyml\_tree.phylip.SPR.out.500.1\_phyml\_tree.txt
> > simulated gene tree with NO HGT
  * HGTF1.0/example/control/thermotoga.aln.phylip\_phyml\_tree.phylip.SPR.out.500.1\_phyml\_boot\_tree.txt
> > the bootstrap replicates of simulated gene tree with NO HGT
# Prerequisites #

## Windows ##

  * [Microsoft .NET Framework 4.0](http://msdn.microsoft.com/netframework/) **or**
  * [Mono for Windows (version 2.10.x or higher)](http://www.mono-project.com/downloads/)

## Linux/Unix ##

  * [A working Mono installation and development libraries (version 2.10.x or higher)](http://www.mono-project.com/downloads/)

# Usage #
## Input data ##
You need convert your sequences to maximum likelihood trees with bootstrap replicates.
  * The convertion could be done using [PhyML](https://code.google.com/p/phyml/downloads/)
  * Sample PhyML command for the species Tree (Running Time: 0h0m11s):

> _$ phyml -i example/thermotoga.aln.phylip -d aa_
  * Sample PhyML command for gene Tree (Running Time: 0h3m44s+0h43m ):
> _$ phyml -i example/control/thermotoga.aln.phylip\_phyml\_tree.phylip.out.500.1 -d aa -b 100_

## Command line ##

  * Windows with Microsoft .NET Framework 4.0
> _$ HGTF40 `<speciesTreeFile> <geneTreeFile> <geneBootTreesFile>`_
  * Linux/Unix or Windows with Mono
> _$ mono HGTF40.exe `<speciesTreeFile> <geneTreeFile> <geneBootTreesFile>`_

## Sample output ##
Using the following sample command line
> _$ /usr/local/mono/latest/bin/mono HGTF45.exe example/thermotoga.aln.phylip\_phyml\_tree.txt example/HGT/thermotoga.aln.phylip\_phyml\_tree.phylip.SPR.out.500.1\_phyml\_tree.txt example/HGT/thermotoga.aln.phylip\_phyml\_tree.phylip.SPR.out.500.1\_phyml\_boot\_trees.txt_

we can get outputs as follows,

---

SpeciesTree:
> (taxon10:0.07225940,taxon13:0.09891456,((taxon12:0.06913140,taxon14:0.07773158):1000.15294095,(((taxon3:0.13937296,taxon15:0.29262961):1000.20156697,(taxon4:0.41554760,taxon5:0.21375623):1000.16542119):1000.12328341,((taxon1:0.21076622,taxon9:0.17869411):1000.18617587,(taxon7:0.04855215,(taxon6:0.00533351,(taxon8:0.00274986,(taxon2:0.00466243,taxon11:0.00145468):1000.00803293):1000.01053198):1000.03848095):1000.18289167):970.04771983):1000.11298768):1000.18673281)

GeneTree:
> (taxon15:0.28986072,taxon13:0.11675252,(taxon3:0.13205365,((taxon5:0.16140822,taxon4:0.44498832):1000.12363313,((taxon10:0.06617431,(taxon14:0.06618866,taxon12:0.09217273):1000.17848019):1000.11303988,((taxon9:0.15728519,taxon1:0.22660310):1000.18079304,(taxon7:0.06186526,(taxon6:0.00438561,(taxon8:0.00460213,(taxon2:0.01126666,taxon11:0.00225284):950.00450374):980.00938478):1000.05246392):1000.19633963):990.04257402):1000.10590160):1000.23484673):1000.18806866)

The predicted HGT is an SPR, not an NNI: We cut node "taxon13" and paste as sibling of node "taxon15"

SprTree:
> (taxon13,taxon15,((((taxon10,(taxon12,taxon14)),((taxon1,taxon9),(taxon7,(taxon6,(taxon8,(taxon2,taxon11)))))),(taxon4,taxon5)),taxon3))

---


## Batch mode ##
Sometimes we are tend to do this work in an easy way. We could write some scripts to combine PhyML and HGTF
  * **Windows**: Save following texts as RUNME.cmd in the same folder of HGTF40.exe.

---

> _@echo off_

> _phyml -i %1 -d aa_

> _phyml -i %2 -d aa -b 100_

> _HGTF40 %1\_phyml\_tree %2\_phyml\_tree %2\_phyml\_boot\_trees_

---


  * **Linux/Unix**: Save following texts as RUNME.sh in the same folder of HGTF40.exe. You could also like to set the permission to execute using chmod a+x RUNME.sh

---

> _`#!/bin/sh`_

> _phyml -i $1 -d aa_

> _phyml -i $2 -d aa -b 100_

> _mono HGTF40.exe %1\_phyml\_tree $2\_phyml\_tree $2\_phyml\_boot\_trees_

---


# To-Dos #
  * We could not find a proper way to describe identified HGT. The program currently gives only the result tree as well as some confusing text.
  * We can prediction individual HGT between a species tree and a gene tree. The multiple HGTs identification is more time consuming. We plan to design some heuristic algorithm to implement this, which may appear in next version.