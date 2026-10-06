# KalmanNet: Data-Driven Kalman Filtering

This project implements **KalmanNet**, a hybrid model-based and data-driven Kalman filtering approach proposed in the research paper **"KalmanNet: Data-Driven Kalman Filtering"** by Guy Revach, Nir Shlezinger, Ruud J. G. van Sloun, and Yonina C. Eldar.

The implementation adapts the KalmanNet concept to a **real UAV trajectory dataset from the EuRoC MAV Dataset** and compares its state-estimation performance with a classical Kalman Filter.

## 📌 Project Overview

The classical Kalman Filter is an optimal state-estimation algorithm for linear Gaussian state-space models. However, its performance depends on having accurate knowledge of the underlying system and noise statistics.

KalmanNet addresses this limitation by combining:

* Classical Kalman Filter structure
* A compact neural network
* GRU-based temporal memory
* Data-driven Kalman gain estimation

Instead of directly learning the complete state-estimation task, KalmanNet learns the **Kalman gain** and incorporates the learned gain into the traditional Kalman filtering process.

The original paper describes this as a hybrid data-driven/model-based filter that can improve robustness when the system model is inaccurate.

## 📄 Research Paper

**Title:** KalmanNet: Data-Driven Kalman Filtering

**Authors:**

* Guy Revach
* Nir Shlezinger
* Ruud J. G. van Sloun
* Yonina C. Eldar

**Conference:** ICASSP 2021

The implementation in this repository is an educational adaptation of the method described in the paper.

## 🎯 Objectives

The main objectives of this project are:

1. Understand the classical Kalman Filter.
2. Implement the main KalmanNet architecture.
3. Use a real UAV trajectory dataset.
4. Train a neural network to estimate the Kalman gain.
5. Compare KalmanNet with a classical Kalman Filter.
6. Investigate the effect of model uncertainty on state estimation.

## 🧠 KalmanNet Architecture

KalmanNet maintains the main prediction and state-update flow of a Kalman Filter.

The Kalman prediction is performed using:

```text
x̂(t|t-1) = F x̂(t-1)
```

The predicted observation is:

```text
ŷ(t|t-1) = H x̂(t|t-1)
```

The innovation is:

```text
Δy(t) = y(t) - ŷ(t|t-1)
```

Instead of calculating the Kalman gain analytically, KalmanNet uses a neural network:

```text
Previous State Estimate
        +
Current Observation
        ↓
Fully Connected Layer
        ↓
GRU
        ↓
Fully Connected Layer
        ↓
Learned Kalman Gain
        ↓
Kalman Update
        ↓
Updated State Estimate
```

The GRU provides temporal memory, allowing the network to learn information related to the unknown second-order statistics of the system.

## 🚁 Dataset

This project uses the **EuRoC MAV Dataset**, a real-world Micro Aerial Vehicle (MAV) dataset.

The implementation uses the:

```text
MH_01_easy
```

sequence from the Machine Hall environment.

The dataset provides UAV trajectory information that can be used as ground-truth information for evaluating state estimation.

### State Representation

The state used in this implementation is:

```text
[x, y, vx, vy]
```

where:

* `x` = position along x-axis
* `y` = position along y-axis
* `vx` = velocity along x-axis
* `vy` = velocity along y-axis

The observation consists of noisy position measurements:

```text
[x, y]
```

This allows the Kalman Filter and KalmanNet to estimate the complete state from partial and noisy observations.

## ⚙️ Technologies Used

* Python
* PyTorch
* NumPy
* Pandas
* Matplotlib
* SciPy
* Jupyter Notebook
* Kaggle Notebook
* EuRoC MAV Dataset

## 📂 Project Structure

```text
KalmanNet-Data-Driven-Filtering/
│
├── KalmanNet_EuRoC_MH01_Kaggle.ipynb
├── README.md
├── requirements.txt
├── .gitignore
│
└── results/
    ├── training_loss.png
    ├── trajectory_comparison.png
    └── error_comparison.png
```

## 🔬 Methodology

The implementation consists of the following stages.

### 1. Load UAV Dataset

The EuRoC MAV trajectory data is loaded and processed to obtain position and velocity information.

### 2. Construct State and Observation

The state vector is constructed as:

```text
x = [x, y, vx, vy]
```

while the observation contains:

```text
y = [x, y]
```

Noise is introduced into the observations to simulate the noisy measurements encountered in practical state-estimation problems.

### 3. Classical Kalman Filter

A classical Kalman Filter is implemented using the state transition matrix `F` and observation matrix `H`.

The standard Kalman filtering process consists of:

