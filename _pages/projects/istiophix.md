---
title: F1 in Schools
author_profile: true
permalink: /projects/istiophix
---
{% include base_path %}


I was a part of Team Istiophix (5 member team) who won the F1 in Schools National Championship in UAE and represented UAE at the World Final in UK. In this project we were to design a 3D model of an F1 car which was 20mm. The car was to race with other cars in a 20m long track and was powered using CO2 cannisters. 

I was the Manufacturing engineer, Simulations Engineer as well as the Graphic Designer of the team. The engineering side of our project mainly consisted of the following tasks,

1. Design a new model under the regulations using CAD.
2. Run simulations on the model using CFD (Computational Fluid Dynamic) and FEA (Finite Element Analysis) refine our initial design.
3. Manufacturing the car which includes two steps
	1. CNC milling the car body using polyurethene
	2. 3D printing wheels, side pods and front wings using FDM (Fused Deposit Modeling) and SLA (Stereolithography)
4. Assembling the Car.



# Simulations
The process of developing the car involved running CFD simulations and FEA analysis to identify which part of the car causes maximum drag so that we could iteratively reduce drag. In addition the FEA analysis helped in improving the structural rigidity and identify structural weakness in our car body.

![Simulations](/images/istiophix_simulations.png)


## CFD Software Stages
**Preparing the CAD for CFD** 
Using geometry tools, a built in module, we were able to merge all the "small" edges and remove all objects which were less than 10^-4mm in size. This minimizes the errors that we encounter during the meshing process. We then set the boundaries around the car and use the automatic meshing tool to form a rectangular mesh around the car.
 
**Material Setting**
We set the materials of either of our components. As the entire car body was merged into one, we directly assign a solid ABS to it, while for the CFD created volume around the car body we set a Fluid Air material to it. 

**Boundary Conditions**
The boundary conditions state the conditions of all six faces of the CFD created volume around the car. We found out that for the optimum results we are to put a boundary condition of Pressure = 0 for all sides except the front. As for the front side of the volume, we put a boundary condition of Velocity = 20 m/s.

**Assigning rotation motion to the wheels**
Now that we have assigned the boundary conditions and the materials, to have a more accurate reading we have to add the rotation condition to our car wheels. For this we add the motion property to our wheels along the central axis

**Solving** 
Now that we have assigned the boundary conditions and the materials, it's time to solve. For solving we decided to iterate over a range from 300-500 iterations to ensure that the recorded values are stable and not unreliable.


**Evaluation**
We evaluated our test using three main methods, 
- Traces
- Global
- Planes

1. Traces

Traces was our most used method for evaluation as it shows the flow path lines moving around the car, within traces we are able to simulate these particles in different ways by changing the seed density and the type of visualization (lines, columns, spheres)

2. Global

Global evaluation is helpful in representing the results of the simulation directly on the surface of the model.
In the given figure below, we can see that high pressure regions are towards the front of the car as the air hits these parts first and then slows down thus reducing the pressure.

3. Planes

Planes are used to visualize our analysis in a cross section of our mesh including both the car model as well as the area (air) around it.





## FEA software stages
We used a static stress simulation within Autodesk Fusion 360 for our FEA analysis. First once we simplified the geometry of our model using the automatic tool, the we went into the Static Stress simulation tool.

Here we first set up the initial constraints, these constraints are the parts that are static/fixed throughout the stress applied to the body.

Now we look into the loads/forces that we apply to the body. The important factors to look into while adding the loads are.

1. Magnitude
2. Distribution
3. Orientation

In our case both the distribution of the weight and orientation of the weight were easily understood as we had our physical model and we could simulate the forces that we apply.

However for the magnitude we could not pin point a specific value for which we would be ensured an accurate result for all our models. For this case we tested our model with multiple magnitudes thus finding the sweet spot for different models.



# Manufacturing Stage
## CNC


**Alignment of the tool in the direction of milling.**
Once the stl model of the car is imported into Quickcam PRO, the car model has to be rotated so that the appropriate corner is in the datum point such that the tool is in the direction of milling.
 
**Offsetting cut depth**



As we are milling the car body from either side of the car, to ensure a smooth finish along the surface joining either sides, we increase the cut depth by 3.175mm which is the radius of our cutter.

 This process makes the total cut depth from 35mm which is half the total car width to 38.175.



**Finishing plan**
We went straight towards a finishing plan as polyurethane is a very “soft” material compared to aluminum. We are able to effectively apply a roster finish without requiring roughing plans.


**Step over value**
Step over value is the incremental value that the tool will take every iteration in the horizontal axis of the car. The step-over value plays a role in the finish of the car as well as the total time taken to manufacture the car. 

For our initial car manufacturing, we changed the step-over value from the default 0.985mm to 0.7mm to make sure that the car surface is relatively smooth enough for sanding while also keeping the total manufacturing time in a place which is about 7-10 min. 

