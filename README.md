# Limelight Documentation Environment Setup

##Windows
* Fork this repository
* Clone your forked repo
* Install python https://www.python.org/downloads/ - Ensure that "add python to path" is selected during installation
* Install dependencies from terminal/powershell: "pip install sphinx sphinx-autobuild sphinx_rtd_theme sphinx_tabs"
* Navigate to the "docs" directory
* Run the autobuild batch script from terminal: "./StartAutobuilding.bat"

##Ubuntu/Linux
* Fork this repository
* Clone your forked repo
* Install python and pip
* Install dependencies from terminal: sudo pip install sphinx sphinx-autobuild sphinx_rtd_theme sphinx_tabs
* Clone this repo
* Navigate to the "docs" directory
* Start autobuilding from terminal: "./sphinx-autobuild . _build/html"

* Your built documentation may now be found at http://localhost:8000
* The documentation will auto-update upon any change to any .rst file
* Make a pull request on github from your forked repository to contribute your changes.

