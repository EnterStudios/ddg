========================================
Energetic effects of mutation benchmarks
========================================

This benchmark capture contains two related benchmarks, both of which measure the accuracy with which a protocol predicts
the energetic effects of mutation.

-----------------------------------
|DDG| (protein stability) benchmark
-----------------------------------

Monomeric |DDG| protocols predict the change in protein stability which occurs as a result of mutagenesis. This benchmark
includes four curated datasets of experimentally measured values which can be used to evaluate the accuracy of a protocol.

This benchmark includes:

- four curated datasets, three of which were previously published (see input/README.rst). Most of the data was taken from the `ProTherm database <http://www.abren.net/protherm>`_;
- scripts to run a Rosetta protocol described in Kellogg et al. (2011);
- an analysis script that output the metrics used for analysis. The script also outputs a scatterplot plotting experimental |DDG| values in kcal/mol against predicted values (in whichever scoring unit is used by the protocol);
- sample output data which can be used to test the analysis scripts.

Benchmarking a complete dataset is quite computationally intensive so we recommend that a benchmark run be performed on a cluster, grid, or cloud computing resource. We have provided scripts to run this benchmark on a Sun Grid Engine cluster (see hpc/sge/ddg_monomer_16/README.rst).

--------------------------
Alanine scanning benchmark
--------------------------

A frequent application of modeling methods is the prediction of energetically important interactions (“hotspots”) in
protein-protein interfaces. By systematically mutating protein interface residues to alanine (“alanine scanning”)
and measuring the effect on binding, `Wells et al., 1995 <#references>`_ showed that not all residues with interface contacts but only a smaller
subset of ‘hotspot’ residues contribute significantly to the binding free energy of human growth hormone to its receptor.
Subsequent studies suggested that such hotspots may be a general characteristic of many protein-protein interfaces. This
benchmark tests the ability of computational alanine scanning protocols to recapitulate the results of experimental alanine
scanning. A computational protocol performing well on this test set can then be used for additional applications, for
instance, as a design tool to disrupt protein-protein interactions by mutations or through targeting small molecules to
hotspots, or to analyze the effect of disease mutations.


This benchmark includes:

- a previously published set of 233 mutations (to alanine) in 19 different protein-protein interfaces with known crystal structures (see `Kortemme & Baker, 2002 <#references>`_);
- scripts to run a new RosettaScripts protocol which has been designed to emulate the protocol described in Kortemme & Baker (2002);
- an analysis script that output the metrics used for analysis. The script also outputs a scatterplot plotting experimental |DDG| values against predicted values (in whichever scoring unit is used by the protocol);

The RosettaScripts alanine scanning protocol is not computationally intensive so this benchmark can be performed on a typical laptop or workstation.

---------
Licensing
---------

This repository contains third party libraries and materials which are distributed under their own terms (see
LICENSE-3RD-PARTY). The novel content in this repository is licensed according to LICENSE.

-------------------------
Downloading the benchmark
-------------------------

The benchmark is hosted on GitHub. The most recent version can be checked out using the `git <http://git-scm.com/>`_ command-line tool:

::

  git clone https://github.com/Kortemme-Lab/ddg.git

---------------------------
Directories in this archive
---------------------------

This archive contains the following directories:

- *input* : contains the input files for the benchmarks. These take the form of PDB files and datasets of experimental |DDG| values. The input files are described in more detail in input/README.rst;
- *output/sample* : contains sample output data that can be used to test the stand-alone analysis script;
- *analysis* : contains the stand-alone analysis script (analyze.py) and the analysis functions (stats.py). All protocols are expected to produce output that will work with both scripts. These scripts do not need to be called directly - each benchmark has a separate analysis step which performs analysis;
- *protocols* : contains the scripts needed to run the benchmarks. The scripts for each protocol are provided in a specific subdirectory;
- *protocols/alanine-scanning* : contains the scripts needed to run the alanine scanning benchmark using a Rosetta protocol;
- *protocols/ddg_monomer_16* : contains the scripts needed to run the protein stability benchmark using the Rosetta ddg_monomer protocol;
- *protocols/rosetta* : contains utility code used to run the Rosetta protocols;
- *hpc* : contains scripts that can be used to run the entire benchmark using specific cluster architectures. For practical reasons, a limited number of cluster systems are supported. Please feel free to provide scripts which run the benchmark for your particular cluster system.
- *hpc/sge/ddg_monomer_16* : contains scripts that can be used to run the the protein stability benchmark using ddg_monomer on a Sun Grid Engine cluster.


