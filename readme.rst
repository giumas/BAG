.. image:: https://travis-ci.com/OpenNavigationSurface/BAG.svg?branch=master
    :target: https://travis-ci.com/OpenNavigationSurface/BAG
	
.. image:: https://ci.appveyor.com/api/projects/status/b4y9lmrhvhlntgo2?svg=true
    :target: https://ci.appveyor.com/project/giumas/bag

|
	
This directory contains the necessary source files and examples required to build and
work with data in the BAG format:

* **api** - This is the primary API directory and contains the source for the Bathymetric Attributed Grid format (BAG).
* **configdata** - Required XML support files. You must have an environment variable called BAG_HOME mapped to this directory in order to run the API functions.
* **docs** - As the documentation evolves it will reside here. Currently this contains some initial draft documentation.
* **examples** - Contains programs to demonstrate some of the API functionality. In particular bagcreate and bagread are good starting points.
* **extlibs** - Contains the 3rd-party libraries upon which the API depends. The examples will also need to link with the libraries in this directory in order to build properly.

*Important Building Notes*: The build instructions can be found here: docs/build_instructions.html
