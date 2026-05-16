AI Personal Trainer

A deep learning and machine learning system that analyzes exercise form from video input using pose estimation and temporal sequence modeling.

The project detects:

The exercise being performed
Whether the form is correct or incorrect

The system was trained on a custom dataset created entirely from scratch, using videos recorded and labeled manually.

Overview

Incorrect exercise form is one of the most common causes of workout-related injuries, especially for people training without supervision.

This project aims to reduce that problem by building an AI-powered personal trainer capable of analyzing body posture and movement over time.

The system processes exercise videos, extracts human pose landmarks using MediaPipe, computes biomechanical features such as joint angles and angular velocities, and predicts:

The exercise type
The correctness of the performed form

Unlike standard image-based classifiers, this project focuses heavily on temporal understanding. Many exercise mistakes are not visible in a single frame and only appear throughout the movement sequence itself.

For example:

An incomplete pushup may look correct in isolated frames
A shallow squat may contain visually correct poses, but never reaches full depth

This makes temporal modeling essential.

Features
Pose extraction using MediaPipe
Joint angle and angular velocity computation
Exercise classification
Form correctness classification
Real-time webcam inference support
Machine Learning pipeline
Deep Learning sequence models
Video-level and window-level prediction
Temporal sequence analysis
Custom dataset generation pipeline
Exercises Supported

Current supported exercises:

Pushups
Squats
Pullups
Russian Twists

Each exercise contains:

Correct form examples
Incorrect form examples
Dataset

Since no suitable dataset existed for this exact task, the dataset was created manually.

Dataset Statistics
103 recorded videos
~29,000 extracted labeled frames
Multiple correct and incorrect forms
Custom preprocessing pipeline
Pose Features

The system extracts 26 biomechanical features from each frame.

Joint Angles
Left elbow angle
Right elbow angle
Left shoulder angle
Right shoulder angle
Left hip angle
Right hip angle
Left knee angle
Right knee angle
Left ankle angle
Right ankle angle
Body Rotation Features
Shoulder Z difference
Hip Z difference
Torso rotation
Angular Velocity Features
Elbow angular velocities
Shoulder angular velocities
Hip angular velocities
Knee angular velocities
Ankle angular velocities
Torso rotation velocity
Project Pipeline
1. Video Collection

Videos were manually recorded for:

Correct form
Incorrect form
Different repetitions
Different movement speeds
2. Pose Extraction

MediaPipe Pose is used to detect body landmarks frame-by-frame.

From these landmarks:

Joint angles are computed
Rotations are calculated
Temporal velocities are extracted
3. Dataset Generation

Extracted features are converted into:

Tabular datasets for ML
Sequential datasets for DL

Labels are automatically inferred from folder structure:

Exercise type
Correctness label
4. Preprocessing

The preprocessing pipeline includes:

Null value checks
Duplicate removal
Outlier filtering
Velocity threshold filtering
Label encoding
Feature scaling
Distribution analysis

Special care was taken to avoid data leakage.

Instead of random frame splitting, videos were grouped so frames from the same video remain inside the same subset.

Machine Learning Models
Random Forest

Used for:

Exercise classification
Form correctness classification
Why Random Forest?
Performs well on tabular numerical data
Handles nonlinear relationships
Robust to noisy features
Good baseline model
Support Vector Machine (SVM)

Used for:

Exercise classification
Form correctness classification
Why SVM?
Effective in high-dimensional spaces
Strong classification boundaries
Works well with structured numerical features
Deep Learning Models

Traditional ML models struggled with temporal mistakes such as incomplete range of motion.

To solve this, sequence-based deep learning architectures were introduced.

LSTM with Attention
Architecture
Sequence length: 90 frames
Input shape: [batch, sequence, features]
Unidirectional LSTM
Bahdanau-style attention mechanism
Shared feature trunk
Multi-head classification output
Why LSTM?

LSTMs naturally model sequential motion over time.

Exercises are not defined by single poses, but by how poses evolve across an entire repetition.

The attention mechanism helps the model focus on the most important part of the movement.

Temporal Convolutional Network (TCN)
Architecture
Dilated causal convolutions
Residual blocks
Weight normalization
Temporal receptive field expansion
Why TCN?

TCNs process sequences in parallel and offer a strong alternative to recurrent architectures.

The project compares:

Recurrent temporal modeling (LSTM)
Convolutional temporal modeling (TCN)
Training Strategy
Data Splits
Training: 70%
Validation: 15%
Testing: 15%
Sequence Windowing

Videos are sliced into temporal windows.

Window size: 90 frames
Zero padding applied
Randomized padding placement

This prevents the model from learning fixed repetition alignment.

Augmentation

Gaussian noise is added during training to simulate:

Camera instability
Pose estimation noise
Lighting variations
Optimization
Optimizer: AdamW
LR Scheduler: ReduceLROnPlateau
Early stopping enabled
Gradient clipping enabled
Results
Machine Learning
Exercise Classification
Random Forest and SVM achieved ~99% F1-score
Correctness Classification
Traditional ML models achieved around 60-66% F1-score

The main limitation:
ML models analyze individual frames only.

Temporal mistakes cannot be reliably detected frame-by-frame.

Deep Learning
LSTM Results
Exercise F1-score: ~1.00
Correctness F1-score: ~0.94
TCN Results
Exercise F1-score: ~0.98
Correctness F1-score: ~0.89

The LSTM consistently outperformed the TCN on correctness prediction.

Key Finding

Exercise correctness is fundamentally a temporal problem.

A single frame often cannot determine:

Range of motion
Repetition completion
Movement consistency

Sequence-based models significantly outperform frame-based classifiers for this task.

Real-Time Inference

The project also supports live webcam inference.

Pipeline:

Webcam capture
MediaPipe pose extraction
Feature generation
Sequence buffering
Model inference
Real-time prediction display
Technologies Used
AI / ML
Python
PyTorch
Scikit-learn
NumPy
Pandas
Computer Vision
OpenCV
MediaPipe
Visualization
Matplotlib
Seaborn
Repository Structure
project/
│
├── data/
│   ├── raw_videos/
│   ├── processed/
│   └── sequences/
│
├── notebooks/
│   ├── preprocessing.ipynb
│   ├── ml_models.ipynb
│   └── dl_models.ipynb
│
├── models/
│   ├── random_forest.pkl
│   ├── svm.pkl
│   ├── lstm.pt
│   └── tcn.pt
│
├── src/
│   ├── feature_extraction.py
│   ├── preprocessing.py
│   ├── training.py
│   ├── inference.py
│   └── webcam_demo.py
│
└── README.md
Limitations
Small dataset size
Limited camera angles
Limited exercise variety
Limited incorrect form variations
Overfitting risk in DL models
Not yet generalized to all body types or environments
Future Improvements
Larger dataset
More exercise categories
More incorrect form variations
Multi-angle support
Mobile deployment
Injury-risk estimation
Personalized feedback generation
Rep counting
Real-time coaching suggestions
Authors
Pio Aouad
Ghadi Khoury
Acknowledgements
MediaPipe Pose
PyTorch
Scikit-learn
OpenCV
