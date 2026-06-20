# **TSW (Tire State and Wear) Simulator**

## **Comprehensive Reference Guide**

**Version:** 1.0.0 (2026)  
**Author:** Mohammadaadil Shaikh  
**License:** Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)

### **Copyright & Legal Disclaimer**

This software and its accompanying documentation are licensed under the **CC BY-NC-ND 4.0** license. By using this simulator, you agree to the following terms:

* **Attribution:** Appropriate credit must be given to the author.  
* **NonCommercial:** The material may not be used for commercial purposes.  
* **NoDerivatives:** Modified versions of this software may not be distributed.  
* **Disclaimer:** This tool is provided "as is" for academic and virtual simulation purposes. The algorithm outputs must be independently verified before application to physical testing. The author assumes no liability for damages arising from the use of this software.

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## 

## **1\. Introduction to the Simulator**

Welcome to the **Tire State and Wear Simulator (TSWS)**.  
TSWS is a highly specialized, micro-mechanical simulation tool designed to analyze tire-road interactions at the *contact patch* level. Rather than looking at the vehicle as a whole, this software zooms in on the exact footprint where the rubber meets the road. The contact patch or the tire’s footprint is a portion of a tire that is in direct contact with the road surface.

**How it Works (The Brush Model Concept):**  
To understand the simulator, imagine the tire tread as a brush made of thousands of tiny, flexible rubber "bristles." As the tire rolls and steers, these bristles are pressed into the road *(vertical load)* and twisted *(shear force)*.

1. **Adhesion (Stick):** Initially, the bristles grip the road and bend.  
2. **Sliding (Slip):** If the twisting force becomes too great, the bristles break traction and scrape across the asphalt.  
3. **Wear:** The energy dissipated during this scraping (sliding) process physically removes rubber from the tire.

TSWS computes this entire process from bending, to slipping, to abrasive wear across thousands of individual bristles dynamically over time.

## **2\. General Workflow Map**

Using TSWS is a linear, step-by-step process. You will move through the application tabs from left to right:

1. **Start:** Welcome and initialization.  
2. **Bristle Distribution:** Define the size of the tire footprint and the bristle resolution.  
3. **Load Distribution:** Define how heavily the vehicle is pressing down on the tire.  
4. **Tire Scrub Torque:** Define rubber friction properties and upload your steering data.  
5. **Tire Shear Stress Animation:** Generate and animate dynamic, color-coded shear stress distributions under dynamic maneuvers.  
6. **Stick-Slip Animation:** Visualize real-time transitions between static adhesion (sticking) and sliding (slipping) states at the tire-road interface.  
7. **Bristle Deflection Animation:** Visualize how individual elastic rubber bristles bend and deform under the influence of steering inputs.  
8. **Tire Wear Animation:** Animate and predict localized tread depth degradation and cumulative material loss over time.

## **3\. Input Data Requirements**

Before starting the simulation, you must prepare your driving data. TSWS mainly requires steering input over time to calculate how the tire twists.

* **File Format:** Your data must be saved as a MATLAB data file (.mat).  
* **Required Variables:** Inside your .mat file, you must have two specific vectors:  
  1. A vector for steering angle (in radians).  
  2. A vector for time (in seconds).  
* **Naming Convention:** By default, the simulator expects these to be named steering\_angle and time\_sec. If your variables are named differently, you can type their exact names into the simulator before loading.  
* **Restriction:** Data should be uniform and have the same length. If not then use interpolation or other methods.  
* Other data requirements at each simulation are mentioned in the following section.

## **4\. Step-by-Step Operations Manual (Tab Guidance)**

### **Tab 1: Start (Landing Page)**

This is the initialization screen. It provides an overview of the simulator's capabilities and outputs.

* **Instructions:** Review the outputs and click the **START SIMULATION** button to unlock the first configuration step.

![][image1]

### Fig 1\. Start Tab

### 

### 

### **Tab 2: Bristle Distribution**

Here, you define the physical geometry of the tire's contact patch.

* **Half Width / Half Length \[m\]:** The physical dimensions of the tire footprint (contact patch) in meters.   
  * **Note:** Only the *half* length and *half* width of the footprint must be entered here, as the underlying simulation algorithm is strictly designed to process semi-dimensions.  
* **Bristle Length / Width \[m\]:** The dimensions of the individual elastic rubber bristle elements modeled within the tire’s contact patch. These elements represent the finite resolution of the simulation grid and must always have smaller dimensions than the overall contact patch half length and half width. Making these dimensions smaller increases the spatial resolution of the simulation but will proportionally increase the computational processing time.  
* **COR x\_c / y\_c \[m\]:** The geometric coordinates defining the Center of Rotation (COR). Physically, this represents the exact point on the tire contact patch where the steering axis (kingpin) intersects the ground. It is the solitary point on the contact patch footprint that remains completely stationary and in firm contact with the road, while the surrounding elements of the tire contact patch rotate around it during steering maneuvers.  
* **Instructions:** Enter your tire's physical dimensions. Click **RUN** to generate the initial 2D bristle grid, then click **NEXT**.  
* **Exportable Variables:**  
  * **x\_bristles:** Longitudinal Bristle coordinates of a contact patch \[m\].  
  * **y\_bristles:** Lateral Bristle coordinates of a contact patch \[m\].

