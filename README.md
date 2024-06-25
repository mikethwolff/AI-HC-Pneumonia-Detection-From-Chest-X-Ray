## AI for Healthcare
The listed project is part of the AI for Healthcare Nanodegree (Udacity).

# Pneumonia-Detection-From-Chest-X-Ray
This project culminates in a model that can classify a given chest x-ray for the presence or absence of pneumonia.

![pneumonia_detection_examples](https://github.com/mikethwolff/AI-HC-Pneumonia-Detection-From-Chest-X-Ray/assets/8941220/875f5c36-6d43-4abe-a36b-73c5657f15d2)
(Example images)

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

