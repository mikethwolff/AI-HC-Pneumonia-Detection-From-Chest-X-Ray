# FDA  Submission

**Your Name:** 
Michael Wolff

**Name of your Device:** 
Chest X-Ray Pneumonia Detection

## Algorithm Description 

### 1. General Information

**Intended Use Statement:**  
Predict the presence of pneumonia and support radiologists in classifying pneumonia in chest X-rays

**Indications for Use:** 
- Applicable for men or women of all ages
- Chest X-Ray image must be taken in the AP(anterior to posterior) or PA(posterior to anterior) position
- Chest X-Ray image must be in DICOM format

**Device Limitations:**
- Predictions can be made with a standard CPUs, GPUs can significantly speed up the process

**Clinical Impact of Performance:**
- The model is most confident when the test result is negative

### 2. Algorithm Design and Function

!(https://github.com/mikethwolff/Pneumonia-Detection-From-Chest-X-Ray/blob/main/pneumonia_detection_workflow.png?raw=true)

**DICOM Checking Steps:**
- 'BodyPartExamined' examined is "CHEST"
- 'PatientPosition' attribute must be either "AP" or "PA"
- 'Modality' attribute must be "DX"

**Preprocessing Steps:**
- Image is normalized
- Image is reshaped
- Image is repeated across 3 channels

**CNN Architecture:**
!(cnn_architecture_w.png)
- The model is based on the VGG16 model
- The VGG16 model output is flattened and passed through several additional dense and dropout layers

### 3. Algorithm Training

**Parameters:**
- Types of augmentation used during training
  - Horizontal flips
  - No Vertical flips
  - Height shift range of 0.1
  - Width shift range of 0.1
  - Rotation range of 25
  - Shear range of 0.1
  - Zoom range of 0.15
- Batch size: 64
- Optimizer learning rate: 1e-4
- Layers of pre-existing architecture that were frozen: First 16 layers of VGG model
- Layers of pre-existing architecture that were fine-tuned: None
- Layers added to pre-existing architecture: Flatten, Dense and Dropout layers

!(training_performance.png)

!(precision_recall_curve.png)

**Final Threshold and Explanation:**

### 4. Databases

- The Dataset is taken from Kaggle: [NIH Chest X-rays](https://www.kaggle.com/datasets/nih-chest-xrays/data)
  - The dataset contains over 112,000 frontal-view chest X-ray images (1024*1024 resolution) from more than 30,000 unique patients
  -  Meta data for all images: 
    - Image Index
    - Finding Labels
    - Follow-up #
    - Patient ID
    - Patient Age
    - Patient Gender
    - View Position
    - Original Image Size
    - Original Image Pixel Spacing

**Description of Training Dataset:** 

- The training data is split equally between Pneumonia and non Pneumonia patients
- It contains 2290 images

**Description of Validation Dataset:** 

- The training data has 20% Pneumonia and 80% non Pneumonia patients
- It contains 1430 images

### 5. Ground Truth

- The disease labels were created using Natural Language Processing (NLP) to mine the associated radiological reports
- The image labels are NLP extracted
- The NLP labelling accuracy is estimated to be >90%
  - NIH Chest X-ray Dataset of 14 Common Thorax Disease Categories:
    - Atelectasis
    - Cardiomegaly
    - Effusion
    - Infiltration
    - Mass
    - Nodule
    - Pneumonia
    - Pneumothorax
    - Consolidation
    - Edema
    - Emphysema
    - Fibrosis
    - Pleural_Thickening
    - Hernia

### 6. FDA Validation Plan

**Patient Population Description for FDA Validation Dataset:**
Applicable for men or women of all ages. Chest X-Ray image must be taken in the AP or PA position and comply to DICOM format.

**Ground Truth Acquisition Methodology:**
Compute the F1 score for each individual radiologist and the model against each of the 4 labels as ground truth.

**Algorithm Performance Standard:**
This [paper](https://arxiv.org/pdf/1711.05225) compares radiologists and our model on the F1 metric, which is the harmonic average of the precision and recall of the models. CheXNet achieves an F1 score of 0.435 (95% CI 0.387, 0.481), higher than the radiologist average of 0.387 (95% CI 0.330, 0.442). We use the bootstrap to find that the difference in performance is statistically significant.
