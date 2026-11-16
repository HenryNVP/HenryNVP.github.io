# Henry Nguyen

📍 Burke, VA • 📞 (206) 751-6618

🔗 [LinkedIn](https://linkedin.com/in/henrynguyen-vp) • [GitHub](https://github.com/HenryNVP) • [Resume](./henry-nguyen-robotics-resume.pdf) • [Email](mailto:henrynguyen.vp@gmail.com) • [Website](https://henrynvp.github.io/)

---

## About Me

Robotics engineer at the hardware-software boundary: closed-loop control and SLAM on physical manipulators and mobile robots, a network-in-the-loop testbed running robot policies on Jetson, and SiL/HiL/End-of-Line release validation across 8+ ESP ECU variants at Bosch. M.S. in AI from San Jose State University.

---

## Featured Robotics & AI Projects

### 🦾 [EdgeVLA-HiL: Processor- and Network-in-the-Loop Testbed](https://github.com/HenryNVP/edge-vla-hil) *(Ongoing)*
* **Tech:** ROS 2, NVIDIA Jetson, MuJoCo, DDS, ONNX Runtime, Docker, GitHub Actions
* Built a network-in-the-loop testbed: an **ACT policy** (ONNX Runtime) on a **Jetson Orin Nano** drives a MuJoCo arm on x86 over **ROS 2/DDS**, with a seeded relay injecting delay, jitter and burst loss. A fail-safe handshake and DDS deadline watchdogs cut arm drift after a node crash from **189 mm to 8 mm**, backed by **470+ tests** in CI.
* Found and fixed silent timing bugs behind plausible but wrong metrics: multi-threaded simulator access corrupting the policy's frames, a sim running at 80% of real time, and unaligned observation/control clocks. Clock alignment alone took in-loop peg insertion from **53% to 83%**.

### 📡 Robot-Side Control Under Stale Cloud Plans (EdgeVLA research)
* **Tech:** PyTorch, LeRobot, robosuite
* Split cloud/robot policy for links slower than the action-chunk horizon: an on-robot MobileNet policy fuses a stale cloud bird's-eye-view map with fresh camera frames and replans every tick. It reached **91% success** on moving-target peg insertion under 1.5 s of delay, against 79% for flat-feature fusion.

### 🚗 [NVIDIA-Accelerated BEVFusion Perception Pipeline](https://github.com/HenryNVP/ros2-bevfusion-jetson)
* **Tech:** Jetson Orin, TensorRT, CUDA, ROS 2, Docker
* Deployed an optimized **BEVFusion** 3D multi-camera/LiDAR perception pipeline (NuScenes) as a containerized modular **ROS 2** package. TensorRT FP16/INT8 achieved **~7 FPS** on Jetson Orin Nano.

### 📱 [FastViT Mobile Optimization](https://github.com/HenryNVP/fastvit-pet-mobile)
* **Tech:** PyTorch, ONNX, Quantization
* Replaced Multi-Head Attention with **Performer Attention** (O(N)) plus FP16 quantization: **4.8x speedup** on mobile hardware with identical accuracy.

---

## AI Agent Projects

### 🤖 [AI Tutor: RAG-Powered Learning Platform](https://github.com/HenryNVP/ai-tutor)
* **Tech:** GenAI, RAG, MCP, FastAPI, OpenAI Agents SDK
* **Summary:** Built a full-stack **multi-agent** educational system that ingests documents to generate cited answers, adaptive quizzes, and lesson notes via a source-filtered **RAG** pipeline using **ChromaDB**. Implemented **MCP servers** and secure Python execution with **FastAPI** backend, enabling structured tool use, real-time data visualization from CSVs, and adaptive learning features that track student progress.

### 🎓 [SAM-E: Enrollment Assistant](https://github.com/HenryNVP/enrollment-assistance)
* **Tech:** LangGraph, RAG, pgvector, Neo4j, FastAPI, Docker, AWS
* **Summary:** Built a scalable microservices ML system for SJSU enrollment: **LangGraph** agentic orchestration over HTTP tool-calling to **pgvector** vector search, a **Neo4j** graph gateway, and web search—from document ingestion to real-time **FastAPI** serving. Answers enrollment and program questions by retrieving policy and curriculum documents, looking up curated prerequisites and program structure, and ingesting scraped SJSU source material. Containerized with **Docker Compose** and deployed on **AWS**.

---

## Professional Experience

**AI Tutor: Live Voice Tutoring Platform** | *2026 – Present*  
*Pre-launch startup project, two-person team · agentic tooling and simulation-driven animation*
* Ran parallel AI coding agents in isolated git worktrees, with written briefs, guardrails and a review gate before merge, over a multi-engine animation codebase.
* Closed the loop on agent output: automatic checks plus **OpenModelica** simulation reject invalid models before they drive the renderer (41 simulated device explainers). A blind evaluation harness (vision-model judge) rated them **35% above** the LLM-only baseline, winning 14 of 20 hard devices.

**Software Integration Engineer (Automotive Safety Systems)** | *Jun. 2023 – Dec. 2023*  
*Bosch Global Software Technologies, Ho Chi Minh, Vietnam*
* Owned complete-software integration testing and release for **8+ ESP variants** through **SiL, HiL** (wiring ECUs into ETAS LABCAR benches) and **End-of-Line** validation of CAN communication and signal integrity.
* Created new ESP (Electronic Stability Program) variants end to end (XML variant definition, C source, code generation, EEPROM configuration, build-system entries) and root-caused cross-variant HiL regression failures to ECU configuration deltas.

**Robotics Control Engineer (Student Researcher)** | *Aug. 2022 – Dec. 2022*  
*Mechatronics Lab, HCMUT, Ho Chi Minh, Vietnam*
* Developed closed-loop **C++** control for a 5-axis manipulator with vision-based feedback; a custom 3-finger gripper plus fuzzy logic controller delivered a **200% payload increase**.

**Robotics Systems Engineer (Student Researcher)** | *Mar. 2021 – Dec. 2021*  
*National Key Laboratory of Digital Control and System Engineering, Ho Chi Minh, Vietnam*
* Validated **SLAM** and autonomous navigation on mobile robots using ROS and multi-sensor integration, spanning mechanical design, electrical interfaces, and control algorithms.

---

## Education

**Master of Science in Artificial Intelligence** | *May 2026 · GPA: 3.6*  
San Jose State University, San Jose, CA

**Bachelor of Engineering in Mechatronic Engineering** | *Apr. 2023 · GPA: 3.5*  
Ho Chi Minh City University of Technology, Vietnam

---

## Technical Skills

* **Languages:** Python, C++, C, SQL, Bash, MATLAB/Simulink
* **Robotics:** ROS 2, MuJoCo/robosuite, DDS, Action Chunking, System Integration, Sensor Fusion, SLAM, PID Control, Kinematics, Motion Control
* **Test & Validation:** Hardware-in-the-Loop (HiL), Software-in-the-Loop (SiL), End-of-Line Testing, ETAS LABCAR, CAN Protocol, Fault Injection, Regression Testing
* **Hardware & Embedded:** Schematic Design, MCU Wiring, PCB Soldering, Oscilloscope, Motor Control Programming, HiL Bench Wiring
* **Machine Learning:** PyTorch, ONNX Runtime, TensorRT, CUDA, ACT/Diffusion Policies, Quantization (FP16/INT8), Edge AI (Jetson)
* **Software & Cloud:** Git, CI/CD, GitHub Actions, Docker, Linux, AWS, LangGraph, MCP, RAG (pgvector)
