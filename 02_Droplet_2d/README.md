## :black_heart: Multiphase flow: Droplet (2D)

> Still working....


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


Fig 01. Schematic of a domain with verticies.



Fig 02. Schematic of a domain with a single block.


Fig 03. Structure of a domain with boundary conditions.

Fig 04. Schematic of a domain and patched droplet at a center (water in blue color)



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