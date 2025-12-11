# UR10 Robotic Portrait Drawing

This project demonstrates an end-to-end workflow for transforming an **AI-generated portrait** into a **physical drawing created by a UR10 robotic arm**. The pipeline combines **Rhino**, **Grasshopper**, **Image Sampler**, and a **custom Python → URScript converter**, enabling the robot to sketch halftone-style artwork based on brightness-mapped points.

---

## 📌 Workflow

![Workflow](https://raw.githubusercontent.com/maduwanthasl/UR10-AI-Art-Drawing/refs/heads/main/Images/Workflow.png)

---

### **1. Image Generation (AI)**

Image of initial concept developed using GPT.

The produced output serves as a basis for artwork.

---

### **2. Point Generation in Rhino + Grasshopper**

Applying a similar approach as that in “Making Portraits with Image Sampler” ([based upon the related tutorial 
video](https://www.youtube.com/watch?v=dhiCwFMi1Ig)), I used Grasshopper to transform this portrait into a defined set of draw-points:

- The input portrait is linked to **Image Sampler**.
- A **grid of points** is established in a bounded area.
- Every point in the grid reads a pixel brightness value from the image.
- The radius of each circle (point activation) is mapped to brightness:
  - **Darker regions → larger circles**
  - **Lighter regions → smaller circles or skipped**
- This produces a **halftone portrait**.
  
The Grasshopper definition provides a clean set of **2D coordinates** that make up the portrait.

---

### **3. Geometry Export**

- A flattened list of all processed **(x, y)** coordinates is exported from Grasshopper.
- Points are **scaled and aligned** to match the UR10 workspace and drawing plane.

---

### **4. Custom Python Conversion Script**

A Python script converts the point list into **valid URScript commands**:

- Reads raw 2D coordinate values.
- Applies robot-specific offsets, tool Z-height, and speed parameters.
- Generates sequential `movej` / `movel` robot motion commands.
- Outputs a `.script` file directly uploadable to the UR10 controller.

---

### **5. UR10 Robot Drawing Execution**

- The `.script` file is sent to the UR10 controller.
- The robot follows the generated point sequence.
- The drawing is recreated on paper in halftone form.

**Final result: a physical portrait drawn entirely through robotic motion.**

---
## Side Experiments
![Side Experiments](https://raw.githubusercontent.com/maduwanthasl/UR10-AI-Art-Drawing/refs/heads/main/Images/Side_Experiments.png)

##  Special Thanks

A special thanks to **Dr. Thileepan Stalin** for his guidance and support throughout the development of this project.



