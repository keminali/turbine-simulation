# turbine-simulation
## Introduction

softwares for turbine simulation
## Geometry
1. **Solidworks**

*Tutorial*

https://confluence.cornell.edu/display/SIMULATION/Wind+Turbine+Blade+FSI+(Part+1)+-+Geometry

https://www.youtube.com/watch?v=0n87iA3YuFc

1. **ANSYS BladeModeler**

## meshing 
1. **ICEM CFD**

2. **ANSYS Meshing**

*tutorial for turbine meshing*

*tetra mesh for one third section*

https://confluence.cornell.edu/display/SIMULATION/Wind+Turbine+Blade+FSI+%28Part+1%29+-+Mesh

*tetra+hexa*
mesh for MRF/sliding mesh

* inner domain, tetra, patch comforming

* outer domain, multizone/sweeping
![tetra+hexa mesh for one third of turbine](https://cloud.githubusercontent.com/assets/17129969/24366199/49402928-1310-11e7-946f-648d3c704fd2.png)

3. **Pointwise**
## UDF
x_component_2nd_stokes_wave
***********
/*second order stokes wave at inlet Boundary, wave velocity components are from equations 3.27 and 3.58, Pengzhi lin. numerical modeling of water waves. CRC press, 2008*/
#include "udf.h"
#define pi 3.14159265359 /*define  constants*/
#define U  0.6 /*free stream velocity*/
#define H 0.076 /*wave height*/
#define g 9.81 /*gravity acceleration*/
#define L 4.8 /*wave length*/
#define d 1.6 /*water depth*/
#define T 1.456/*effective wave period (include doppler effect)*/ 

/*DEFINE_PROFILE: define an inlet velocity profile  that varies as a function of z coordinates or t.*/
DEFINE_PROFILE(streamwise_velocity,ft,var) /* DEFINE Macros, ft is a thread; var:index */
{ 
	/*define variables*/
	real r[ND_ND]; /*Coordinates, r[0] mean x coordinates, r[1] means y coordinates*/
	real k; /*wave number*/
	real z; /*z(vertical) axis*/
	real omega; /*effective wave angular velocity*/
	real t; /*t*/
	k = 2.0*pi/L; 	/*assign values to variables*/
	omega=2.0*pi/T;
	t = CURRENT_TIME; /* Special Fluent macro, current running t */

	face_t f; /* "f" is a face index for each face on the boundary */ 
	
	begin_f_loop(f,ft)/* face loop macro ,loop over all faces in a given face thread,i.e. "ft" */ 
	{		
		 F_CENTROID(r,f,ft); /*F_CENTROID finds the coordinate position of the centroid of the face "f" and stores the coordinates in the "r" array */
		z =r[2]; /* r[1] is y coordinate,r[2] is z coordinate */
		
F_PROFILE(f,ft,var) = U + H*g*k*cosh(k*(-z-0.782+d))*cos(-omega*t)/(2.0*(omega-k*U)*cosh(k*d)) + 3.0*H*H*(omega-k*U)*k*cosh(2.0*k*(-z-0.782+d))*cos(-2.0*omega*t)/(16.0*pow(sinh(k*d),4.0));
/*x-velocity component (flow direction): u_r+U= U + H*g*k*cosh(k*(z+d))*cos(-omega*t)/(2.0*(omega-k*U)*cosh(k*d)) + 3.0*H*H*(omega-k*U)*k*cosh(2.0*k*(z+d))*cos(-2.0*omega*t)/(16.0*pow(sinh(k*d),4.0)); z=0 is the mean free surface levelin the theory model, however, free surface level is z=-0.782m in the Fluent geometry model*/

	}
	end_f_loop(f,ft)
}
**********************************



## Solver
1. **Fluent**

1. **Openfoam**

## post-processing
1. **Cfd-post**

1. **Fluent**

1. **Tecplot**

$\omega=d\rho$
