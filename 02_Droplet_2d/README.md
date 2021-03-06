## :black_heart: Multiphase flow: Droplet (2D)

:no_entry: :no_entry: :no_entry: Still working.... :no_entry: :no_entry: :no_entry:


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

Copy tutorial on your personal folder
```
[ofuser@xxxxxxxxxxxx workingDir]$ cp -R $FOAM_RUN/tutorials/multiphase/interFoam/RAS/weirOverflow .
[ofuser@xxxxxxxxxxxx workingDir]$ mv weirOverflow droplet_2d
[ofuser@xxxxxxxxxxxx droplet_2d]$ ls
0.orig  Allclean  Allrun  constant  system

```
Here, each directory means
* `0`: Initial Conditions (IC) & Boundary Conditions (BC)
* `constant` : information of kinematic viscosity
* `system` : infomration of discrtization, solver, and time including time step, start+end time, time step when writes output file
* `Allclean` 
* `Allrun` : run 

### ☺︎ Run 
Run the code with `Allrun` script 
```
[ofuser@xxxxxxxxxxxx droplet_2d]$./Allrun 
Restore 0/ from 0.orig/
Running blockMesh on /home/ofuser/workingDir/OpenFOAM_examples/droplet_2d
Running setFields on /home/ofuser/workingDir/OpenFOAM_examples/droplet_2d
Running interFoam on /home/ofuser/workingDir/OpenFOAM_examples/droplet_2d
```

### ☺︎ Blockmesh + Geometry  

Generate Verticies, Faces, and Blocks
> vertex numbering follows the Right-hand rule

