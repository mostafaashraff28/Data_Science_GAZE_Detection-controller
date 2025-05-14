# Data_Science_GAZE_Detection-controller
Absolutely! Here's a professional, well-structured documentation for your MobileNetV2 gaze classification model and training process — ideal for reports, presentations, or GitHub README.
________________________________________
📄 Gaze Direction Classification using MobileNetV2
📌 Project Overview
This project implements a gaze direction classification system as part of a gaze-controlled robotic car. The model takes in cropped right-eye images and classifies them into one of four gaze directions:
•	Left
•	Right
•	Forward
•	Close (eyes closed → stop command)
The trained model is intended for real-time deployment on a Raspberry Pi 4 to drive a robot based on eye movements, enhancing accessibility and assistive control.
________________________________________
🧠 Model: MobileNetV2 (Transfer Learning)
📦 Architecture
•	Base model: MobileNetV2
•	Input shape: 224x224x3
•	Top layers added:
o	GlobalAveragePooling2D
o	Dropout (0.3)
o	Dense(4, activation='softmax') for 4-class classification
🔍 Rationale for MobileNetV2
•	Lightweight and optimized for mobile/edge devices
•	Strong feature extraction with low latency
•	Easily converted to TensorFlow Lite for Raspberry Pi deployment
________________________________________
🔧 Dataset
•	Collected using: MediaPipe FaceMesh (right-eye crop)
•	Classes: left_eye, right_eye, forward_eye, close_eye
•	Subjects: 20 people × ~50 images per class = ~4000+ images
•	Image size: 224×224
•	Split method: Person-wise 4-fold cross-validation (14 train / 3 val / 3 test per fold)
________________________________________
🏗️ Training Strategy
✅ Preprocessing
•	Images rescaled to [0, 1]
•	Augmentations applied only to training:
o	rotation_range = 15°
o	zoom_range = 0.2
o	brightness_range = [0.6, 1.4]
o	width_shift / height_shift = 0.1
✅ Training Phases
Phase 1: Classification Head
•	Base MobileNetV2 frozen
•	Optimizer: Adam (1e-4)
•	Loss: Categorical Crossentropy + Label Smoothing (0.1)
•	Epochs: 50
•	Scheduler: Cosine decay learning rate
•	EarlyStopping on val_loss
Phase 2: Fine-Tuning
•	Unfreeze top 30 layers of MobileNetV2
•	Learning Rate: 2e-5
•	Optimizer: Adam
•	Additional 50 epochs
•	Same callbacks
________________________________________
📊 Performance Summary
Fold	Val Acc	Test Acc	F1 Score
1	0.9059	0.8425	0.8427
2 ✅	0.8286	0.9077	0.9052
3	0.8398	0.7866	0.7447
4	0.9493	0.8387	0.8386
•	Average Test Accuracy: 84.39% ± 4.3%
•	Best Model: model_fold_2_fine_tuned.keras
________________________________________
📤 Deployment
•	Converted to .tflite using TensorFlow Lite converter
•	Optimized with:
o	float32 and float16 quantization
•	Real-time inference on Raspberry Pi 4 with GPIO control for robot steering
________________________________________
📁 Output Artifacts
•	model_fold_X_phase1.keras
•	model_fold_X_fine_tuned.keras
•	classification_report_fold_X.txt
•	confusion_matrix_fold_X.png
•	tflite_models/model_fold_X_fine_tuned.tflite
________________________________________
Would you like me to generate this as:
•	A Markdown file?
•	A PDF report?
•	Or embed it into your GitHub README?
Just say the format — and I’ll deliver it for you.