### **Tab 3: Load Distribution**

This tab defines the vertical load pressing the tire into the road.

* **Type of Distribution:** Use the dropdown to select how the pressure is spread (e.g., Uniform, Parabolic, Quartic). The two-part name indicates the mathematical model mapping: the first word describes the load distribution across the *length* of the contact patch, and the second word describes the distribution across the *width*. For example, *Parabolic Taperedoff* represents a parabolic load profile across the footprint length combined with a tapered-off profile across the width.  
* **Tire Vertical Load \[N\]:** The total normal force acting on a specific tire (i.e front left tire, front right tire).  
* **Shape Parameters (Plateau Length, Gamma, Chi, nu):** These coefficients adjust the curve and shape of the load distribution.  
  * **Plateau Length:** The total width of a patch where the load distribution is not tapered off, defined in meters. To function correctly within the algorithmic constraints, this value must be larger than the footprint half-width. However, users are encouraged to experiment with this value to observe its physical impact on the pressure shape.  
  * **Gamma:** This coefficient controls the structural symmetry of the quartic load distribution across the physical length of the contact patch. The simulation environment strictly expects this parameter to be a *positive value*.  
  * **Nu:** The central load constraint parameter. This defines the exact percentage of the total load distributed across the longitudinal boundaries spanning  a \* chi to \-a \* \-chi.  
  * **Chi:** The percentage representation of the total contact length over which the central load distribution constraint remains physically valid.  
* **Instructions:** Input your load parameters and click **RUN** to visualize the pressure profile. Click **NEXT** to proceed.  
* **Exportable Variables:**  
  * **x\_bristles:** Longitudinal Bristle coordinates of a contact patch \[m\].  
  * **y\_bristles:** Lateral Bristle coordinates of a contact patch \[m\].  
  * **qz:** load distribution across length and width of the contact patch \[N/m^2\].

### **Tab 4: Tire Scrub Torque (Data Loading)**

This is the most critical configuration tab. You will define the friction properties and load your dataset here.

* **Tire Stiffness \[N/m^3\]:** The elastic shear modulus of your tire's rubber compound.  
* **mu\_adhesive / mu\_sliding:** The static and kinetic friction coefficients between your tire and the specific road surface. It should be noted that the adhesive friction coefficient is greater than the sliding friction coefficient.  
* **vector names:** Ensure these match the variables in your .mat file (e.g., steering\_angle and time\_sec).  
* **Instructions:** 1\. Input your stiffness and friction values.  
  2\. Click the magenta **Load Data** button and select your .mat file from your computer.  
  3\. Click the green **RUN** button to calculate the scrub torque.  
  4\. Once the plots generate, click **NEXT**.  
* **Exportable Variables:**  
  * **x\_bristles:** Longitudinal Bristle coordinates of a contact patch \[m\].  
  * **y\_bristles:** Lateral Bristle coordinates of a contact patch \[m\].  
  * **qz:** load distribution across length and width of the contact patch \[N/m^2\].  
  * **tire\_scrub\_t:** Total tire scrub torque at each iteration \[N . m\].  
  * **tire\_shear\_stress:** Shear stress of each bristle \[N / m^2\].  
  * **Stick\_slip\_state:** Stick slip state of each bristle. 1 for positive slip (positive steering rate), \-1 for negative slip (negative steering rate), 0 for adhesion or stick state.

### **Tab 5: Tire Shear Stress Animation**

This tab animates the shear stress building up across the contact patch as the steering wheel turns.

* **Instructions:** Click **RUN** to start the animation. You will see the stress map evolve alongside the steering angle and steering rate plots. Use the **STOP** button to pause the calculation if needed.

### **Tab 6: Bristles Stick Slip Animation**

This tab visualizes the exact moment individual bristles lose their grip and begin to slide against the asphalt.

* **Visual Legend:**  
  * **Blue:** Adhesion (The bristle is gripping the road perfectly).  
  * **Red:** Positive Sliding (The bristle is slipping in the positive steering direction).  
  * **Cyan:** Negative Sliding (The bristle is slipping in the negative steering direction).  
* **Animation Controls:** You can speed up the animation by increasing the **Skip Frames** value.  
* **Exportable Variables:**  
  * **x\_bristles:** Longitudinal Bristle coordinates of a contact patch \[m\].  
  * **y\_bristles:** Lateral Bristle coordinates of a contact patch \[m\].  
  * **qz:** load distribution across length and width of the contact patch \[N/m^2\].  
  * **tire\_scrub\_t:** Total tire scrub torque at each iteration \[N . m\].  
  * **tire\_shear\_stress:** Shear stress of each bristle \[N / m^2\].  
  * **Stick\_slip\_state:** Stick slip state of each bristle. 1 for positive slip (positive steering rate), \-1 for negative slip (negative steering rate), 0 for adhesion or stick state.  
  * **X\_deflected:** Longitudinal deflected bristle coordinates  due to steering \[m\].  
  * **Y\_deflected:** Lateral deflected bristle coordinates  due to steering \[m\].

