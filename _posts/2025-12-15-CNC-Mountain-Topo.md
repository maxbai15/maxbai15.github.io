# Creating a Topographic Mountain Model: Terrain2STL to Makera Carvera ATC

This post details the end-to-end workflow for transforming real-world terrain data into a physical 3D model using a combination of **Terrain2STL**, **Vectric Aspire 12.5**, and the **Makera Carvera ATC CNC** machine.


## Project Showcase



*(Image: The finished CNC-carved topographic mountain model, showcasing the vertical relief and detail.)*

## Workflow: Topographic Maps Creation

The process is divided into three main phases: Data Acquisition (Terrain2STL), CAD/CAM (Aspire), and CNC Machining (Carvera).

### Part 1: Data Acquisition with Terrain2STL

Terrain2STL is a simple web tool for extracting stereolithography (.STL) files from global terrain data.

1.  **Access and Location:** Navigate to [https://jthatch.com/Terrain2STL/](https://jthatch.com/Terrain2STL/). Click **Center to View** to place the initial red selection box.
2.  **Define the Model Area:** Pan and drag the red box to precisely cover the mountainous area you wish to model.
    
3.  **Adjust Model Settings:** Crucial adjustments for a mountain model:
    * **Vertical Scaling (Z-Scale):** This is essential. I used a value of **2.5** to exaggerate the mountains and valleys, making the relief stand out clearly on a smaller physical model. *(The default of 1 often results in too-subtle relief.)*
    * **Base Height (mm):** Set to a small value, like **1.0 mm**, to ensure the model has a solid base for CNC carving.
4.  **Generate and Download:** Click **Generate Model**. The website will process the data and download a `.ZIP` file containing the `.stl` file.

### Part 2: CAD/CAM Setup in Aspire Vectric 12.5

This phase prepares the model for cutting and generates the toolpaths.

#### Phase 1: Job Setup & STL Import

1.  **Create New File & Define Stock:**
    * Set **Job Type** to **Single Sided**.
    * Set **Job Size (X, Y)** and **Thickness (Z)** to match your physical stock material (e.g., X=5 inches, Y=7 inches, Z=1 inch).
    * **Z Zero Position:** Set to **Material Surface** (essential for the Carvera).
    * **XY Datum Position:** Set to **Bottom Left**.
    
2.  **Import & Orient 3D Model:**
    * Go to the **Modeling** tab and click **Import a Component or 3D Model**. Select your `.stl` file.
    * In the **3D Orientation** window, set **Initial Orientation** to **Top**.
    * **Model Size:** **Uncheck the lock** to adjust the Z-size independently. Set the Z height to a value **less than or equal to** your stock thickness (e.g., Z=0.95 inches for 1-inch stock).
    * Click **Apply**, **Center Model**, and then **Position and Import**.

#### Phase 2: Sizing and Base Adjustments

1.  **Component Properties:** In the **Component Tree**, right-click the imported component and select **Properties**.
    * Set **Shape Height** to the final desired height (e.g., **1.0 inch**).
    * Set **Base Height** to **0.0** to push the lowest part of the relief right down to the bottom of the modeled component area. This maximizes the model's presence within the stock material.

#### Phase 3: Positioning & Creating a Boundary

1.  **Center the Component:** Switch to the **2D View** (Design Tab), select the model, and use the **Center** tool to align it with the stock.
2.  **Create Boundary Vector:** Use the **Draw Rectangle** tool to create a vector that outlines the final dimensions of your piece. This vector will be used for both machining limits and the final profile cut.

#### Phase 4: 3D Toolpath Generation (CAM)

We use a Roughing pass to clear the bulk, followed by a Finishing pass for detail.

1.  **3D Roughing Pass:**
    * **Tool:** 1/8" Flute End Mill (Tool #1 for the Carvera ATC).
    * **Machining Limit Boundary:** Select the Model Boundary (the vector you created).
    * **Machining Allowance:** Set to **0.024"** to leave material for the finer finishing bit.
    * **Strategy:** Z Level.
2.  **3D Finishing Pass:**
    * **Tool:** 1/8" Ball Nose (Tool #6 for the Carvera ATC). *(A smaller tool would add more detail, but this balances detail and time for the initial run.)*
    * **Strategy:** Raster, set at a **0-degree angle** to run along the grain of the wood.
    

#### Phase 5: 2D Profile Toolpath Generation

This final pass cuts the finished model out of the stock.

1.  **2D Profile Toolpath:**
    * **Tool:** 1/8" Flute End Mill (Tool #1).
    * **Cut Depth:** Set to **0.5 inch** (or a desired depth to create a small base).
    * **Machine Vectors:** Select **On the line**.
    * **Note:** If you intend to cut all the way through the stock, ensure you use the proper **Cut Depth** and add **Tabs** (I opted for a partial cut and used a bandsaw for final separation).

#### Phase 6: Simulation & Exporting

1.  **Preview All Toolpaths:** Click the **Preview all Toolpaths** button. Critically examine the simulation. Does the relief look right? Are there any missed areas or unexpected cuts?
    
2.  **Save G-Code:**
    * Click **Save Toolpaths**.
    * Select all three toolpaths (**3D Rough**, **3D Finish**, **2D Profile**).
    * Choose the **Carvera ATC (mm) (\*cnc)** post-processor. This combines the cuts into a single file for automated tool changes on the Carvera.

---

## Challenge & Resolution

Today I started trying to mill my topography graph. I uploaded the file into makeraCam, but it appeared upside down at first, but after going back to the aspire file everythign seemed fine. In the end, the upside down showing up on makeraCam was just an illusion. Additionally, when starting to cut it was in the wrong spot because the machine was set to anchor 2 instead of anchor 1.


## Project Summary

### What I Learned

The most critical takeaway is the relationship between the digital design and the physical material constraints. Specifically:

* **Z-Scale is Key:** Exaggerating the Z-axis in Terrain2STL is not just a preference; it is a necessity for achieving clear topographic visibility on a small-scale CNC model.
* **Aspire's Base Height:** Understanding the **Base Height** and **Shape Height** properties in Aspire is crucial for correctly positioning the 3D component relative to the stock surface, preventing material from being cut off or wasted.

### What I Would Do Differently

* **Smaller Ball Nose:** For a more detailed finish, I would upgrade to a 1/16" or even 0.8mm Ball Nose bit for the Finishing Pass. While it increases machine time, the contour lines and subtle elevation changes would be rendered more accurately.
* **Contour Lines:** I plan to incorporate a V-carve toolpath to engrave the topographic contour lines onto the surface *after* the 3D finishing pass. This would add a beautiful, map-like layer of detail.

### Future Plans

My next project is to machine a larger version of this mountain, adding the **engraved contour lines** as mentioned above, and integrating an LED light strip into the base to create a unique backlit "mountain lamp" effect.

---

## Project Files

You can find the necessary project files below for review and replication:


* [`TOPO_MOUNTAIN_PROJECT.crv3d`](/assets/files/MaxBaiTopo.crv3d) - The native Aspire project file.
* [`TOPO_MOUNTAIN_TOOLPATHS.cnc`](/assets/files/MaxBaiTopo.cnc) - The Makera Carvera ATC G-Code file (combines Rough, Finish, and Profile).
* [`STL_Mountain_Export.stl`](/assets/files/terrain-92631.stl) - The raw 3D model exported from Terrain2STL.
