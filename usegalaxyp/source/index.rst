.. Using Galaxy-P documentation master file, created by
   sphinx-quickstart on Tue May 28 20:37:34 2013.

Galaxy-P User Guide
===================

.. toctree::
   :maxdepth: 2

============
Introduction
============

This guide covers intermediate and advanced usage of the Galaxy-P platform for proteomic and mass spec data analysis. For an gentle introduction to Galaxy, please check out the `Galaxy 101 <https://main.g2.bx.psu.edu/u/aun1/p/galaxy101>`_ maintained at Penn State.

.. ifconfig:: target == 'internal'

    A proteomics version of this Galaxy 101 targeting Galaxy-P has been assembled by Pratik Jagtap and can be found `here <https://galaxyp.msi.umn.edu/u/pratik/p/galaxy-p-101-building-up-and-using-a-proteomics-workflow>`_

===
FTP
===

Galaxy provides the ability to upload files via the web interface (under the
tool menu "Data Source" -> "Upload"), however this only allows one to upload
one file at a time and web browsers generally limit uploads to 2 GB. Galaxy-P
provides an FTP server that can be used to upload more and larger files.


Uploading Files
---------------

.. ifconfig:: target == 'internal'

    ===============  ==========================================
    Property         Value
    ===============  ==========================================
    Hostname         galaxyp.msi.umn.edu
    Connection       FTPS / FTP + Encyrption
    Username         Your MSI username
    Port             990
    ===============  ==========================================


.. ifconfig:: target == 'public'

    ===============  ==========================================
    Property         Value
    ===============  ==========================================
    Hostname         usegalaxyp.org
    Connection       FTPS / FTP + Encyrption
    Username         E-mail used to register Galaxy-P account.
    Port             990
    ===============  ==========================================


The following walkthrough demonstrates how to connect to Galaxy-P FTP server
using WinSCP_ and upload RAW data files. WinSCP is demonstrated because it is
a popular piece of freely available software, but many other tools could be
used as long as they support FTP with encryption.

- Open WinSCP, specify connection information, and then click login. You may
  also want to give this connection a name and save it for later reuse.

    .. ifconfig:: target == 'internal'

        .. image:: ../../images/winscp_msi_galaxyp_login.png
        
    .. ifconfig:: target == 'public'

        .. image:: ../../images/winscp_usegalaxyp_login.png

- The first time you connect, you will likely be prompted to store the hosts
  SSL certificate, do this by clicking "Yes".

  .. image:: ../../images/winscp_save_host.png

- When prompted for your password, please enter it and click "Okay".

  .. image:: ../../images/winscp_password.png

- If everything has gone well, you should now see two file browsers. The one
  on your left is your computer's files and the one on the right is your
  Galaxy-P staging area (which should be initially empty).

  Using the left file browser, navigate to the files you wish to upload and
  select them.

  .. ifconfig:: target == 'internal'

    .. image:: ../../images/winscp_about_to_copy.png
       :scale: 50
    
  .. ifconfig:: target == 'public'

    .. image:: ../../images/winscp_about_to_copy_public.png
       :scale: 50

- Drag and drop these files to the right panel to begin the transfer and wait
  as the files are transfered. 

  .. image:: ../../images/winscp_copying.png

- Verify your files have been copied.

  .. ifconfig:: target == 'internal'

    .. image:: ../../images/winscp_copied.png
       :scale: 50

  .. ifconfig:: target == 'public'

    .. image:: ../../images/winscp_copied_public.png
       :scale: 50

- These files may now be imported into a Galaxy history using the "Data
  Source" -> "Upload" tool. When filling out the upload information, instead of
  using the browser upload option simply check the uploaded files in the "Files
  uploaded by FTP" section.

  Likewise, these a multiple file dataset can be created using these files and
  the "Multiple File Datasets" -> "Upload and merge" tool.

  .. image:: ../../images/ftp_multifile_upload_tool.png

See Also:

- `Galaxy Project Documentation on FTP Uploads <http://wiki.galaxyproject.org/FTPUpload>`_


.. _WinSCP: http://winscp.net/eng/index.php


======================
Multiple File Datasets
======================





Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

