<h2 align="center">Vũ Ngọc Sơn</h2>
<h3 align="center">AI Engineer · Computer Vision · NLP · Reinforcement Learning</h3>

<p align="center">
  Final-year Software Engineering student at PTIT, obsessed with building end-to-end AI systems — from training deep learning models to deploying them on embedded hardware. I specialize in Computer Vision, NLP, and Graph Neural Networks, and care deeply about models that work in the real world, not just on benchmarks. When I hit a wall, I may not always have an immediate answer — but I will find one.
</p>


## 🛠️ Tech Stack

**Languages:** Python (primary) · C++ 

**AI/ML Frameworks:** TensorFlow · PyTorch · Scikit-learn · HuggingFace · YOLOv5

**Computer Vision:** OpenCV · TFLite · CLAHE · INT8 Quantization · EasyOCR

**NLP:** PhoBERT · Sentence-Transformers · Rasa · DIETClassifier · TEDPolicy

**Graphs & RL:** PyTorch Geometric (GCN) · OpenAI Gym · DDQN · Prioritized Experience Replay

**Backend & Deployment:** FastAPI · Flask · Docker (basic) 

**Tools & Platforms:** Git · Linux · Google Colab · Kaggle · VS Code

---

## Featured Projects

---

### Traffic Violation Detection System

> **Core challenge:** Detect and log traffic violations in real-time video without human supervision.

A real-time traffic monitoring pipeline that automatically identifies violating vehicles and reads their license plates from video streams.

**Key Engineering Decisions:**
- **Custom YOLOv5 Fine-tuning** — Trained on a domain-specific vehicle dataset for accurate detection under varying lighting and occlusion conditions
- **EasyOCR License Plate Extraction** — Integrated OCR pipeline to reliably extract plate numbers from cropped bounding boxes
- **Rule-Based Violation Logic** — Deterministic classification engine that maps detected behavior to violation categories and writes structured logs automatically

**Stack:** Python · OpenCV · YOLOv5 · EasyOCR

---

### Hybrid Recommendation System

> **Core challenge:** Build a recommender that works with sparse interaction data and evolves without downtime.

