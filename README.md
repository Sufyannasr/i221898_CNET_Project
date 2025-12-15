# Enhanced Streaming Intrusion Detection for WSN/IoT

## Overview
This project implements an **enhanced intrusion detection system (IDS)** for **Wireless Sensor Networks (WSN) and IoT environments**.  
It extends the base paper’s **K-Means Sampling (KMS) + PCA + Machine Learning** framework by addressing its key real-world limitations.

Unlike the base paper, this implementation supports **streaming data**, **concept drift detection**, **novel (zero-day) attack detection**, **federated learning**, and **privacy preservation**, making it suitable for deployment in real IoT systems.

---

## Base Paper Summary
The base paper proposes:
- K-Means Sampling (KMS) for class balancing  
- PCA for dimensionality reduction  
- Traditional ML classifiers for attack detection  

**Limitations of the base paper:**
- Offline training only  
- No support for streaming data  
- No concept drift detection  
- No novel / zero-day attack detection  
- No privacy or federated learning  
- Oversampling before train-test split (data leakage risk)

---

## What This Code Does

### 1. Streaming Intrusion Detection
- Processes data in **mini-batches** to simulate real-time IoT traffic
- Models are trained incrementally using `partial_fit`
- No need to retrain from scratch

### 2. Dual-Model Learning
The system uses two complementary learners:
- **Enhanced MLP (Neural Network)** – high accuracy
- **Enhanced SGD Classifier** – lightweight and fast
- Predictions are combined using an **ensemble**

### 3. Concept Drift Detection
- Continuously monitors classification error
- Detects changes in traffic behavior over time
- Uses a **confirmation window** to reduce false alarms
- Supports:
  - Drift warnings
  - Drift confirmation
  - Adaptive responses (learning rate change, model reset, rollback)

### 4. Novel (Zero-Day) Attack Detection
- Uses **EllipticEnvelope** for anomaly detection
- Detects unseen or unknown attack patterns
- Tracks:
  - Average novelty score per batch
  - Novelty rate (fraction of novel samples)
- Saves detected novel samples for analysis

### 5. Federated K-Means Sampling (KMS)
- Each node maintains a **local reservoir** of representative samples
- Periodic **federated aggregation** builds global knowledge
- Uses **robust aggregation (median)** instead of mean
- Prevents raw data sharing between nodes

### 6. Differential Privacy (ε = 1.0)
- Adds calibrated Laplace noise to:
  - Local samples
  - Federated updates
- Protects sensitive IoT data
- Balances privacy and accuracy

### 7. Continual Learning with Replay
- Maintains a bounded replay buffer
- Prevents catastrophic forgetting
- Balances replay samples across classes

### 8. Stability & Reproducibility Fixes
- Correct train-test split (no oversampling before split)
- Proper SGD initialization
- Gradient clipping for neural network stability
- Calibrated drift and novelty thresholds

---

## Key Improvements Over the Base Paper

| Aspect | Base Paper | This Implementation |
|------|-----------|--------------------|
| Training | Offline | Streaming / Online |
| Concept Drift | ❌ None | ✅ Adaptive Drift Detection |
| Novel Attacks | ❌ Not supported | ✅ Zero-day Detection |
| Privacy | ❌ None | ✅ Differential Privacy (ε=1) |
| Federation | ❌ Centralized | ✅ Federated KMS |
| Oversampling | Static | Dynamic, stream-aware |
| Continual Learning | ❌ None | ✅ Replay + Adaptation |
| Reproducibility | Weak | Fixed & reliable |

---

## System Architecture (High Level)

1. Incoming data stream  
2. Novelty detection (zero-day attacks)  
3. Drift detection (behavior change)  
4. Federated KMS balancing  
5. Incremental training (MLP + SGD)  
6. Ensemble prediction  
7. Performance monitoring & visualization  

---

## How to Run

### Requirements
- Python 3.8+
- NumPy, Pandas, Scikit-learn
- PyTorch
- Imbalanced-learn
- Matplotlib
