.. Proteomics Data Analysis with CloudBioLinux documentation master file, created by
   sphinx-quickstart on Sat Apr 27 17:24:01 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Proteomics Data Analysis with CloudBioLinux's documentation!
=======================================================================

Contents:

.. toctree::
   :maxdepth: 2

------------------------------------------
Starting a CloudBioLinux/CloudMan Instance
------------------------------------------

`This screencast <http://www.youtube.com/watch?v=AKu_CbbgEj0>`_ demonstrates
how to use BioCloudCentral to launch CloudBioLinux/CloudMan. These
instructions mostly apply except for proteomic data analysis, we recommend
using our BioCloudCentral `instance <http://cloud.msi.umn.edu/launch>`_ that
is configured to point at images for proteomic data analysis.

---------
GUI Tools
---------

Linux may be known for opaque command-line tools, but there are actually many
powerful easy-to-use graphical Desktop tools for proteomic data analysis. The
following section outlines some of those available in the CloudBioLinux
environment for proteomics.

MZmine
------

PeptideShaker
-------------

Pride Converter
---------------

Pride Inspector
---------------

TOPPAS
------

------------------
Command-Line Tools
------------------

Tabb Tools
----------

OpenMS
------





------------------------
Programming Environments
------------------------

The image is an ideal environment for mass spec data analysis and tool
development. It comes preconfigured with libraries for `R`, `Python`, and
`Ruby`.

R Libraries
-----------

The proteomics image provides immediate access to dozens of  potentially
useful R libraries. For high-level information about R and proteomics checkout
the blog post `Why R for Mass Spectrometrist and Computational Proteomics
<http://www.r-bloggers.com/why-r-for-mass-spectrometrist-and-computational-
proteomics/>`_.

To start R, simply open a terminal and type `R` followed by enter.

::

    ubuntu@hostname:~$ R
    > 

You will then be staring at the `R` prompt. The interesting libraries to explore include:

`XCMS <http://metlin.scripps.edu/xcms/index.php>`_::

    > library(xcms)

`mzR <http://www.bioconductor.org/packages/2.12/bioc/html/mzR.html>`_::

    > library(mzR)

`FactoMineR <http://factominer.free.fr/>`_::

    > library(FactoMineR)

`caret <http://caret.r-forge.r-project.org/>_::

    > library(caret)

`ggplot2 <http://ggplot2.org/>`_ and `VennDiagram <http://www.biomedcentral.com/1471-2105/12/35>`_::

    > library(ggplot2)
    > library(VennDiagram)


Ruby
----

mspire comes pre-installed. mspire is a ruby library for working with mass
spectrometry data.

    $ irb  # Launch interactive ruby interpreter.
    irb(main):001:0> require 'mspire/mzml'
    => true
    irb(main):002:0>

Visit the `mspire homepage <https://github.com/princelab/mspire>` for more
information on using mspire.

Python
------

pyteomics

    ubuntu@hostname:~$ python
    Python 2.7.3 (default, Aug  1 2012, 05:14:39) 
    [GCC 4.6.3] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> from pyteomics.mass import Composition

Visit the `Pyteomics Documentation <http://pythonhosted.org/pyteomics/>`_ for more information on using pyteomics.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

