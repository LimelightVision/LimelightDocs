# Limelight Documentation Environment Setup

* Install python

* WINDOWS: Add python folder and python scripts folder to PATH
* WINDOWS: pip install sphinx sphinx-autobuild sphinx_rtd_theme sphinx_tabs
* LINUX: sudo pip install sphinx sphinx-autobuild sphinx_rtd_theme sphinx_tabs

* Navigate to the /docs directory in this repo

* LINUX: sphinx-autobuild . _build/html
* WINDOWS: run the "StartAutobuilding" batch script

* This will start hosting the documentation at localhost:8000
* It will auto-update on any file change in the repo
