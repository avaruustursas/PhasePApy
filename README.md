# PhasePApy 

### __List of changes by avaruustursas (13.08.2018)__

   _This repository has been forked and modified from the original. Following changes has been made to tailor the PhasePApy package more to the needs of the author of this fork._
	 
_Few optimizations has been made to the code to answer some of the bottlenecks found in the 1D association process. There might be a bit more optimization possible by refactoring parts of the code to use more of the language specific features of the Python or features added in later Python3 versions._

___Python3.6 or later is required___

___

##### assoc1D.py
###### Changes to database io operations
   - SqlAlchemy `session.commit()` is used less often to save time from slow database operations

	 _The original script uses_ `session.commit()` _more often than necessary, which might lead to performance problems. Instead of using commit after every addition of new data or updating the database, the commit function is used only after series of additions. This could save considerable amount of time as the database io operations could become bottleneck of the script running time performance._

###### LocalAssociator contructor
   - Adds a workaround on possible problems with near surface source depth (0.0km) travel time tables

   _The original script doesn't handle situations where the minimum S-P value parsed from travel time table database is exactly zero. The minimum S-P value is now checked and if it would be set to zero a new value is queried from the travel time database to prevent problems._

   _If this approach doesn't work, then the user can manually edit the travel time database to include non-zero value for the minimum S-P value._
		

###### comb()
   - Changes the way the candidate event tuples are generated.
	   
		_The original script creates every possible combination of the candidates with Python3_ `itertools.combinations()` _function and then deletes most of them if the combination tuple has same station more than once. This can lead _
		
		_Instead of this only the combinations having each station only once are created with Python3 function_ `itertools.product()`_. This potentially saves time and memory._
   
		_The result of the_ `comb()` _function isn't converted to python list to save memory. This change also has been accounted in the assoc1D_ `associate_candidates()` _method that uses the_ `comb()`.

###### associate_candidates()
   - Small changed to account the comb() method changes.

     _The original_ `comb()` _method returned a list and after this indices were used to refer candidates from the list. As this is no longer possible with different python data structure a minor modification is in place to get things done without indices._

___

## Reference 

Chen,C., and A. A. Holland, (2016). PhasePApy: A Robust Pure Python Package for Automatic 
Identification of Seismic Phases, Seismological Research Letters, 87(6), DOI: 10.1785/0220160019.

			
## About

The PhasePApy is a Pure Python program package developed by Chen Chen (c.chen@ou.edu; 
c.chen8684@gmail.com) under the guidance of Austin Holland.

Version 1.1: 2016	--	FBpicker, AICDpicker, KTpicker; 1D and 3D Associator

The PhasePApy is a Seismic Phase Picker and Associator program package, which is 
designed to identify and associate picks to the seismic events or microseismic events. 
This work has currently been submitted for peer review. Please reference the paper in 
review if you are using this code. Once the paper is published we will include the 
full reference.

The first version 1.0 has been used by Oklahoma Geological Survey (OGS) for near real
time earthquake monitoring since 2014. Here, we only provide the version 1.1 to public,
because we think this one is more user-friendly and easy to manipulate. You might need 
to modify the code a bit for your requirements. Also, we still think the code has some 
space for further clean-up and improvement, so please feel free to modify and update it. 

If you have certain ideas to improve this package, please let us know.
If you are going to use this package for any commercial purpose, please let us know.
			

## Components 
For more information check out the wiki [https://github.com/austinholland/PhasePApy/wiki#welcome-to-the-phasepapy-wiki]

The PhasePApy package composes of two sub-packages: PhasePicker and Associator. These 
two sub-packages can work jointly or separately, depending on users’ requirements. 

### PhasePicker
The PhasePicker contains three pickers: FBpicker, AICDpicker, and KTpicker

### Associator
The Associator includes the 1D and 3D Associator modules which uses phase arrival look up
tables to associate earthquakes. The associator was tested for local to regional earthquake
 P- and S-phases, but the algorithm can be accommodated for any distance and possibly phase.

## License
PhasePApy is released as public domain under the Creative Commons Public Domain Dedication [CC0 1.0] (https://creativecommons.org/publicdomain/zero/1.0/legalcode). 
The software was developed at the [Oklahoma Geological Survey](http://www.ogs.ou.edu).

## Examples 
There are examples in the test directory if one has the required libraries the examples
should run directly out of a git clone of PhasePApy.  

Because the associators require travel-time tables, we have included a data set and travel-
time tables in the git repository.  This is not ideal, but because PhasePApy does not 
include a travel-time calculator for velocity models this approach seemed like the best. 
For this reason the repository is larger than it needs to be.



## Required Libraries
Currently PhasePApy does not have a Python installer for module installation the files 
need to be placed in a proper location and the PYTHON_PATH or sys.path should point to where
the library exists.

The PhasePApy package relies on open libraries: 
+ Obspy 
+ Numpy
+ Scipy 
+ MatPlotLib (Optional) if you are going to plot results with plotting scripts in this 
package, you need matplotlib as well. Otherwise, the users can visualize it based on their own methods.	