### **Tab 7: Bristles Deflection Animation**

This tab shows the physical structural bending of the rubber elements under load.

* **Instructions:** Click **RUN** to observe how the bristles physically displace from their resting positions as scrub torque is applied.

### **Tab 8: Tire Wear Animation**

The final computational tab calculates the abrasive material loss based on the sliding energy calculated in the previous steps.

* **Wear Coeff. \[mm^3/Joule\]:** The Archard wear coefficient. This defines how easily your specific rubber compound wears away per joule of frictional energy.  
* **Outputs:** Watch the *Tire Wear Animation* to see localized tread depth loss, and monitor the *Total Tire Material Loss* plot to see the accumulated degradation over the driving cycle.  
* **Exportable Variables:**  
  * **x\_bristles:** Longitudinal Bristle coordinates of a contact patch \[m\].  
  * **y\_bristles:** Lateral Bristle coordinates of a contact patch \[m\].  
  * **qz:** load distribution across length and width of the contact patch \[N/m^2\].  
  * **tire\_scrub\_t:** Total tire scrub torque at each iteration \[N . m\].  
  * **tire\_shear\_stress:** Shear stress of each bristle \[N / m^2\].  
  * **stick\_slip\_state:** Stick slip state of each bristle. 1 for positive slip (positive steering rate), \-1 for negative slip (negative steering rate), 0 for adhesion or stick state.  
  * **x\_deflected:** Longitudinal deflected bristle coordinates  due to steering \[m\].  
  * **y\_deflected:** Lateral deflected bristle coordinates  due to steering \[m\].  
  * **bristle\_wear:** Amount of each bristle wear at each iteration \[mm^3\].  
  * **cum\_bristle\_wear:** Amount of *cumulative bristle* *wear* at each iteration \[mm^3\].  
  * **height\_loss:** Amount of height loss at iteration \[mm\].

## **5\. Exporting Data**

At any point during the simulation, you can save your results to your computer.

1. Click the **Export Output Variables** button located on the left side of the interface.  
2. The simulator will package the current arrays (such as  grids values, shear stress, torque values etc) and save them to your workspace or as a file for further processing in MATLAB or Excel.

## **6\. Example**

* **Data Source:** Actual experimental vehicle maneuver logs sourced from the P1 Garage Vehicle (Bucknell University).   
* **File Name:**  tsws\_example\_data.mat  
* **Pre-configured Variables:** This file contains pre-mapped \`steering\_angle\` and \`time\_sec\` vectors structured in the exact format required by the underlying brush model algorithm.


## **7\. Simulator Assumptions & Limitations**

To ensure accurate engineering interpretation, users must understand the physical boundaries of the TSWS brush model:

1. **No Thermal Dynamics:** The simulator assumes constant friction coefficients. In reality, aggressive sliding generates heat, which changes rubber friction. Thermal softening is not modeled here.  
2. **Stationary Steered:**  The vehicle is assumed to be placed stationary and the steering wheel is rotating left and right.   
3. **Flat Road Surface:** The model assumes a perfectly flat contact patch. It does not account for dynamic suspension changes like roll steer, camber thrust, or uneven terrain.  
4. **Abrasive Wear Only:** The simulator calculates abrasive wear based on frictional energy. It does not model structural tire failures, chunking, or tearing that might occur off-road or under extreme racing conditions.

**Note:** In future, the author will try to surpass these challenges. Stay tuned\!

**8\. Input Parameters and their Initial value**

| Input Parameters | Unit | Initial Value |
| :---- | :---- | :---- |
| Half Length | m | 0.101 |
| Half Width | m | 0.07 |
| Bristle Length | m | 0.002 |
| Bristle WIdth | m | 0.002 |
| COR x\_c | m | 0.03 |
| COR y\_c | m | 0 |
| Tire Vertical Load | N | 3600 |
| Plateau Length | m | 0.14 |
| Gamma | \- | 1 |
| Chi Minus | \- | 0.77 |
| Chi Plus | \- | 0.77 |
| nu | \- | 0.86 |
| Tire Stiffness | N/m^3 | 3.5 \* 10^7 |
| mu\_adhesive | \- | 0.55 |
| mu\_sliding | \- | 0.66 |
| Wear Coefficient | mm^3/ Joule | 1.5\*10^-3 |

**9\. Exportable Variables**

Here, 

* j \= number of bristles in longitudinal direction  
* k \= number of bristles in longitudinal direction  
* i \= number of iteration point  


| Variable Name | Size |
| :---- | :---- |
| x\_bristles | j \* k |
| y\_bristles | j \* k |
| qz | j \* k \* i |
| tire\_scrub\_t | i \* 1 |
| tire\_shear\_stress | j \* k \* i |
| stick\_slip\_state | j \* k \* i |
| x\_deflected | j \* k \* i |
| y\_deflected | j \* k \* i |
| bristle\_wear | j \* k \* i |
| cum\_bristle\_wear | j \* k \* i |
| height\_loss | i \* 1 |

**10\. Installation Requirements**

* MATLAB 2022a or after

[image1]: <data:image/png;base64,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>