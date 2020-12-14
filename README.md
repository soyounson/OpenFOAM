## :black_heart: OpenFOAM on mac

> OpenCFD Ltd. uses Docker Hub to distribute pre-compiled versions of OpenFOAM for Linux, Mac OS X and Windows, including a complete development environment. [OpenFOAM](https://openfoam.com/download/install-binary-mac.php)

Prerequisites : Docker (for mac), OpenFOAM 
Motivation : solve for fun 
Numerical solver : Finite Volume Method (FVM)

### ☺︎ Install Docker on Mac
[Docker Desktop for mac user manual](https://docs.docker.com/docker-for-mac/) navigates how to install
Download [Docker Desktop for mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
then install `Docker.dmg`
<div>
<img width="722" alt="02_docker_drag_to_application" src="https://user-images.githubusercontent.com/40614421/100893073-410f9100-34bb-11eb-8f12-98fcba6c9da2.png">
</div>

### ☺︎ Install and run OpenFOAM
turn on docker
open a terminal
[release of OpenFOAM® v2006 (20 06)](https://hub.docker.com/r/openfoamplus/of_v2006_centos73)
(when I skiped this process, I had an error message...)
```
(base) ist-xxx-xx: ~ $ docker pull openfoamplus/of_v2006_centos73
```
#### ☺︎ Download `installMacOpenFOAM` and `startMacOpenFOAM`
first, make the scripts executable
```
(base) ist-xxx-xx: ~ $ chmod +x installMacOpenFOAM
(base) ist-xxx-xx: ~ $ chmod +x startMacOpenFOAM
```
install OpenFOAM and start working on the container
```
(base) ist-xxx-xx: ~ $ ./installMacOpenFOAM
(base) ist-xxx-xx: ~ $ ./startMacOpenFOAM
```
<div>
<img width="626" alt="03_docker_openform" src="https://user-images.githubusercontent.com/40614421/100897356-e62c6880-34bf-11eb-8ed5-fcde36115f23.png">
</div>

```
(base) ist-xxx-xx: ~ $ cd /home/ofuser/workingDir
(base) ist-xxx-xx: ~ $ cp -r $FOAM_TUTORIALS/incompressible/icoFoam/cavity/(base) ist-xxx-xx: ~ $ cavity .
(base) ist-xxx-xx: ~ $ cd cavity
(base) ist-xxx-xx: ~ $ blockMesh
(base) ist-xxx-xx: ~ $ icoFoam
```
#### ☺︎ Start OpenFOAM on terminal
* start docker
> docker is starting...
<img width="364" alt="02_docker_starting" src="https://user-images.githubusercontent.com/40614421/101661317-6d418980-3a48-11eb-94cb-f8b0958ad33c.png">

> docker is starting...
<img width="365" alt="01_docker_running" src="https://user-images.githubusercontent.com/40614421/101661360-79c5e200-3a48-11eb-9b6c-87fb8f5a7290.png">

* open terminal
* go to the directory where `startMacOpenFOAM` locates
* `./startMacOpenFOAM`

<img width="572" alt="03_start_openFOAM" src="https://user-images.githubusercontent.com/40614421/101661404-84807700-3a48-11eb-9359-c332bcedfdd9.png">

```
(base) ist-xxx-xx: ~ $ ./startMacOpenFOAM
```
* you are all set!

### ☺︎ OpenFOAM tutorial
[tutorial](https://www.openfoam.com/documentation/tutorial-guide/)

```
/*--------------------------------*- C++ -*----------------------------------*\
| =========                 |                                                 |
| \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox           |
|  \\    /   O peration     | Version:  v2006                                 |
|   \\  /    A nd           | Website:  www.openfoam.com                      |
|    \\/     M anipulation  |                                                 |
\*---------------------------------------------------------------------------*/
FoamFile
{
    version     2.0;
    format      ascii;
    class       volScalarField;
    object      p;
}
```

### ☺︎ Tutorial
Download [tutorail](https://www.openfoam.com/documentation/tutorial-guide/introduction.php)
> Copies of all tutorials are available from the tutorials directory of the OpenFOAM installation. The tutorials are organised into a set of directories according to the type of flow and then subdirectories according to solver.

```
[ofuser@xxxxxxxxxxxx workingDir]$ mkdir -p $FOAM_RUN
[ofuser@xxxxxxxxxxxx workingDir]$ cp -r $FOAM_TUTORIALS $FOAM_RUN
```

### ☺︎ Curriculum
- [x] 01. [Incompressible Flow](https://www.openfoam.com/documentation/tutorial-guide/incompressible.php)
- [ ] 02. [Multiphase Flow](https://www.openfoam.com/documentation/tutorial-guide/multiphase.php)
   * Droplet on the surface (single component multiphase) ... coming soon
- [ ] 03. [Compressible Flow](https://www.openfoam.com/documentation/tutorial-guide/compressible.php)
- [ ] 04. [Stress Analysis](https://www.openfoam.com/documentation/tutorial-guide/stressAnalysis.php)
- [ ] ...


### ☺︎ Post-processing 
paraview (opensource)
[download paraview](https://www.paraview.org/download/)
open with `paraFoam` later

```
[ofuser@xxxxxxxxxxxx cavity]$ parafoam
```
### ☹︎ Problem : paraFoam doesn't work on Mac...
As Tutorial described in section 2.1.4 [Post-processing](https://cfd.direct/openfoam/user-guide/v7-cavity/), type `paraFoam`

```
[ofuser@xxxxxxxxxxxx cavity]$ paraFoam
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-ofuser'
QXcbConnection: Could not connect to display 
```
But, get error message as seen above..
Other ppl also suffered from this problem. The solution can be found in the post [OpenFOAM: QXcbConnection: Could not connect to display :0](https://www.cfd-online.com/Forums/paraview/197451-qxcbconnection-could-not-connect-display-0-a.html#post705610)

So! The solution is.. 
```
[ofuser@xxxxxxxxxxxx cavity]$ paraFoam -touch-all
Created 'cavity.blockMesh'
Created 'cavity.OpenFOAM' 'cavity.foam'
```
Then, open paraview manually...
select `OepnFOAMReader`


### ☺︎ Terminate OpenFOAM
to end OpenFOAM, 
```
[ofuser@xxxxxxxxxxxx workingDir]$ exit
```

Enjoy :)

ref : 
* for FVM, definitely, [Peric's Thesis (1985)](https://www.researchgate.net/publication/35233550_A_finite_volume_method_for_the_three-dimensional_fluid_flow_in_complex_ducts)
* for generating [Eq](https://editor.codecogs.com/)
