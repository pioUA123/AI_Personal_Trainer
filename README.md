# AI Personal Trainer

## **Overview**

Incorrect exercise form is one of the leading causes of workout-related injuries, especially for people training without supervision.
This project aims to solve that problem by building an AI-powered personal trainer capable of analyzing exercise form from video input.

The system processes videos, extracts human pose landmarks using MediaPipe, computes biomechanical features such as joint angles and angular velocities, and predicts:

* The exercise being performed
* Whether the form is correct or incorrect

Unlike standard image classifiers, this project focuses heavily on **temporal understanding**. Many exercise mistakes are not visible in a single frame and only appear throughout the movement sequence itself.

For example:

* An incomplete pushup may contain frames that individually look correct
* A shallow squat may contain valid poses without ever reaching proper depth

This makes temporal sequence modeling essential.

---

# **Features**

* Exercise classification
* Form correctness classification
* Pose extraction using MediaPipe
* Joint angle computation
* Angular velocity extraction
* Machine Learning pipeline
* Deep Learning temporal sequence models
* Real-time webcam inference
* Video-level prediction aggregation
* Custom dataset generation pipeline

---

# **Supported Exercises**

Current supported exercises:

* Pushups
* Squats
* Pullups
* Russian Twists

Each exercise contains:

* Correct form examples
* Incorrect form examples

---

# **Dataset**

No suitable public dataset existed for this exact task, so the dataset was created manually.

## **Dataset Statistics**

* 103 recorded videos
* ~29,000 extracted labeled frames
* Correct and incorrect form samples
* Custom preprocessing pipeline

---

# **Pose Features**

The system extracts 26 biomechanical features from each frame.

## **Joint Angles**

* Left elbow angle
* Right elbow angle
* Left shoulder angle
* Right shoulder angle
* Left hip angle
* Right hip angle
* Left knee angle
* Right knee angle
* Left ankle angle
* Right ankle angle

## **Body Rotation Features**

* Shoulder Z difference
* Hip Z difference
* Torso rotation

## **Angular Velocity Features**

* Elbow angular velocities
* Shoulder angular velocities
* Hip angular velocities
* Knee angular velocities
* Ankle angular velocities
* Torso rotation velocity

---

# **Project Pipeline**

## **1. Video Collection**

Videos were manually recorded for:

* Correct form
* Incorrect form
* Different repetition styles
* Different movement speeds

---

## **2. Pose Extraction**

MediaPipe Pose is used to detect body landmarks frame-by-frame.

From these landmarks:

* Joint angles are computed
* Rotations are calculated
* Angular velocities are extracted

---

## **3. Dataset Generation**

Extracted features are converted into:

* Tabular datasets for ML
* Sequential datasets for DL

Labels are automatically inferred from folder structure:

* Exercise type
* Correctness label

---

## **4. Preprocessing**

The preprocessing pipeline includes:

* Null value checks
* Duplicate removal
* Outlier filtering
* Velocity threshold filtering
* Label encoding
* Feature scaling
* Distribution analysis

Special care was taken to avoid data leakage.

Instead of random frame splitting, videos were grouped so that all frames from the same video remain inside the same subset.

---

# **Machine Learning Models**

## **Random Forest**

Used for:

* Exercise classification
* Form correctness classification

### **Why Random Forest?**

* Performs well on tabular numerical data
* Handles nonlinear relationships
* Robust to noisy features
* Strong baseline model

---

## **Support Vector Machine (SVM)**

Used for:

* Exercise classification
* Form correctness classification

### **Why SVM?**

* Effective in high-dimensional feature spaces
* Strong classification boundaries
* Works well with structured numerical features

---

# **Deep Learning Models**

Traditional ML models struggled with temporal mistakes such as incomplete range of motion.

To solve this, sequence-based deep learning architectures were introduced.

---

## **LSTM with Attention**

### **Architecture**

* Sequence length: 90 frames
* Unidirectional LSTM
* Bahdanau-style attention mechanism
* Shared feature trunk
* Multi-head classification output

### **Why LSTM?**

LSTMs naturally model sequential motion over time.

Exercises are not defined by single poses, but by how poses evolve across an entire repetition.

The attention mechanism helps the model focus on the most informative parts of the movement.

---

## **Temporal Convolutional Network (TCN)**

### **Architecture**

* Dilated causal convolutions
* Residual blocks
* Weight normalization
* Temporal receptive field expansion

### **Why TCN?**

TCNs process sequences in parallel and provide a strong alternative to recurrent architectures.

The project compares:

* Recurrent temporal modeling (LSTM)
* Convolutional temporal modeling (TCN)

---

# **Training Strategy**

## **Data Splits**

* Training: 70%
* Validation: 15%
* Testing: 15%

---

## **Sequence Windowing**

Videos are sliced into temporal windows.

* Window size: 90 frames
* Zero padding applied
* Randomized padding placement

This prevents the model from learning fixed repetition alignment.

---

## **Augmentation**

Gaussian noise is added during training to simulate:

* Camera instability
* Pose estimation noise
* Lighting variations

---

## **Optimization**

* Optimizer: AdamW
* LR Scheduler: ReduceLROnPlateau
* Early stopping enabled
* Gradient clipping enabled

---

# **Results**

## **Machine Learning**

### **Exercise Classification**

Random Forest and SVM achieved approximately **99% F1-score** for exercise classification.

### **Correctness Classification**

Traditional ML models achieved around **60-66% F1-score** for correctness classification.

The main limitation is that ML models analyze isolated frames only. Temporal mistakes cannot be reliably detected frame-by-frame.

---

## **Deep Learning**

### **LSTM Results**

* Exercise F1-score: ~1.00
* Correctness F1-score: ~0.94

### **TCN Results**

* Exercise F1-score: ~0.98
* Correctness F1-score: ~0.89

The LSTM consistently outperformed the TCN on correctness prediction.

---

# **Key Finding**

Exercise correctness is fundamentally a temporal problem.

A single frame often cannot determine:

* Range of motion
* Repetition completion
* Movement consistency

Sequence-based models significantly outperform frame-based classifiers for this task.

---

# **Real-Time Inference**

The project also supports live webcam inference.

## **Pipeline**

1. Webcam capture
2. MediaPipe pose extraction
3. Feature generation
4. Sequence buffering
5. Model inference
6. Real-time prediction display

---

# **Technologies Used**

## **AI / ML**

* Python
* PyTorch
* Scikit-learn
* NumPy
* Pandas

## **Computer Vision**

* OpenCV
* MediaPipe

## **Visualization**

* Matplotlib
* Seaborn

---

# **Repository Structure**

```bash
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
```

---

# **Limitations**

* Small dataset size
* Limited camera angles
* Limited exercise variety
* Limited incorrect form variations
* Overfitting risk in DL models
* Not yet generalized to all body types or environments

---

# **Future Improvements**

* Larger dataset
* More exercise categories
* More incorrect form variations
* Multi-angle support
* Mobile deployment
* Injury-risk estimation
* Personalized feedback generation
* Rep counting
* Real-time coaching suggestions

---

# **Authors**

* Pio Aouad
* Ghadi Khoury

---

# **Acknowledgements**

* MediaPipe Pose
* PyTorch
* Scikit-learn
* OpenCV


AI tools such as ChatGPT were occasionally used for grammar refinement, wording assistance, and organization support.
