# 🍎 Automated Fruit Defect Detection Pipeline

## 📖 Overview
This project is an automated, classical computer vision pipeline designed to detect agricultural defects (rot, mold, bruising) across a wide variety of fruits. 

Unlike Deep Learning approaches that require thousands of training images, this system uses an explainable, rule-based algorithm. It dynamically adjusts its camera filters and grading rubrics based on the specific type of fruit it is analyzing, making it highly efficient and easy to debug.

## ✨ Features
* **Dynamic HSV Segmentation:** Automatically switches color thresholding rules based on the fruit type to isolate healthy tissue and ignore background noise.
* **Texture Analysis (ORB):** Utilizes the Oriented FAST and Rotated BRIEF (ORB) algorithm to detect sharp, jagged textures commonly caused by fungal mold or rot.
* **Shape Analysis (Circularity):** Calculates the mathematical circularity of the extracted mask. Since defects "eat" into the healthy color mask, defective fruits present highly jagged perimeters, drastically lowering their circularity score.
* **"VIP List" Rule Engine:** A custom thresholding classifier that applies unique Pass/Fail rules for each fruit type, accommodating natural anomalies (like pineapple spikes or dragonfruit scales).

## 🍇 Supported Fruits
The pipeline currently supports and accurately classifies:
* Strawberries 🍓
* Mangoes 🥭
* Guavas 🍏
* Pineapples 🍍
* Dragonfruits 🐉
* Pears 🍐

## 🛠️ Prerequisites
To run this pipeline, you will need Python 3.x and the following libraries installed:
```bash
pip install opencv-python numpy matplotlib

📂 Directory Structure
Ensure your project folder is set up exactly like this before running the script:
project_folder/
│
├── Main.ipynb                  # The main Jupyter Notebook / Python script
├── README.md                   # This file
└── img/                        # Directory containing all fruit images
    ├── Normal_Mango.jpg
    ├── Defect_Mango.jpg
    ├── Defect_Strawberry.jpg
    └── ...

💻 Usage
Run the main script or execute the cells in Main.ipynb.
The script will automatically target the img/*.jpg directory, process every image in a batch, and print a terminal log of its decisions.
Example Output:
-> Result: Mango is NORMAL (Circularity: 0.78, Keypoints: 45)
 -> Result: Mango is DEFECTIVE (Circularity: 0.17, Keypoints: 186)
 -> Result: Pineapple is NORMAL (Circularity: 0.21, Keypoints: 50)
 -> Result: Pineapple is DEFECTIVE (Circularity: 0.19, Keypoints: 2000)

==============================
FINAL EXAM REPORT
==============================
Total Objects: 42
Defective Objects: 6
Average Area: 521 pixels
Defect Percentage: 14.29%

🔧 Tuning and Configuration
If you wish to add a new fruit to the pipeline, you must update two sections in the code:

In preprocess_and_segment(): Add an elif statement with the exact Lower and Upper HSV array bounds to isolate the new fruit's healthy color.

In the Main Loop (Part 3): Add an elif rule to the "VIP List" that defines the minimum acceptable Circularity and maximum acceptable ORB Keypoints for that specific fruit.