=========
Protocols
=========

This repository contains a protocol which can be used to run the |DDG| benchmark and another which can be used to run the
alanine scanning benchmark. We welcome the inclusion of more protocols. Please contact support@kortemmelab.ucsf if you wish
to contribute towards the repository.

Each protocol is accompanied by specific documentation in its protocol directory.

-------------------------------------------------
Protein stability protocol 1: ddg_monomer, row 16
-------------------------------------------------

Created by: Elizabeth Kellogg, Andrew Leaver-Fay, David Baker [1]_

Software suite: Rosetta

Protocol directory: protocols/ddg_monomer_16

----------------------------------------------------
Alanine scanning protocol 1: RosettaScripts protocol
----------------------------------------------------

Created by: Kyle Barlow

Software suite: Rosetta

Protocol directory: protocols/alanine-scanning

=====================================
Running the benchmark: helper scripts
=====================================

While both the |DDG| and alanine protocols can be run directly on each case using published command line arguments to Rosetta, we have also included helper scripts for each protocol to assist in running them.
Both protocol's helper scripts are customized to that protocol, but are used in similar ways.

#. The benchmark setup script is run.
   This setup script may take in options to determine which subset of the benchmark is run, or what flags will be passed to Rosetta.
   The setup script will create and copy all necessary input files into a "job output" directory, containing a Python run script.
#. (Optional) If the benchmark is to be run on a high-performance cluster, the self-contained generated job output directory can be copied onto that cluster.
#. The Python run script (in the job output directory) is run with no arguments.
   Rosetta will be called with the appropriate arguments by this run script, and the output saved into the same directory.
   On a machine with multiple CPUs, Python's multiprocessing module is used to speed the runtime.
   The script can also be run on a SGE cluster by using the qsub command.

The above steps are repeated two times in the |DDG| protocol (see relevant documentation).

========
Analysis
========

The same set of analysis scripts is used by all protocols. Conceptually, the analysis scripts should be a black box that
is separated from the output of each protocol by an interface. The expected input format is described in analysis/README.rst.

The analysis scripts generates three metrics which can be used to evaluate the results of the |DDG| and alanine scanning
simulations and also produces a scatterplot of the experimental and predicted values. The benchmark analysis is described
in more detail in analysis/README.rst.


==========
References
==========

The latest release of this repository: |releasedoi|

.. |releasedoi| image:: https://zenodo.org/badge/doi/10.5281/zenodo.18595.svg   
   :target: http://dx.doi.org/10.5281/zenodo.18595

-----------------------------------
|DDG| (protein stability) benchmark
-----------------------------------

Kellogg, EH, Leaver-Fay, A, Baker, D. Role of conformational sampling in computing mutation-induced changes in protein structure and stability. 2011.
Proteins. 79(3):830-8. `doi: 10.1002/prot.22921 <https://dx.doi.org/10.1002/prot.22921>`_.

--------------------------
Alanine scanning benchmark
--------------------------

Clackson T, Wells JA. A hot spot of binding energy in a hormone-receptor interface.
Science. 1995 Jan 20;267(5196):383-6.
`doi: 10.1126/science.7529940 <https://dx.doi.org/10.1126/science.7529940>`_.

Kortemme, T, Baker, D. A simple physical model for binding energy hot spots in protein–protein complexes.
Proc Natl Acad Sci U S A. 2002 Oct 29;99(22):14116-21. Epub 2002 Oct 15.
`doi: 10.1073/pnas.202485799 <https://dx.doi.org/10.1073/pnas.202485799>`_.

Kortemme T, Kim DE, Baker D. Computational alanine scanning of protein-protein interfaces.
Sci STKE. 2004 Feb 3;2004(219):pl2.
`doi: 10.1126/stke.2192004pl2 <https://dx.doi.org/10.1126/stke.2192004pl2>`_.


=====
Notes
=====

.. [1] The Rosetta application was written by the authors above. This protocol capture was compiled by Shane O'Connor. Any errors in the protocol capture are likely to be the fault of the compiler rather than that of the original authors. Please contact support@kortemmelab.ucsf.edu with any issues which may arise.


.. |Dgr|  unicode:: U+00394 .. GREEK CAPITAL LETTER DELTA
.. |ring|  unicode:: U+002DA .. RING ABOVE
.. |DDGH2O| replace:: |Dgr|\ |Dgr|\ G H\ :sub:`2`\ O
.. |DDG| replace:: |Dgr|\ |Dgr|\ G


