# A computional fluid dynamics (CFD) module for FreeCAD

**An advanced open source CAD integrated CFD preprocessing tool to enable automated one-stop CFD simulation**

![Schematic of automated engineering design pipeline](https://forum.freecadweb.org/download/file.php?id=73587)

by Qingfeng Xia, 2015 <http://www.iesensor.com/HTML5pptCV>

the team from CSIR South Africa, 2016
+ Oliver Oxtoby <http://www.linkedin.com/in/oliver-oxtoby-05616835>
+ Alfred Bogears <http://www.csir.co.za/dr-alfred-bogaers>
+ Johan Heyns  <http://www.linkedin.com/in/johan-heyns-54b75511>

CSIR team has forked this repo into <https://github.com/jaheyns/CfdOF>, focusing on usability for new users and OpenFOAM. Meanwhile, this repo still targets at preparing real-world case files from FreeCAD goemetry for advanced users, which also means user needs to tweak the OpenFOAM case files in production environment. Features from CSIR fork will be picked up regularly in the future.

changelog and roadmap at [Roadmap.md](./Roadmap.md)

## LGPL license as FreeCAD


## Features and limitation

This CFD module now have a subforum in FreeCAD's official forum, see the link: <https://forum.freecadweb.org/viewforum.php?f=37>

This module aims to accelerate CFD case build up. Limited by the long solving time and mesh quality sensitivity of CFD problem, this module will not as good as FEM module. For engineering application, please seek support from other commercial CFD software.

### Features

1. Python script and GUI to run a basic laminar flow simulation is supported by Cfd Workbench

2. An independent python module, FoamCaseBuilder (LGPL), can work with and without FreeCAD to build up case for *OpenFOAM<https://www.openfoam.com/>*

3. Coupling with *FenicsSolver<https://github.com/qingfengxia/FenicsSolver>*, a multiphysics FEA solver set, has been demonstrated in early 2018.

4. Topology optimisation (meshing and boundary setup remain valid during geometry topology change)

![FreeCAD CFDworkbench screenshot](https://github.com/qingfengxia/qingfengxia.github.io/blob/master/images/FreeCAD_CFDworkbench_screenshot.png)


### Features comparison with CfdOF forked

missing features: 

- cfMesh, snappyHexMesh meshing (I will manually merged  this feature later)
- pure python fluid boundary class (my Cfd module using C++ corresponding class in Fem module)
- dropped PyFoam dependency (I have a plan and it is half done)
- porous model, multiphase model (I will not implement it)
- support windows platforms by BlueCFD  ( I support windows 10 only with WSL)
- see more at CfdOF github

added features:   

- windows 10 WSL platform support
- create new analysis from mesh file generated from external meshing tool (e.g. mesh generated from Salome)
  link to [a video demo](http://www.iesensor.com/blog/wp-content/uploads/2018/12/FreeCAD_CFD_using_external_mesh.webm)
- Fenincs FEM solver, initial demonstration of support other 
- boundary layer supported via gmsh
- FoamBoundaryWidget class to provide raw dict to override "CFdFluidBoundary" setting


### Limitation:

1. turbulence model case setup is usable, but only laminar flow with dedicate solver and boundary setup can converge
2. OpenFOAM thermal sovler is under development

### Platform support status
- Linux:  

        Ubuntu 16.04 as a baseline implementation, but should works for other linux distribution.

-  Windows 10 with Bash on Windows (WSL ubuntu 16.04) support (tested on windows 10 v1803):

        Official OpenFOAM  (Ubuntu deb) can be installed and run on windows via Bash on Windows (WSL) Since version 1803 can piped process output back python, which is needed to detect OpenFOAM installation.  There is a tutorial to install OpenFOAM on windows WSL: <https://openfoam.org/download/windows-10/>, make sure OpenFOAM bashrc is sourced in `~/.bashrc`.   On FreeCAD v0.17 for windows, gmsh has been installed, but 'PyFoam' is not included to the FreeCAD's python2.7 bundle. However, `pip` bundled with FreeCAD does not work, instead, 'PyFoam' is downloaded and extracted to "C:\Program Files\FreeCAD 0.17\bin\Lib\site-packages".
    For paraview, it is recommended to uninstall WSL's paraview by `sudo apt-get remove openfoam5paraview54`, to use windows version (make sure paraview is installed and on PAHT) for better 3D performance. paraview on WSL just does not work for me, although `glxgears` works on software rendering via`export LIBGL_ALWAYS_INDIRECT=1`.

- MAC (not tested):

        As a POSIX system, it is possible to run OpenFOAM and this module, assuming OpenFOAM/etc/bashrc has been sourced for bash

![FreeCAD CFDworkbench on Windows 10](http://www.iesensor.com/blog/wp-content/uploads/2018/05/FreeCAD_CFD_module_openfoam_now_working_with_WSL.png)

### Relationship with official FEM workbench:

CFD is a pure Python wokbench based on FEM workbench. Code for meshing, material database capacity are shared.  FemConstraintFluidBoundary is written in C++ and included in official FreeCAD FEM module, similar are VTK mesh and result IO C++ code.

=============================================

## Installation guide

### Prerequisites OpenFOAM related software

#### Debian/Ubuntu
see more details of Prerequisites installation in *Readme.md* in *FoamCaseBuilder* folder

- OpenFOAM (3.0+)  `sudo apt-get install openfoam` once repo is added/imported

> see more [OpenFOAM official installation guide](http://openfoamwiki.net/index.php/Installation), make sure openfoam/etc/bashrc is sourced into ~/.bashrc

- PyFoam (0.6.6+) `sudo pip install PyFoam` (see platform notes for Windows). In the future, this dependency will be removed.

- matplotlib for residual plot (it is bundled with FreeCAD on Windows), gnuplot is not needed any longer. On ubuntu/debian, `sudo apt-get install python-matplotlib`

- paraFoam (paraview for OpenFOAM), usually installed with OpenFoam.

RHEL/Scientific Linux/Centos/Fedora: should work Installation tutorial/guide is welcomed from testers

### Install freecad v0.17 stable
After Oct 2016, Cfd boundary condition C++ code (FemConstraintFluidBoundary) has been merged into official master in stable v0.17

Make sure Gmsh function is enabled and installed.  gmsh 3 has been bundled with FreeCAD v0.17 on Windows.

### Install Cfd workbemch

#### using FreeCAD 0.17 addon manager

#### from github for developers
`git clone https://github.com/qingfengxia/Cfd.git`

symbolic link or copy the folder into `<freecad installation folder>/Mod`,
e.g, on POSIX system:

`sudo ln -s (path_to_CFD)  (path_to_freecad)/Mod`

### Qt5 and Python3 support will follow with FreeCAD's official release

There is a possibibilty that FreeCAD 0.18 will release a Python3 support, since python for Qt5 (pyside2) is generally available. There should be little work on this Cfd module to support Python3.


## Testing

### Tutorial to build up case in FreeCAD interactively

Similar with FemWorkbench

+ make a simple part  in PartWorkbench or complex shape in Partdesign workbench

+ select the part and click "makeCfdAnalysis" in CfdWorkbench
> which creats a CfdAnalysis object, FemMesh object, and default materail

+ config the solver setting in property editor data tab on the left combi panel, by single click sovler object

+ double click mesh object to refine mesh

+ hide the mesh and show hte part, so part surface can be select in creatation of boundary condition

+ add boundary conditions by click the toolbar item, and link surface and bondary value

+ double click solver object to bring up the SolverControl task panel
> select working directory, write up case, further edit the case setting up
  then run the case (currently, copy the solver command in message box and run it in new console)

### Test with prebuilt case

A simple example of a pipe with one inlet and one outlet is included in this repo (test_files/TestCase.fcstd). The liquid viscosity is 1000 times higher than water, to make it laminar flow.
[The video tutorial is here](https://www.iesensor.com/download/FreeCAD_CfdWorkbench_openfoam_tutorial.webm)

*The test case is built on Linux, on windows you need to regenerate mesh, and change the case working folder to use the example case*

Turbulence flow case setup is also usable, see (test_files/TestCaseKE.fcstd), but Reynolds number is set to be 1000, otherwise, calculation will diverge due to bad tetragen mesh. Better CFD meshing is planned for cfmesh, perhaps, snappyHexMesh.


Johan has built a case, see attachment [test procedure on freecad forum](http://forum.freecadweb.org/viewtopic.php?f=18&t=17322)


In 2018, I have completed a new feature, intergrate teh external Salome meshing tool into FreeCAD and OpenFOAM workflow.


### Test FoamCaseBuider package (only in Linux terminal mode)

There is a test script to test installation of this FoamCaseBuilder, copy the file FoamCaseBuilder/TestBuilder.py to somewhere writtable and run

`python2 pathtoFoamCaseBuilder/TestBuilder.py`

This script will download a mesh file and build up a case without FreeCAD.



## Roadmap

### see external [Roadmap.md](./Roadmap.md)



## How to contribute this module

Cfd is still under testing and development, but we are planning to be merged into FreeCAD official.

You can fork this Cfd module (stay with official stable FreeCAD release) to add new CFD solver or fix bugs for this OpenFOAM solver by send pull request.

There is a ebook "Module developer's guide on FreeCAD source code", there are two chapters describing how Fem and Cfd modules are designed and implemented.

<https://github.com/qingfengxia/FreeCAD_Mod_Dev_Guide.git> where updated PDF could be found on this git repo

This is an outdated version for early preview:
<https://www.iesensor.com/download/FreeCAD_Mod_Dev_Guide__20161224.pdf>


