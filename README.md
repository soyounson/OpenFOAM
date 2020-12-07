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
* open terminal
* go to the directory where `startMacOpenFOAM` locates
* `./startMacOpenFOAM`
* you are all set!

<div>
<img width="565" alt="04_start_openFOAM" src="https://user-images.githubusercontent.com/40614421/100897944-884c5080-34c0-11eb-8215-0e5d59b446e0.png">
</div>

```
(base) ist-xxx-xx: ~ $ ./startMacOpenFOAM
```

### ☺︎ Terminate OpenFOAM
to end OpenFOAM, 
```
[ofuser@xxxxxxxxxxxx workingDir]$ exit
```

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
- [ ] 02. [Compressible Flow](https://www.openfoam.com/documentation/tutorial-guide/compressible.php)
- [ ] 03. [Multiphase Flow](https://www.openfoam.com/documentation/tutorial-guide/multiphase.php) (Favorite)
- [ ] 04. [Stress Analysis](https://www.openfoam.com/documentation/tutorial-guide/stressAnalysis.php)
- [ ] ...


### ☺︎ Incompressible Flow 
In an incompressible fluid, particles have constant density (`ρ = const`), and so in the particle frame of reference, the Lagrangian observer does not see any density variation and [`Dρ/Dt = 0`](https://en.wikipedia.org/wiki/Incompressible_flow).  

<img src="http://latex.codecogs.com/svg.latex?\frac{\partial\rho}{\partial&space;t}=0&space;" title="http://latex.codecogs.com/svg.latex?\frac{\partial\rho}{\partial t}=0 " /> 

#### Lid-driven cavity flow
2D  laminar, isothermal, incompressible flow 
uniform mesh

icoFoam solver : laminar, isothermal, incompressible flow
pisoFoam solve : turbulent, isothermal, incompressible flow
blockMesh (..\system) : mesh generator supplied with OpenFOAM
blockMeshDict (..\system)

```
[ofuser@xxxxxxxxxxxx workingDir]$ cd $FOAM_RUN/tutorials/incompressible/icoFoam/cavity/cavity
[ofuser@xxxxxxxxxxxx cavity]$ ls
0  constant  system
```

##### Generate mesh  
open terminal and run `blockMesh`

```
[ofuser@xxxxxxxxxxxx cavity]$ blockMesh
[ofuser@xxxxxxxxxxxx cavity]$ cd system
[ofuser@xxxxxxxxxxxx system]$ ls
PDRblockMeshDict  blockMeshDict  controlDict  fvSchemes  fvSolution
```
<div>
<img width="972" alt="05_blockMesh" src="https://user-images.githubusercontent.com/40614421/100899092-c1d18b80-34c1-11eb-8ea6-34bd1f84b947.png">
</div>

[`blockMesh`](https://www.openfoam.com/documentation/guides/latest/doc/guide-meshing-blockmesh.html) is a structured hexahedral mesh generator.
Key features:
* structured hex mesh
* built using blocks
* supports cell size grading
* supports curved block edges

Check `blockMeshDict` which is dictionary located in `...\system`. Find all details about [Mesh generation with the blockMesh utility](https://cfd.direct/openfoam/user-guide/v6-blockmesh/) 
(add comments by SS)

```
scale   0.1;

vertices
(
    (0 0 0)   # vertex 0
    (1 0 0)   # vertex 1
    (1 1 0)   # vertex 2
    (0 1 0)   # vertex 3
    (0 0 0.1) # vertex 4
    (1 0 0.1) # vertex 5
    (1 1 0.1) # vertex 6
    (0 1 0.1) # vertex 7
);
blocks
(
    hex (0 1 2 3 4 5 6 7)   # vertex number (total 8 verticies (0~7))
    (20 20 1)               # numbers of cells in each direction (20x20)
    simpleGrading (1 1 1)   # cell expansion ratios, uniform mesh
);

edges
(
);

boundary
(
    movingWall
    {
        type wall;
        faces
        (
            (3 7 6 2) # top face (moving wall) with 4 vertices
        );
    }
    fixedWalls
    {
        type wall;
        faces
        (
            (0 4 7 3) # left face 
            (2 6 5 1) # right face
            (1 5 4 0) # bottom face
        );
    }
    frontAndBack
    {
        type empty;
        faces
        (
            (0 3 2 1) # back face 
            (4 5 6 7) # front face 
        );
    }
);

mergePatchPairs
(
);
```

##### Initial Conditions (IC) & Boundary Conditions (BC)

```
[ofuser@xxxxxxxxxxxx cavity]$ ls
0  constant  system
[ofuser@xxxxxxxxxxxx cavity]$ cd 0
[ofuser@xxxxxxxxxxxx 0]$ ls
U  p
```
`0` directory means `t=0`, aka setup at time equals to 0. There are two files, `U` (velocity) and `p` (pressure)

First, in the `U` file
```
$             # kg m  s K mol A cd
dimensions      [0 1 -1 0  0  0 0];  # U [m/s]

internalField   uniform (0 0 0);

boundaryField
{
    movingWall
    {
        type            fixedValue;
        value           uniform (1 0 0);  # uniform velocity (Ux,Uy,Uz)
    }

    fixedWalls
    {
        type            noSlip; # left, right, and bottom faces : noslip bc
    }

    frontAndBack
    {
        type            empty; # front and back faces
    }
}
```
Here, [`dimension`](https://cfd.direct/openfoam/user-guide/v8-basic-file-format/#x17-1290004.2.6) shows 7 properties and unit.
| order | property | SI unit |
| --- | --- | --- |
| 1 | Mass | Kg (kilogram) |
| 2 | Length | m (meter) |
| 3 | Time | s (second) |
| 4 | Temperature | K (Kelvin) |
| 5 | Quantity | mol (mole) |
| 6 | Current | A (ampere) |
| 7 | Luminous intensity | cd (candela) |


Then, in the `p` file
```
dimensions      [0 2 -2 0 0 0 0]; # p [m^2/s^2]

internalField   uniform 0;

boundaryField
{
    movingWall
    {
        type            zeroGradient;
    }

    fixedWalls
    {
        type            zeroGradient;
    }

    frontAndBack
    {
        type            empty;
    }
}
```
`zeroGradient` BC for p means “the normal gradient of pressure is zero”. 

##### other properties

```
[ofuser@xxxxxxxxxxxx cavity]$ ls
0  constant  system
[ofuser@xxxxxxxxxxxx cavity]$ cd constant
[ofuser@xxxxxxxxxxxx constant]$ ls
polyMesh  transportProperties
[ofuser@xxxxxxxxxxxx constant]$ more transportProperties
```
other physical properties are stored
```
nu              0.01; # kinematic viscosity
```

[Reynolds number](https://en.wikipedia.org/wiki/Reynolds_number) is defined by,

<img src="http://latex.codecogs.com/svg.latex?Re=\frac{\rho\cdot&space;U&space;\cdot&space;d}{\mu}=\frac{U\cdot&space;d}{\nu}" title="http://latex.codecogs.com/svg.latex?Re=\frac{\rho\cdot U \cdot d}{\mu}=\frac{U\cdot d}{\nu}" />

where 𝝆: density, U: velocity, d: characteristic length, 𝝁: dynamic viscosity, and 𝝂: kinematic viscosity
Here, Re = 10, d = 0.1, U = 1 so 𝝂 = 0.01

##### check system

```
[ofuser@xxxxxxxxxxxx cavity]$ ls
0  constant  system
[ofuser@xxxxxxxxxxxx cavity]$ cd system
0  0.1  0.2  0.3  0.4  0.5  constant  system
[ofuser@xxxxxxxxxxxx system]$ more controlDict
```
`controlDict` is a file related with time including time step, start+end time, time step when writes output file

```
application     icoFoam;

startFrom       startTime;

startTime       0;

stopAt          endTime;

endTime         0.5;

deltaT          0.005; # time step

writeControl    timeStep;

writeInterval   20;    # write outfile every 20 steps

purgeWrite      0;

writeFormat     ascii;

writePrecision  6;

writeCompression off;

timeFormat      general;

timePrecision   6;

runTimeModifiable true;

```
how to define a time step?
>[convergence condition by Courant–Friedrichs–Lewy (CFL) is a necessary condition for convergence while solving certain partial differential equations (usually hyperbolic PDEs) numerically.](https://en.wikipedia.org/wiki/Courant%E2%80%93Friedrichs%E2%80%93Lewy_condition)
So, time step and grid size should be considered carefllly based on the CFL conditions.
For one-dimensional case, the CFL has the following form:

<img src="http://latex.codecogs.com/svg.latex?C=\frac{U&space;dt}{dx}\leqslant&space;1" title="http://latex.codecogs.com/svg.latex?C=\frac{U dt}{dx}\leqslant 1" />

where C: Courant number, U: velocity, dt: time step, du: length interval 
Here, U = 1, dx = d/Nx = 0.1/20 = 0.005 and dt = 0.005 


##### Numerical Scheme 
check discritization and linear-solver 

```
[ofuser@xxxxxxxxxxxx system]$ ls
PDRblockMeshDict  blockMeshDict  controlDict  fvSchemes  fvSolution
[ofuser@xxxxxxxxxxxx system]$ more fvSchemes
```
specify the choice of finite volume discretisation schemes (treatment of each term in the system of equations) in the `fvSchemes` dictionary

```
ddtSchemes
{
    default         Euler;  # temporal schemes
}

gradSchemes
{
    default         Gauss linear;   # gradient schemes
    grad(p)         Gauss linear;
}

divSchemes
{
    default         none;            # divergence/convection schemes
    div(phi,U)      Gauss linear;
}

laplacianSchemes
{
    default         Gauss linear orthogonal; 
}

interpolationSchemes
{
    default         linear;    
}

snGradSchemes
{
    default         orthogonal;        # Surface-normal gradient schemes
}
```
and 
```
[ofuser@xxxxxxxxxxxx system]$ ls
PDRblockMeshDict  blockMeshDict  controlDict  fvSchemes  fvSolution
[ofuser@xxxxxxxxxxxx system]$ more fvSchemes
```

Case solution parameters are specified in the `fvSolution` dictionary

```
solvers
{
    p
    {
        solver          PCG;
        preconditioner  DIC;
        tolerance       1e-06;
        relTol          0.05;
    }

    pFinal
    {
        $p;
        relTol          0;
    }

    U
    {
        solver          smoothSolver;
        smoother        symGaussSeidel;
        tolerance       1e-05;
        relTol          0;
    }
}

PISO
{
    nCorrectors     2;
    nNonOrthogonalCorrectors 0;
    pRefCell        0;
    pRefValue       0;
}
```
##### run Solver 
open terminal and run `icoFoam`
```
[ofuser@xxxxxxxxxxxx cavity]$ icoFoam
```

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

<img width="1115" alt="06_paraview_data_with" src="https://user-images.githubusercontent.com/40614421/101380151-764e2180-38b5-11eb-8c03-ebea19116db4.png">

click `Apply`

<img width="1153" alt="07_paraview_apply" src="https://user-images.githubusercontent.com/40614421/101380169-79e1a880-38b5-11eb-9f23-d2ff0f7714ab.png">

Then, check properties, here `P`, and `U` 

<img width="1151" alt="08_paraview_properties" src="https://user-images.githubusercontent.com/40614421/101380172-7a7a3f00-38b5-11eb-8736-afe40ce2c4d8.png">

> coming soon :)




Enjoy :)

ref : 
* for FVM, definitely, [Peric's Thesis (1985)](https://www.researchgate.net/publication/35233550_A_finite_volume_method_for_the_three-dimensional_fluid_flow_in_complex_ducts)
* for generating [Eq](https://editor.codecogs.com/)
