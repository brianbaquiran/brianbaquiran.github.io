.. title: Installing Python 2.7.11 on CentOS 7
.. slug: installing-python-2711-on-centos-7
.. date: 2016-05-23 19:17:07 UTC+08:00
.. tags: python, centos, linux, pydata
.. category: 
.. link: 
.. description: 
.. type: text

CentOS 7 ships with python 2.7.5 by default. We have some software that requires 2.7.11. It's generally a bad idea to clobber your system python, since other system-supplied software may rely on it being a particular version. 

Our strategy for running 2.7.11 alongside the system python is to build it from source, then create virtualenvs that will run our software.

.. TEASER_END

Step 1. Update CentOS and install development tools
---------------------------------------------------
.. code-block:: bash

  # as root
  yum upgrade -y
  yum groupinstall 'Development Tools' -y
  yum install zlib-devel openssl-devel

Step 2. Download the Python source tarball
------------------------------------------
.. code-block:: bash

  # As a regular user (avoid doing mundane things as root)
  $ cd /tmp
  $ wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tgz
  $ tar -zxf Python-2.7.11.tgz
  $ cd Python-2.7.11

Step 3. Configure, build and install into ``/opt`` (replace with ``/usr/local/`` if you prefer)
-----------------------------------------------------------------------------------------------
.. code-block:: bash

  $ ./configure --prefix=/opt/
  $ make
  $ make install

Step 4. Install pip and virtualenv for the system Python
--------------------------------------------------------

You have to be root for this.

.. code-block:: bash

  # curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  # python get-pip.py
  # pip install virtualenv

Step 5. Use the system virtualenv to create a venv for your updated Python
--------------------------------------------------------------------------
You can now create virtualenvs, just point --python to the 2.7.11 interpreter

.. code-block:: bash

  $ mkdir env
  $ virtualenv --python=/opt/bin/python2.7 env/pyenv
  $ source env/pyenv/bin/activate
  $ python --version
  Python 2.7.11