```text
Prediction
    ↓
Innovation Calculation
    ↓
Kalman Gain
    ↓
State Update
```

### 4. KalmanNet

KalmanNet follows the same model-based prediction and update structure.

The main difference is that the Kalman gain is learned using a neural network consisting of:

```text
Fully Connected Layer → GRU → Fully Connected Layer
```

The network is trained using the true state as the target.

### 5. Training

The network is trained using Mean Squared Error (MSE):

```text
MSE = mean((x_true - x_estimated)²)
```

The Adam optimizer is used for neural-network training.

### 6. Evaluation

The trained KalmanNet is evaluated against the classical Kalman Filter.

The comparison includes:

* State estimation error
* Position estimation
* Trajectory reconstruction
* MSE
* Performance under model uncertainty

## 📊 Expected Results

The main purpose of the experiment is to investigate whether the data-driven Kalman gain can provide improved state estimation when the assumed system model is inaccurate.

The original KalmanNet paper reports that KalmanNet can achieve performance close to the optimal Kalman Filter when the model is accurate and can provide improved robustness compared with the classical Kalman Filter when the model parameters are inaccurate.

This project applies that idea to a real UAV trajectory.

## 📈 Visualizations

The notebook generates visualizations such as:

### UAV Trajectory

Comparison between:

* Ground-truth trajectory
* Kalman Filter estimate
* KalmanNet estimate

### Training Loss

The training and validation loss of the KalmanNet neural network.

### Estimation Error

Comparison of the estimation errors produced by:

```text
Classical Kalman Filter
vs.
KalmanNet
```

### Model Mismatch

The implementation can also evaluate the filters using inaccurate system parameters to investigate robustness.

## 🚀 How to Run

### Option 1 — Kaggle

1. Open Kaggle.
2. Create a new notebook.
3. Upload:

```text
KalmanNet_EuRoC_MH01_Kaggle.ipynb
```

4. Enable Internet access if the notebook downloads the dataset.
5. Run all cells.

### Option 2 — Local Jupyter Notebook

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/KalmanNet-Data-Driven-Filtering.git
```

Move into the project directory:

```bash
cd KalmanNet-Data-Driven-Filtering
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter notebook
```

Open:

```text
KalmanNet_EuRoC_MH01_Kaggle.ipynb
```

and run the cells sequentially.

## 📦 Requirements

The main Python libraries required are:

```text
numpy
pandas
matplotlib
scipy
torch
jupyter
```

Install them using:

```bash
pip install -r requirements.txt
```

## 🔎 Difference Between KF and KalmanNet

| Feature                   | Kalman Filter | KalmanNet          |
| ------------------------- | ------------- | ------------------ |
| Model-based               | Yes           | Yes                |
| Data-driven               | No            | Yes                |
| Kalman gain               | Analytical    | Learned            |
| GRU                       | No            | Yes                |
| Requires noise statistics | Yes           | Reduced dependency |
| Handles model uncertainty | Limited       | More robust        |
| State estimation          | Yes           | Yes                |

## 💡 Key Learning

The main concept demonstrated by this project is that deep learning does not necessarily have to replace a traditional algorithm completely.

KalmanNet combines the strengths of both approaches:

```text
Domain Knowledge
      +
Deep Learning
      ↓
Hybrid State Estimator
```

The known system dynamics are retained, while the neural network learns the part of the filtering process that depends on unknown or inaccurate statistical information.

## 🔮 Future Improvements

Possible extensions include:

* Using additional EuRoC sequences.
* Using the full 3D UAV state.
* Incorporating IMU measurements.
* Using orientation information.
* Testing on the UZH-FPV dataset.
* Extending the model to nonlinear dynamics.
* Comparing with Extended Kalman Filter (EKF).
* Comparing with Unscented Kalman Filter (UKF).
* Hyperparameter tuning of the GRU.
* Testing under different levels of measurement noise.
* Evaluating additional metrics such as RMSE and MAE.

## 📚 Reference

Revach, G., Shlezinger, N., van Sloun, R. J. G., & Eldar, Y. C.

**"KalmanNet: Data-Driven Kalman Filtering."**

IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), 2021.

The paper introduces KalmanNet as a hybrid data-driven/model-based implementation of Kalman filtering in which a dedicated neural network learns the Kalman gain.

## ⚠️ Disclaimer

This repository is intended for **academic and educational purposes**. It is an implementation and adaptation of the KalmanNet research concept for experimentation with UAV state estimation.

It should not be considered a production-grade navigation or flight-control system.

## 👤 Author

**Sagnik Das**

B.Tech — Computer Science and Engineering

---

⭐ If you find this project useful, consider starring the repository.