**[GitHub](https://github.com/sonplusplus/RecommendSys/)**

A production-oriented recommendation engine combining semantic content understanding and collaborative filtering signals for a Vietnamese e-commerce platform.

**Key Engineering Decisions:**
- **3-Source Data Merging Pipeline** — Fuses base CSV, accumulated interaction logs, and live API signals with deduplication and weight aggregation across training runs, ensuring model freshness without cold-start collapse
- **Hybrid Scoring: PhoBERT (60%) + ALS (40%)** — PhoBERT generates content-aware item embeddings; ALS Collaborative Filtering captures implicit behavioral patterns via confidence-weighted matrix factorization
- **Adaptive Training Strategy Selector** — Dynamically chooses between full retrain, warm-start, or incremental update depending on data drift magnitude — zero-downtime async model hot-swap keeps the API always live
- **FastAPI Serving Layer** — Clean REST endpoints exposing recommendation results with low-latency inference

**Stack:** Python · FastAPI · PhoBERT · HuggingFace · ALS (Collaborative Filtering)

---

### IoT Network Intrusion Detection (GNN)

> **Core challenge:** Detect cyberattacks on IoT network flows using graph-structured representations.

**[GitHub](https://github.com/sangvirgo/IOT-traffic-anomaly-detect/)**

A Graph Convolutional Network applied to real-time network intrusion detection, framing traffic flows as a dynamic graph where topology encodes semantic similarity.

**Key Engineering Decisions:**
- **KNN Graph Construction** — Network flows represented as nodes; edges built via cosine similarity between feature vectors, capturing relational context that tabular models miss
- **GCN Architecture** — Message-passing layers aggregate neighborhood information per flow, enabling the model to detect coordinated multi-flow attack patterns
- **Active Learning Loop** — Iteratively selects the most informative unlabeled samples for annotation, dramatically reducing labeling cost while preserving detection quality
- **96.7% Recall on Attack Flows** — False negatives reduced to 105/3,174 attack flows — designed to minimize missed threats over false alarm rate in security-critical context

**Stack:** Python · PyTorch · PyTorch Geometric · Graph Neural Networks

---

### Drowsiness Detection for ESP32-CAM

> **Core challenge:** Run a reliable drowsiness detector on a microcontroller with <1MB of RAM.

**[GitHub](https://github.com/sonplusplus/Drowsiness-Detection-ESP32/)**

An embedded-first drowsiness detection system optimized end-to-end for the ESP32-CAM — from IR night-vision preprocessing to INT8-quantized on-device inference.

**Key Engineering Decisions:**
- **IR Night-Vision Simulation** — CLAHE + histogram stretch preprocessing pipeline replicates low-light camera conditions during training, closing the sim-to-real gap before deployment
- **Lightweight SE-CNN (27K params, 32×32 input)** — Squeeze-and-Excitation block provides channel-wise attention to boost accuracy with minimal parameter overhead — designed within strict microcontroller memory constraints
- **PERCLOS with 30-Frame Sliding Window** — Instead of noisy single-frame classification, drowsiness is calculated as the percentage of eye closure over 30 consecutive frames, dramatically reducing false alarms
- **INT8 Quantization & TFLite Export** — Post-training INT8 quantization shrinks model size for embedded deployment while achieving 98.4% recall on closed-eye class — the critical class in safety systems

**Stack:** Python · TensorFlow · TFLite · OpenCV · CLAHE

---

### 🕹️ Pacman DDQN — RL Agent from Pixels

> **Core challenge:** Teach an agent to play Pacman from raw pixels — no game state, no hand-crafted features.

**[GitHub](https://github.com/Reinforcement-Learning-Pacman/Double-DeepQ-Learning)**

A Double Deep Q-Network that learns optimal Pacman strategies end-to-end from visual observations, implementing advanced RL techniques to stabilize training.

**Key Engineering Decisions:**
- **Decoupled Q-Networks (DDQN)** — Separates action selection (Q-policy) from value estimation (Q-target), eliminating the overestimation bias that destabilizes vanilla DQN in long-horizon tasks
- **Prioritized Experience Replay (PER)** — Samples transitions proportional to their TD-error magnitude, focusing learning on surprising or poorly-understood experiences and accelerating convergence
- **3-Layer CNN Visual Encoder** — Processes raw game pixels directly; no manual feature engineering — learns spatial representations (walls, ghosts, pellets) entirely from reward signal

**Stack:** Python · TensorFlow · OpenAI Gym

---

### E-Commerce Platform with NLU Chatbot

> **Core challenge:** Build an intelligent Vietnamese-language shopping assistant that understands intent, extracts entities, and fetches real product data — without hardcoding every response.

**[Frontend Repo](https://github.com/sonplusplus/Front-Ecommerce)** · **[Chatbot Repo](https://github.com/sonplusplus/Chatbot-ecommerce)**

A full e-commerce frontend paired with a Rasa-powered NLU chatbot for Vietnamese customer support. The chatbot uses ML models to classify intent, extract entities, and manage multi-turn dialogue, then calls live product APIs to respond with real data.

**Key Engineering Decisions:**
- **DIETClassifier NLU Pipeline** — Dual Intent and Entity Transformer classifies user intent across 20+ intents (`ask_product_recommendation`, `compare_products`, `filter_products_by_price`...) and extracts structured entities (`product_name`, `brand`, `price_range`, `feature`) from raw Vietnamese text — learned from training data, not hardcoded patterns
- **TEDPolicy Dialogue Management** — Transformer Embedding Dialogue policy learns optimal action sequences from conversation stories, enabling flexible multi-turn flows beyond simple if/else routing
- **Real-Time API Integration** — Custom Rasa SDK actions call a live backend for product search, price lookup, stock availability, and order status — responses reflect actual inventory, not mock data
- **Slot Filling for Context Persistence** — User preferences (`product_type`, `brand`, `price_range`) persist across conversation turns, enabling coherent multi-step queries like "find Asus laptops under 15M → check if it's in stock"
- **Full E-Commerce Flow** — Frontend supports OAuth2 (Google, GitHub), OTP verification, VNPay payment, Vietnamese address APIs, and multi-step checkout

**Stack:** Python · Rasa 3.6 · Rasa SDK · ReactJS · Vite · Tailwind CSS · JWT · OAuth2

---

## 🎯 Current Focus

- **Seeking an AI Engineer / ML Engineer Internship** — Looking to apply academic knowledge to real-world products and contribute to production-grade AI systems

---

## 🌱 Currently Learning

- **Vision Transformers (ViT)** — Architecture deep-dives and applications in Object Detection & Image Segmentation
- **LLM Fine-tuning & RAG** — Retrieval-Augmented Generation pipelines for domain-specific AI applications

---

## Connect

- **Email**: vungocson189204@gmail.com
- **GitHub**: [github.com/sonplusplus](https://github.com/sonplusplus)
- **Linkedin** : [linkedin.com/in/vungocson189](https://www.linkedin.com/in/sonvungoc189/)
- Always open to collaborating on AI/ML projects or discussing research ideas

---

<p align="center">
  <i>Seeking AI Engineer internship opportunities — Computer Vision · NLP · Embedded AI · RL</i>
</p>