However, for our final car model, we decided to use a 0.6mm step over value which was the lowest available to us to ensure that the tool effectively covers the surface to smooth them, although it may take a longer time of about 20min.

**Raster angle**
Raster angle is the angle at which the tool is oriented to cut the car body. The default roster angle 270 degrees which is directly perpendicular to the horizontal axis of the car. 

However we found that this angle increases the total time taken for the CNC. We found out that an ideal angle of 225 degrees makes sure that the total time taken for the CNC is minimal while also ensuring the quality is maximum as it removes more material given the same step over value, at the same time because of the how the structure of different sides of our car, we used a 180-degree raster angle for the underside of the car as it is more efficient in that case.


**Exporting and Mirroring**
The CAM simulation is now done and exported into a file which is to be run by our equipment. The file in question only holds information for the right side of the body, however for CNC milling we require information to mill the three sides of the car ie. Right Side, Left Side, Bottom Side. For the left side we append the following strings “/M71'' into the binary file which holds the spatial information the CNC machine will follow. This command makes sure that the CNC machine is able to mirror the binary file for use on both sides of the car body. And for the bottom side we repeat processes from 1 - 5 with the appropriate changes mentioned above and export a second binary file.

**Running the CNC Machine**
Now we run the CNC machine using the official Virtual Reality CNC milling software which comes with our equipment. Each side of the car body takes 5-7 minutes each. As our binary file is made given datum point at the right back corner of the car model, the right side of the car body is first milled. We then rotate the car with the underside facing up, we then use the bottom file to mill the underside of the car. Lastly we rotate the car onto the left side facing up and using the "skip blocK" option we are able to skip our "M71" command so the initial mirrored mill would not be mirrored and hence would mill the left side.

**Milling**
As discussed earlier, using different techniques we can mill our body to ensure different surface finishes by changing the roster angle. For our final car model we used a roster angle of 270 (perpendicular to the vertical component of our car) which ensures maximum surface finish and least sanding.

Another important aspect was to ensure proper alignment of the model block within the fixtures. When the model block in fitted in place, there is a small  wiggle room that may be present in the vertical component of our car, we made sure while switching arrangements the block was fitted in place similarly.

Often our car models come out unsymmetrical, this is because of an error in the y axis offset, and this issue may cause misalignment of other components  

## 3D Printing
### FDM

We had access to three FDM printers, one tier time upbox and two tier time upmini 2. Although we initally printed all our components in using FDM, we eventually switched to using SLA for the rearwing and hubcaps. Our FDM prints were strong but had bad surface finish.

**3D Printing Process**

For FDM prints we used a 0.15mm nozzle diameter and a fine finish to ensure that our final 3d printed parts were the best it could be. Once the print was complete, all we had to do was to scrape our print off the build plate and remove the support structures automatically made by the software.

**Manufacturing considerations**
Because of manufacturing defects, we had to calibrate our 3D printer by finding the right dimensions of each component before finalizing our print dimensions eg for a dimension of a 3mm hole we had to print using a 2.89mm hole.
 


In addition our prints were inconsistent between different arrangements of prints the dimensions of our bearing opening in our wheel varied depending on the number of wheels printed in a single print.
### SLA

We bought our very own Elegoo Mars 2 Pro, an MSLA printer to print out components (rear wing and hub caps). SLA printers as they print  by hardening resin on a specific layer depend only on the height of the entire model compared to the amount  of the volume thus making it a very fast alternative to FDM for rapid prototyping.

Our Mars 2 Pro was paired up with Chitubox Pro software which was used to make support structures.and detect islands

**3D Printing Process**
Our SLA included a more involved process from loading up the resin to acquiring our print. We first had to load up the resin in the tank, as resin was a toxic liquid, we had to make sure to use nitrile gloves to prevent any contact with the resin. For our  prints we used a Creality resin for which was had to use a 15 second layer exposure time with a 60 second bottom layer exposure time. For short test prints we used a 5 second layer exposure time as we were able to test out the dimensions and support structures of our model immediately. 

The default chitubox support structures for our print were not up to our standard to give us proper finish/support for all components, so for SLA printing we had to manually add support structures to prevent the formation of islands which ruin both our final print as well as our FEP film. 

Once the print was complete, we had to wash our print in 99% isopropyl alcohol to remove any resin that may be left in the surface of our print which may later harden to cause undesirable structures.

Lastly once we wash our print, we cure them in a curing machine, which is a machine that projects UV light into a box where our print is spinning so as to ensure all parts are equally covered.


**Manufacturing considerations**

Unlike FDM printers SLA printers are very accurate and precise, however one downside that comes with this is the requirement of "tree" support structures who deform and warp the axels hence changing the dimensions.

One solution to this problem is to keep our model tilted at a 45 degree angle as shown below, although we got better results, it was still not up to the mark. So we decided to use SLA for only the front wing and our hub caps as they were prints which required the least amount of supports. 