# Henry Nguyen

📍 San Jose, CA • 📞 (206) 751-6618

🔗 [LinkedIn](https://linkedin.com/in/henrynguyen-vp) • [GitHub](https://github.com/HenryNVP) • [Resume](./cv-henry-nguyen-capria-ventures.pdf) • [Email](mailto:henrynguyen.vp@gmail.com) • [Website](https://henrynvp.github.io/)

---

## About Me

I hold an M.S. in AI from San Jose State University, specializing in **Autonomous Systems**, **Agentic AI**, and **Edge Deployment**. I bridge the gap between high-level reasoning (LLMs) and low-level hardware optimization (TensorRT/CUDA).

---

## AI Agent Projects

### 🤖 [AI Tutor: RAG-Powered Learning Platform](https://github.com/HenryNVP/ai-tutor)
* **Tech:** GenAI, RAG, MCP, FastAPI, OpenAI Agents SDK
* **Summary:** Built a full-stack **multi-agent** educational system that ingests documents to generate cited answers, adaptive quizzes, and lesson notes via a source-filtered **RAG** pipeline using **ChromaDB**. Implemented **MCP servers** and secure Python execution with **FastAPI** backend, enabling structured tool use, real-time data visualization from CSVs, and adaptive learning features that track student progress.

### 🎓 [SAM-E: Enrollment Assistant](https://github.com/HenryNVP/enrollment-assistance)
* **Tech:** LangGraph, RAG, pgvector, Neo4j, FastAPI, Docker, AWS
* **Summary:** Built a scalable microservices ML system for SJSU enrollment: **LangGraph** agentic orchestration over HTTP tool-calling to **pgvector** vector search, a **Neo4j** graph gateway, and web search—from document ingestion to real-time **FastAPI** serving. Answers enrollment and program questions by retrieving policy and curriculum documents, looking up curated prerequisites and program structure, and ingesting scraped SJSU source material. Containerized with **Docker Compose** and deployed on **AWS**.

---

## Autonomous Systems & Edge AI

### 🚗 [ROS2 BEV-Fusion: Real-Time 3D Perception](https://github.com/HenryNVP/ros2-bevfusion-jetson)
* **Tech:** ROS2, TensorRT, CUDA, Jetson, Edge AI
* **Summary:** Developed an optimized **BEVFusion** 3D perception pipeline for multi-camera and LiDAR fusion, validated on NuScenes and deployed as a modular **ROS2** package. Optimized end-to-end inference with **TensorRT** and quantization, achieving **~7 FPS** for the full BEVFusion pipeline on Jetson Orin Nano and publishing **ROS2** detection outputs with latency metrics.

### 📱 [FastViT Mobile Optimization](https://github.com/HenryNVP/fastvit-pet-mobile)
* **Tech:** PyTorch, ONNX Runtime, Quantization, Knowledge Distillation
* **Summary:** Re-architected FastViT by replacing Multi-Head Attention with **Performer Attention** (**O(N)**) and implementing **FP16 quantization**, achieving a **4.8x speedup** on Android with **identical Top-1 accuracy**.

---

## Additional Projects

* **Image Classification:** Engineered a **timm** pipeline with automated **ONNX** export for rapid CNN/ViT benchmarking.
* **Anime RecSys:** Trained and deployed **NeuMF** and **Two-Tower** recommender system via **FastAPI**.
* **Client Web Projects:** Delivered commercial **WordPress** sites with automated booking, increasing client inquiries.

---

## Professional Experience

**Software Integration Engineer (Automotive Safety Systems)** | *Jun. 2023 -- Dec. 2023*  
*Bosch Global Software Technologies, Ho Chi Minh, Vietnam*
* Managed the software release lifecycle for **8+ ESP (Electronic Stability Program)** projects, integrating modules from cross-functional teams to deliver production-ready baselines.
* Executed comprehensive integration testing (**SiL & HiL**) and authored **ISO-compliant validation reports**, identifying critical defects to ensure system stability before delivery.

**Undergraduate Researcher** | *Aug. 2022 -- Dec. 2022*  
*Mechatronics Lab, HCMUT, Ho Chi Minh, Vietnam*
* Designed and fabricated a custom 3-finger gripper (+200% payload) and **retrofitted** a 5-axis manipulator, engineering a **vision-guided control stack** (C++, YOLO) for automated pick-and-place.

---

## Education

**Master of Science in Artificial Intelligence** | *May 2026*  
San Jose State University, San Jose, CA  
*Coursework: AI & Data Engineering, Deep Learning, Reinforcement Learning, Autonomous Driving, MLOps*

**Bachelor of Engineering in Mechatronic Engineering** | *Apr. 2023*  
Ho Chi Minh City University of Technology, Vietnam

---

## Technical Skills

* **Languages:** Python, C++, C, SQL, Bash, MATLAB
* **AI & Agents:** LLMs, LangGraph, RAG, MCP, Vector/Graph DBs, Agentic Workflows
* **Machine Learning:** PyTorch, TensorRT, ONNX, CUDA, Quantization, Transformers, CNNs
* **Robotics:** ROS2, System Integration, Sensor Fusion, Navigation, Control Systems
* **Software & Tools:** Git, Docker, CI/CD, Linux, FastAPI, Microservices, Edge AI (Jetson)
