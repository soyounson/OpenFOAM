## :black_heart: Multiphase flow: weiroverflow

To solve multiphase flow, front tracking (FT) or front capturing (FC) methods are combined with Navier-Stokes equations to find the interface movement and its location at different times. The FT method is a Lagrangian approach which is straight forward to track the moving interface explicitly showing high accuracy ([Tryggvason, Bunner et al. 2001](https://web.itu.edu.tr/~nas/publications/jcppaper.pdf)). However, difficulties in the application of the Lagrangian approach arise when the interface becomes tortuous and breaks up. In these situations, the markers located on the interface, which allow distinguishing between the different phases, come so close to each other that singularities arise. As a result, the location of the interface becomes a not well-defined problem. The FC method is an Eulerian approach and defines the location of interface implicitly. The application of the FC method into numerical methods is quite easy and, as a result, this method is commonly used in solving multiphase flow ([Scardovelli and Zaleski 1999](https://www.annualreviews.org/doi/abs/10.1146/annurev.fluid.31.1.567)). Among the several FC methods, the Volume of Fluid (VOF) ([Hirt and Nichols 1981](https://www.sciencedirect.com/science/article/pii/0021999181901455)) and the level set method ([Sussman, Fatemi et al. 1998](https://www.sciencedirect.com/science/article/abs/pii/S0045793097000534)) are widely adopted in many studies. VOF is based on the application of marker and cell methods ([Harlow and Welch 1965](https://ui.adsabs.harvard.edu/abs/1965PhFl....8.2182H/abstract)), where for each cell a fraction function is defined ranging between 0 to 1. The fraction function is 0 when the cell is totally occupied by gas or 1 when totally occupied by liquid. When the fraction function has a value between 0 and 1, the interface is located within the cell and the liquid density is calculated using the
fraction function value (Hirt and Nichols 1981). 

<!-- # should update * icoFoam solver : laminar, isothermal, incompressible flow
* pisoFoam solve : turbulent, isothermal, incompressible flow
* blockMesh (..\system) : mesh generator supplied with OpenFOAM
* blockMeshDict (..\system) -->

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
[ofuser@xxxxxxxxxxxx weirOverflow]$ ls
0.orig  Allclean  Allrun  constant  system

```
Here, each directory means
* `0`: Initial Conditions (IC) & Boundary Conditions (BC)
* `constant` : information of kinematic viscosity
* `system` : infomration of discrtization, solver, and time including time step, start+end time, time step when writes output file
* `Allclean` 
* `Allrun` : run 

```
#!/bin/sh
cd "${0%/*}" || exit                                # Run from this directory
. ${WM_PROJECT_DIR:?}/bin/tools/RunFunctions        # Tutorial run functions
#------------------------------------------------------------------------------

restore0Dir

runApplication blockMesh  # Generate mesh

runApplication setFields  # Define initial values 

runApplication $(getApplication)  # call interFoam

#------------------------------------------------------------------------------
```

### ☺︎ Run 

```
[ofuser@xxxxxxxxxxxx weirOverflow]$ ./Allrun
Restore 0/ from 0.orig/
Running blockMesh on /home/ofuser/workingDir/OpenFOAM_examples/weirOverflow
Running setFields on /home/ofuser/workingDir/OpenFOAM_examples/weirOverflow
Running interFoam on /home/ofuser/workingDir/OpenFOAM_examples/weirOverflow
[ofuser@xxxxxxxxxxxx weirOverflow]$ ls
0       14  20  28  36  42  50  58  Allclean       log.interFoam
0.orig  16  22  30  38  44  52  6   Allrun         log.setFields
10      18  24  32  4   46  54  60  constant       system
12      2   26  34  40  48  56  8   log.blockMesh
```
Here, 

For postprocessing
```
[ofuser@xxxxxxxxxxxx weirOverflow]$ paraFoam -touch-all
Created 'weirOverflow.blockMesh'
Created 'weirOverflow.OpenFOAM' 'weirOverflow.foam'
```

### In detail...
### ☺︎ Generate mesh  

Generate Verticies, Faces, and Blocks
> vertex numbering follows the Right-hand rule

![11_vertex](https://user-images.githubusercontent.com/40614421/101821342-21661180-3b28-11eb-8cba-b007fcfd1212.png)

Fig 01. Structure of the weirOverflow and verticies.

![12_blocks](https://user-images.githubusercontent.com/40614421/101821340-21661180-3b28-11eb-8f18-adaba201c6ae.png)

Fig 02. Structure of the weirOverflow and blocks.

![13_bc](https://user-images.githubusercontent.com/40614421/101821336-2034e480-3b28-11eb-867c-376cd86e6b35.png)

Fig 03. Structure of the weirOverflow, faces and boundary conditions.

```
[ofuser@xxxxxxxxxxxx weirOverflow]$ cd system
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
    (-18  0 -0.5)   # vertex 0
    (  0  0 -0.5)   # vertex 1
    ( 30  0 -0.5)   # vertex 2
    ( 90  0 -0.5)   # vertex 3
    (-18 30 -0.5)   # vertex 4
    (  0 30 -0.5)   # vertex 5
    ( 15 30 -0.5)   # vertex 6
    ( 90 30 -0.5)   # vertex 7
    (-18 54 -0.5)   # vertex 8
    (  0 54 -0.5)   # vertex 9
    ( 15 54 -0.5)   # vertex 10
    ( 90 54 -0.5)   # vertex 11

    (-18  0 0.5)    # vertex 12
    (  0  0 0.5)    # vertex 13
    ( 30  0 0.5)    # vertex 14
    ( 90  0 0.5)    # vertex 15
    (-18 30 0.5)    # vertex 16
    (  0 30 0.5)    # vertex 17
    ( 15 30 0.5)    # vertex 18
    ( 90 30 0.5)    # vertex 19
    (-18 54 0.5)    # vertex 20
    (  0 54 0.5)    # vertex 21
    ( 15 54 0.5)    # vertex 22
    ( 90 54 0.5)    # vertex 23
);

blocks  # 8 verticies per each block
(
    hex (0 1 5 4 12 13 17 16) (20 20 1) simpleGrading (1 0.5 1)   # Block 0
    hex (2 3 7 6 14 15 19 18) (60 40 1) simpleGrading (1 2 1)     # Block 1
    hex (4 5 9 8 16 17 21 20) (20 24 1) simpleGrading (1 1 1)     # Block 2
    hex (5 6 10 9 17 18 22 21) (15 24 1) simpleGrading (1 1 1)    # Block 3
    hex (6 7 11 10 18 19 23 22) (60 24 1) simpleGrading (1 1 1)   # Block 4
);

edges
(
);

patches  # BC
(
    patch inlet
    (
        (0 12 16 4)
        (4 16 20 8)
    )
    patch outlet
    (
        (7 19 15 3)
        (11 23 19 7)
    )
    wall lowerWall
    (
        (0 1 13 12)
        (1 5 17 13)
        (5 6 18 17)
        (2 14 18 6)
        (2 3 15 14)
    )
    patch atmosphere
    (
        (8 20 21 9)
        (9 21 22 10)
        (10 22 23 11)
    )
);

mergePatchPairs
(
);

// ************************************************************************* //
```


### ☺︎ Fluid properties + Turbulence model

Since we consider two fluid components, we should define water (higher density) and air (lower density).
```
[ofuser@xxxxxxxxxxxx weirOverflow]$ cd constant
[ofuser@xxxxxxxxxxxx constant]$ ls
g  polyMesh  transportProperties  turbulenceProperties
[ofuser@xxxxxxxxxxxx constant]$ more transportProperties 
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

water  # high density 
{
    transportModel  Newtonian;
    nu              1e-06;      # kinematic viscosity
    rho             1000;       # density
}

air    # low density
{
    transportModel  Newtonian;
    nu              1.48e-05;   # kinematic viscosity
    rho             1;          # density
}

sigma           0.07;    # surface tension


// ************************************************************************* //
```

For gravity force, 
```
[ofuser@xxxxxxxxxxxx constant]$ more g
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
    class       uniformDimensionedVectorField;
    location    "constant";
    object      g;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

dimensions      [0 1 -2 0 0 0 0]; # [LT-2]
value           (0 -9.81 0);      # gravity acceleration in y-dir


// ************************************************************************* //
```

For Turbulence, 
```
[ofuser@xxxxxxxxxxxx constant]$ more turbulenceProperties 
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

### ☺︎ Initial Conditions (IC) & Boundary Conditions (BC)

`0` directory means `t=0`, aka setup at time equals to 0.
First, in the `U` file

```
[ofuser@xxxxxxxxxxxx weirOverflow]$ cd 0
[ofuser@xxxxxxxxxxxx 0]$ ls
U  alpha.water  epsilon  include  k  nut  p_rgh
[ofuser@xxxxxxxxxxxx 0]$ more U 
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
    class       volVectorField;
    object      U;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

#include        "include/initialConditions"
              # kg m  s K mol A cd
dimensions      [0 1 -1 0 0 0 0];

internalField   uniform (0 0 0); # as initial setup, Ux,Uy,Uz = zero

boundaryField    # Boundary conditions for velocity
{
    inlet
    {
        type            variableHeightFlowRateInletVelocity;
        flowRate        $inletFlowRate;
        alpha           alpha.water;
        value           uniform (0 0 0);  # uniform velocity (Ux,Uy,Uz)
    }

    outlet
    {
        type            zeroGradient;
    }

    lowerWall
    {
        type            noSlip;
    }

    atmosphere
    {
        type            pressureInletOutletVelocity;
        value           uniform (0 0 0);
    }

    defaultFaces
    {
        type            empty;
    }
}

// ************************************************************************* //
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


`alpha.water ` defines α = 0 (low density, here air) and α = 1 (high density, here water)

```
[ofuser@xxxxxxxxxxxx 0]$ more alpha.water 
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
    location    "0";
    object      alpha.water;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

dimensions      [0 0 0 0 0 0 0];      # alpha : dimensionless quantity
   

internalField   nonuniform List<scalar> 
5080
(
1
1
1
1
1
1
1
1
1
--More--(8%)
```

Define the initial conditions of fluid in `setFieldsDict`

```
[ofuser@xxxxxxxxxxxx weirOverflow]$ cd system
[ofuser@xxxxxxxxxxxx system]$ more setFieldsDict 
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
    object      setFieldsDict;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

defaultFieldValues
(
    volScalarFieldValue alpha.water 0
);

regions
(
    boxToCell
    {
        box (-100 0 -100) (0 20 100);  # fill water (high density, α = 1) in the box 

        fieldValues
        (
            volScalarFieldValue alpha.water 1
        );
    }
);

// ************************************************************************* //

```

### ☺︎ Numerical Scheme and solvers
Check discritization and linear-solver.
To solve multiphase flow problem, consider [interFoam](https://www.openfoam.com/documentation/guides/latest/doc/guide-applications-solvers-multiphase-interFoam.html)

```
[ofuser@xxxxxxxxxxxx weirOverflow]$ cd 0
[ofuser@xxxxxxxxxxxx system]$ ls
blockMeshDict  controlDict  fvSchemes  fvSolution  setFieldsDict
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

endTime         60;

deltaT          0.001;

writeControl    adjustable;

writeInterval   2;

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

> ... maybe update more

### ☺︎ Post-processing 
For postprocessing,
```
[ofuser@xxxxxxxxxxxx weirOverflow]$ paraFoam -touch-all
Created 'weirOverflow.blockMesh'
Created 'weirOverflow.OpenFOAM' 'weirOverflow.foam'
```

go to the directory, then open the file `weirOverflow.OpenFOAM` manually.
(On mac, `paraFoam` doesn't work in terminal. The detailed explanation has been described in section: [☹︎ Problem : paraFoam doesn't work on Mac...](https://github.com/soyounson/OpenFOAM#%EF%B8%8E-problem--parafoam-doesnt-work-on-mac))

<img width="887" alt="01_outputfile" src="https://user-images.githubusercontent.com/40614421/102250418-d707da80-3f03-11eb-9267-e45e328c945f.png">

Open data with `OpenFOAMReader`.

<img width="446" alt="02_open_paraview_format" src="https://user-images.githubusercontent.com/40614421/102250432-db33f800-3f03-11eb-9e8e-5d98a0ca62cf.png">

Then, click `Apply` to activate your output.

<img width="1385" alt="03_open" src="https://user-images.githubusercontent.com/40614421/102250433-db33f800-3f03-11eb-806b-30967cb8c202.png">

By selecting `alpha.water` (phase fraction) in purple square, check how water (fluid) locates in the domain (t = 2)

<img width="1383" alt="04_change_properties" src="https://user-images.githubusercontent.com/40614421/102250438-dbcc8e80-3f03-11eb-86b3-4d5d18e0cd3b.png">

Now, check water (flow) at different time step. As an initial condition, we set the maximum time step of 60. (you can increase maximum time steps to check stabilization of fluid).

<img width="1382" alt="05_check_time_steps" src="https://user-images.githubusercontent.com/40614421/102250439-dc652500-3f03-11eb-8656-50f80068edd0.png">

To save an animation, go to `File` menu, select `Save Animation`. 

<img width="1387" alt="06_save_animation" src="https://user-images.githubusercontent.com/40614421/102250442-dc652500-3f03-11eb-8091-a4b3df9fc200.png">

You can manage saving options if you want. 

<img width="681" alt="07_save_as_a_file" src="https://user-images.githubusercontent.com/40614421/102250445-dcfdbb80-3f03-11eb-9942-4def5aca2aaf.png">

<img width="573" alt="08_save_options" src="https://user-images.githubusercontent.com/40614421/102250446-dd965200-3f03-11eb-8d18-c4f6dae923b8.png">
⎯

Youtube ▷ link below,

<div align="center">
  <a href="https://www.youtube.com/watch?v=9mBfow0UPvk"><img src="https://img.youtube.com/vi/9mBfow0UPvk/0.jpg" alt="IMAGE ALT TEXT"></a>
</div>

Enjoy :)