![01_verticies](https://user-images.githubusercontent.com/40614421/102402694-781a9200-3fe5-11eb-8678-f2a3b44d051b.png)

Fig 01. Schematic of a domain with verticies.

![02_block](https://user-images.githubusercontent.com/40614421/102402721-7fda3680-3fe5-11eb-8eda-b45ea614a339.png)

Fig 02. Schematic of a domain with a single block.

![03_bc](https://user-images.githubusercontent.com/40614421/102402720-7fda3680-3fe5-11eb-820b-b3ce73fb2948.png)

Fig 03. Structure of a domain with boundary conditions.

![04_droplet_patch](https://user-images.githubusercontent.com/40614421/102402718-7ea90980-3fe5-11eb-83be-0b5b0208c159.png)

Fig 04. Schematic of a domain and patched droplet at a center (water in blue color)

### ☺︎ Properties 
In `constant` folder, you can manage some properties. 
* g : gravity acceleration [m/s^2]

* transportProperties : fluid properties (Here, water and air)

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
    class       dictionary;
    location    "constant";
    object      transportProperties;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

phases (water air);

water
{
    transportModel  Newtonian;
    nu              1e-06;   # kinematic viscosity of water
    rho             1000;    # density of water
}

air
{
    transportModel  Newtonian;
    nu              1.48e-05;  # kinematic viscosity  of air
    rho             1;         # density  of air
}

sigma           0.07;          # surface tension (water-air @ 25'c)


// ************************************************************************* //
```
* turbulenceProperties : [Reynolds Averaged Simulation (RAS)](https://www.openfoam.com/documentation/guides/latest/doc/guide-turbulence-ras.html) and [K-epsilon model](https://www.openfoam.com/documentation/guides/latest/doc/guide-turbulence-ras-k-epsilon.html) in linear-eddy-viscosity models.

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
    class       dictionary;
    location    "constant";
    object      turbulenceProperties;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

simulationType  RAS;

RAS
{
    RASModel        kEpsilon;

    turbulence      on;

    printCoeffs     on;
}

// ************************************************************************* //
```

### ☺︎ Geometry + Boundary conditions + Initial conditions

```
[ofuser@xxxxxxxxxxxx droplet_2d]$ cd system
[ofuser@xxxxxxxxxxxx droplet_2d]$ ls
blockMeshDict  controlDict  fvSchemes  fvSolution  setFieldsDict
```
#### Geometry

```
[ofuser@xxxxxxxxxxxx system]$ more blockMeshDict 
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
    class       dictionary;
    object      blockMeshDict;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

scale   1;

vertices
(
    (  0   0 -0.5)    # vertex 0
    ( 30   0 -0.5)    # vertex 1
    ( 30  30 -0.5)    # vertex 2
    (  0  30 -0.5)    # vertex 3
    (  0   0  0.5)    # vertex 4
    ( 30   0  0.5)    # vertex 5
    ( 30  30  0.5)    # vertex 6
    (  0  30  0.5)    # vertex 7

);

blocks
(
    hex (0 1 2 3 4 5 6 7) (30 30 1) simpleGrading (1 1 1)   # Block 0
);

edges
(
);

patches     # BC
(
    wall lowerWall
    (
        (0 1 5 4)
        (3 2 6 7)
    )
    patch atmosphere
    (
        (0 4 7 3)
        (2 6 5 1)
    )
);

mergePatchPairs
(
);

// ************************************************************************* //
```
#### IC+solver

```
[ofuser@xxxxxxxxxxxx system]$ more controlDict 
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
    class       dictionary;
    location    "system";
    object      controlDict;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

application     interFoam;

startFrom       latestTime;

startTime       0;

stopAt          endTime;

endTime         2;

deltaT          0.001;   # time step

writeControl    adjustable;

writeInterval   0.1;

purgeWrite      0;

writeFormat     ascii;

writePrecision  6;

writeCompression off;

timeFormat      general;

timePrecision   6;

runTimeModifiable yes;

adjustTimeStep  on;

maxCo           0.2;

maxAlphaCo      0.2;

maxDeltaT       1;

// ************************************************************************* //
```

```
[ofuser@xxxxxxxxxxxx system]$ more fvSchemes 
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
    class       dictionary;
    location    "system";
    object      fvSchemes;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

ddtSchemes
{
    default         Euler;
}

gradSchemes
{
    default         Gauss linear;
}

divSchemes
{
    div(rhoPhi,U)    Gauss linear;
    div(phi,alpha)   Gauss vanLeer;
    div(phirb,alpha) Gauss linear;
    div(phi,k)       Gauss upwind;
    div(phi,epsilon) Gauss upwind;
    div(phi,R)       Gauss upwind;
    div(R)           Gauss linear;
    div(phi,nuTilda) Gauss upwind;
    div(((rho*nuEff)*dev2(T(grad(U))))) Gauss linear;
}

laplacianSchemes
{
    default         Gauss linear corrected;
}

interpolationSchemes
{
    default         linear;
}

snGradSchemes
{
    default         corrected;
}

// ************************************************************************* //
```


> coming soon


### ☹︎ Problem : Unable to set reference cell for field p
get this message 

```
--> FOAM FATAL IO ERROR: 
Unable to set reference cell for field p
    Please supply either pRefCell or pRefPoint
```
Solution : add two lines to your fvSolutions in system folder [ref](https://www.cfd-online.com/Forums/openfoam-pre-processing/183498-unable-set-reference-cell-field-p-please-supply-either-prefcell-prefpoi.html)
```
[ofuser@xxxxxxxxxxxx droplet_2d]$ cd system/
[ofuser@xxxxxxxxxxxx system]$ ls
blockMeshDict  controlDict  fvSchemes  fvSolution  setFieldsDict
[ofuser@xxxxxxxxxxxx system]$ vi fvSolution
```
In this PIMPLE part, add `pRefCell  0;` and `pRefValue 0;`
using key `i` to insert and modify `fvSolution` and `etc` to end and `:wq` to write and quit (save).

```
...
PIMPLE
{
    momentumPredictor no;
    nCorrectors     3;
    nNonOrthogonalCorrectors 0;
}
```
<img width="850" alt="fvSolution_problem" src="https://user-images.githubusercontent.com/40614421/102700355-ee6cfd80-424c-11eb-9dc5-b09e35b23a7b.png">


### ☺︎ Post-processing 
For postprocessing
```
[ofuser@xxxxxxxxxxxx droplet_2d]$ paraFoam -touch-all
Created 'droplet_2d.blockMesh'
Created 'droplet_2d.OpenFOAM' 'droplet_2d.foam'
```

go to the directory, then open the file `weirOverflow.OpenFOAM` manually.
(On mac, `paraFoam` doesn't work in terminal. The detailed explanation has been described in section: [☹︎ Problem : paraFoam doesn't work on Mac...](https://github.com/soyounson/OpenFOAM#%EF%B8%8E-problem--parafoam-doesnt-work-on-mac))



> coming soon

Enjoy :)
