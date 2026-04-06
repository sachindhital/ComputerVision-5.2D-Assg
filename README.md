# Azure AI: Custom Vision Vehicle Detection & Video Tracking

This project implements an end-to-end computer vision solution using **Azure AI Custom Vision**. It transitions from static image classification and object detection to a dynamic video processing pipeline that annotates vehicle movements in real-time[cite: 1415, 1553].

## 📋 Project Overview
The system is trained to identify and locate specific vehicle types—**Bus, Car, Motorcycle, and Truck**—within images and video files.

### Key Features
* **Automated Dataset Ingestion**: Integration with `kagglehub` to download the "Vehicle Type Recognition" dataset directly to the local project environment.
* **Hybrid Detection Logic**: Uses the Azure Computer Vision API to detect initial bounding box regions before passing them to the Custom Vision trainer.
* **Video Processing Pipeline**: A complete workflow that deconstructs video into frames, applies object detection to each, and reassembles them into an annotated `.mp4` file.
* **Performance Analysis**: Detailed evaluation of model accuracy, limitations in multi-object scenes, and a roadmap for potential improvements.

---

## 🛠️ Tech Stack & Requirements
* **Language**: Python 3.12 
* **Azure Services**: Custom Vision (Training & Prediction)  and Computer Vision Client.
* **Key Libraries**:
    * `azure-cognitiveservices-vision-customvision`: For model training and inference.
    * `opencv-python`: For video frame extraction and video writing.
    * `Pillow (PIL)`: For image manipulation and drawing bounding boxes.
    * `msrest`: For API authentication and credentials management.

---

## 🚀 Project Workflow

### 1. Resource Setup & Authentication
The project initializes by connecting to Azure Custom Vision using a specific **Endpoint** and **Training/Prediction Keys** managed via environment variables.

### 2. Project Initialization
A new Object Detection project is created using the "General" domain via the `CustomVisionTrainingClient`.

### 3. Data Preparation & Fallback Tagging
* **Tagging**: Primary tags are established for Bus, Car, Motorcycle, and Truck.
* **Detection Logic**: The system attempts to find matching regions using Computer Vision
* **Fallback Strategy**: If no region matches the category, a "full-image fallback" (0,0 to 1,1) is applied to ensure the image remains useful for training
### 4. Training and Publication
The model is trained until the iteration status reaches "Completed". It is then published to a prediction endpoint (e.g., `vehicle-detector`) for live inference.

### 5. Video Inference Pipeline
1.  **Extraction**: Video is converted to individual JPG frames.
2.  **Prediction**: Each frame is processed by the `predictor.detect_image` function.
3.  **Annotation**: Bounding boxes and labels (e.g., `bus (79.8%)`) are drawn onto the frames using `ImageDraw`.
4.  **Reconstruction**: Annotated frames are re-encoded into a final `.mp4` video at 20 FPS.

---

## 📊 Evaluation & Limitations
* **Accuracy**: The model demonstrates reasonable accuracy when objects are clearly visible and similar to the limited training set.
* **Limitations**: The model struggles with multi-object scenes and can produce false positives in cluttered environments due to the lack of diverse training data.
* **Future Roadmap**: Potential enhancements include data augmentation (rotation/flipping), increasing the dataset size, and exploring edge deployment for real-time performance

---
