## AI for Healthcare
The listed project is part of the AI for Healthcare Nanodegree (Udacity).

# Pneumonia Detection From Chest X-Ray
This project culminates in a model that can classify a given chest x-ray for the presence or absence of pneumonia.

![pneumonia_detection_examples](https://github.com/mikethwolff/AI-HC-Pneumonia-Detection-From-Chest-X-Ray/assets/8941220/875f5c36-6d43-4abe-a36b-73c5657f15d2)

## This project consist of four steps:

1. Exploratory Data Analysis
2. Building and Training Your Model
3. Clinical Workflow Integration
4. FDA Preparation

## The goal of this project, upon it´s completion, is to be able to:

- recommend appropriate imaging modalities for common clinical applications of 2D medical imaging
- perform exploratory data analysis (EDA) on medical imaging data to inform model training and explain model performance
- establish the appropriate ‘ground truth’ methodologies for training algorithms to label medical images
- extract images from a DICOM dataset
- train common CNN architectures to classify 2D medical images
- translate outputs of medical imaging models for use by a clinician
- plan necessary validations to prepare a medical imaging model for regulatory approval

## Dataset

The dataset contains over 112,000 frontal-view chest X-ray images (1024*1024 resolution) from more than 30,000 unique patients.
The Dataset is taken from Kaggle: [NIH Chest X-rays](https://www.kaggle.com/datasets/nih-chest-xrays/data)

**Description of Training Dataset:** 

- The training data is split equally between Pneumonia and non Pneumonia patients
- It contains 2290 images

**Description of Validation Dataset:** 

- The validation data has 20% Pneumonia and 80% non Pneumonia patients
- It contains 1430 images

## Algorithm Design and Function

**DICOM Checking Steps:**
- 'BodyPartExamined' examined is "CHEST"
- 'PatientPosition' attribute must be either "AP" or "PA"
- 'Modality' attribute must be "DX"

**Preprocessing Steps:**
- Image is normalized
- Image is reshaped
- Image is repeated across 3 channels

**CNN Architecture:**
- The model is based on the VGG16 model
- The VGG16 model output is flattened and passed through several additional dense and dropout layers

# 1. Exploratory Data Analysis
The first part of this project will involve exploratory data analysis (EDA) to understand and describe the content and nature of the data.

Some important things to focus on during the EDA may be:

- The patient demographic data such as gender, age, patient position,etc. (as it is available)
- The x-ray views taken (i.e. view position)
- The number of cases including:
- Number of pneumonia cases,
- Number of non-pneumonia cases
- The distribution of other diseases that are comorbid with pneumonia
- Number of disease per patient
- Pixel-level assessments of the imaging data for healthy & disease states of interest (e.g. histograms of intensity values) and compare distributions across diseases.

Find the EDA [here](https://github.com/mikethwolff/AI-HC-Pneumonia-Detection-From-Chest-X-Ray/blob/main/EDA.ipynb).

# 2. Building and Training Your Model

## Training and validating Datasets

From the findings in the EDA component of this project, we curate the appropriate training and validation sets for classifying pneumonia. We asure to take the following into consideration:

- Distribution of diseases other than pneumonia that are present in both datasets
- Demographic information, image view positions, and number of images per patient in each set
- Distribution of pneumonia-positive and pneumonia-negative cases in each dataset
- Model Architecture

In this project, we fine-tune an existing CNN architecture to classify x-rays images for the presence of pneumonia. There is no archictecture required for this project, but a reasonable choice would be using the VGG16 architecture with weights trained on the ImageNet dataset. Fine-tuning can be performed by freezing the chosen pre-built network and adding several new layers to the end to train, or by doing this in combination with selectively freezing and training some layers of the pre-trained network.

## Training

In training our model, there are many parameters that can be tweaked to improve performance including:

- Image augmentation parameters
- Training batch size
- Training learning rate
- Inclusion and parameters of specific layers in your model


## Performance Assessment

As we train our model, we will monitor its performance over subsequence training epochs. We choose the appropriate metrics upon which to monitor performance. Note that 'accuracy' may not be the most appropriate statistic in this case, depending on the balance or imbalance of your validation dataset, and also depending on the clinical context that you want to use this model in (i.e. can you sacrafice high false positive rate for a low false negative rate?)

Note: Detecting pneumonia is hard even for trained expert radiologists. [This paper](https://arxiv.org/pdf/1711.05225.pdf) describes some human-reader-level F1 scores for detecting pneumonia, and can be used as a reference point for how well your model could perform.

# 3. Clinical Workflow Integration

The imaging data provided to you for training your model was transformed from DICOM format into .png to help aid in the image pre-processing and model training steps of this project. In the real world, however, the pixel-level imaging data are contained inside of standard DICOM files.

For this project, we create a DICOM wrapper that takes in a standard DICOM file and outputs data in the format accepted by your model. We assure to include several checks in your wrapper for the following:

- Proper image acquisition type (i.e. X-ray)
- Proper image acquisition orientation (i.e. those present in your training data)
- Proper body part in acquisition

# 4. FDA Submission

For this project, we will complete the steps from the FDA's official guidance on both the algorithm description and the algorithm performance assessment.
