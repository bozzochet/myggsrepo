# DTP

## Simulation with GGS

GGS Simulation of a simple detector with the silicon detectors with timing capabilities

Ingredients:

- macros/run.mac: datacard, a la Geant, to set the simulation parameters
- macros/geo.mac: datacard for the parametric geometry
- {src,include}/DetectorConstruction.{cc,hh}: definition, a la Geant, of the geometry
<!-- - Analysis/Analysis.C: ROOT macro to read the GGS output file -->

Typical commands:

##### - compilation and installation:
```
cd <build_path>
cmake -DCMAKE_INSTALL_PREFIX=<installation_path> <source_path> -DGGS_DIR=$GGS_SYS/
make
make install
```

The `make install` is not required and even not suggested. All the pieces are both copied in the build directory and installed: everything should work even in the build directory.

A working example can be found in `useful_commands/cmake_and_compile.sh` that includes also the creation of the `gdml` version of the geometry (see below). A reasonable approch could be to call the build dir as the source one (`<git_clone_name>`) but with the suffix `_build`, then:
```
cd <git_clone_name>_build
cmake -DCMAKE_INSTALL_PREFIX=${PWD/%_build}_install ${PWD/%_build} -DGGS_DIR=$GGS_SYS/
./useful_commands/cmake_and_compile.sh
```
and then, everytime the source code or the scripts are changed, is enough to launch a
```
./useful_commands/cmake_and_compile.sh
```
(if you change the source `cmake_and_compile.sh` you have to call it twice: first time is **fully** executed in its previous version but the `cmake` call changes it; second time the new versions is exectued).

##### - simulation:
```
GGSPenny -g lib/libTestGeometry.so -gd macros/geo.mac -d macros/run.mac -ro test.root
```

A working example can be found in `useful_commands/run.sh`

- visualization of the geometry and of the simulated events:
```
GGSLeonard -g lib/TestGeometry.gdml
```
```
GGSLeonard -g lib/TestGeometry.gdml -i test.root
```

Working examples can be found in `useful_commands/show_geometry.sh` and  `useful_commands/show_events.sh`

##### - conversion from parametric geometry to GDML
```
GGSWolowitz -g lib/libTestGeometry.so -o lib/TestGeometry.gdml -t gdml -gd macros/geo.mac
```

##### - conversion from parametric geometry to VGM (http://ivana.home.cern.ch/ivana/VGM.html)
```
GGSWolowitz -g lib/libTestGeometry.so -gd macros/geo.mac -t vgm -o lib/TestGeometry.vgm.root
```

## Analysis

- conversion from GGS output to plain ROOT file:
```
./exe/Digitization [output file name] <input file name 1> <input file name 2> ...
```
for example:
```
./exe/Digitization DigitOut.root GGSOutput.root
```
If the first and only input argument (not sure if more are passed) is a text file with a list of files they're are chained (as passing them as paramenters).
- create analysis histos:
```
./exe/DataAnalysis [output file name] <input file name 1> <input file name 2> ...
```
for example:
```
./exe/DataAnalysis AnaOut.root DigitOut.root
```
`DataAnalysis` output will be `AnaOut.root`

and
```
./exe/DataAnalysis_calo [output file name] <input file name 1> <input file name 2> ... [nominal energy] [min deposited energy] [max deposited energy]
```
for example:
```
./exe/DataAnalysis_calo AnaOut.root DigitOut.root 100 60 80
```
where `DataAnalysis_calo` executable analysis considers energy deposit in calorimeter.
The command requires three additional arguments:
```
./exe/DataAnalysis_calo AnaOut.root DigitOut.root <nominal energy in GeV> <E calo min in GeV> <E calo max in GeV>
```
where `Ecalo_min` and `Ecalo_max` are needed to consider hit times only if in the specified energy range. The `beam_energy` par is just used to create the binning of the energy histos. 
