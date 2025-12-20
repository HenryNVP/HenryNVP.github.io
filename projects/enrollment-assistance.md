# SAM-E: Planning Assistant - Enrollment

**SAM-E** is an AI-powered enrollment assistance tool specialized for the San Jose State University (SJSU) academic domain. The system helps students with enrollment decisions, budgeting, scheduling, multi-term planning, degree audits, and scenario comparisons through an intelligent conversational interface.

## Project Scope

SAM-E provides comprehensive enrollment assistance for SJSU students, including:

- **Enrollment Questions**: Course availability, deadlines, prerequisites, capacity limits, time windows
- **Budgeting**: Tuition, fees, books, commuting, and housing cost calculations
- **Scheduling**: Course locations, times, professors, accessibility features, conflict detection
- **Multi-Term Planning**: Long-term academic planning across multiple semesters
- **Degree Audits**: Progress tracking by major, minor, and concentration
- **Transfer Assistance**: Course equivalency lookups and transfer policy information
- **Scenario Comparisons**: Full-time vs. part-time enrollment, in-person vs. hybrid delivery modes

The system is built as a microservices architecture with three core services, combining LangGraph-powered conversational AI with specialized enrollment domain logic and knowledge retrieval capabilities.

---

## 1. System Architecture

The solution follows a **microservices architecture** with three independent services:

### **Microservice 1: Agent Service** (`services/agent_ai`, Port 8000)
Main orchestration hub - FastAPI application with:
- LangGraph agent workflows (state machine)
- Authentication & session management (guest + registered users)
- Chat API (sync & streaming) for enrollment questions
- Document upload proxy
- Enrollment assistance tools orchestration:
  - `enrollment_search` - Course search with filters
  - `degree_audit` - Major/minor/concentration progress
  - `scenario_compare` - Enrollment scenario comparisons
  - `schedule_optimize` - Optimal schedule generation
  - `transfer_check` - Transfer course equivalency
  - `budget_calculate` - Cost calculations
  - `deadline_check` - Enrollment deadlines
- Prometheus metrics + Langfuse tracing

### **Microservice 2: RAG Service** (`services/rag_api`, Port 8080)
Document processing & knowledge retrieval - FastAPI application with:
- Multi-format document ingestion (15+ file types)
- Policy document processing (enrollment, transfer, financial aid)
- Text chunking & embedding generation (OpenAI)
- Vector similarity search (pgvector)
- Knowledge graph integration (Neo4j) for prerequisites and requirements
- Hybrid retrieval (vector + graph-enhanced RAG)
- Entity extraction (NER) & relationship mapping
- JWT-protected endpoints (`/embed`, `/query`, `/ids`)

### **Microservice 3: Enrollment Service** (`services/enrollment_api`, Port 8090)
Enrollment domain logic - FastAPI application with:
- **Degree Audit Engine**: Progress tracking, requirement matching, gap analysis
- **Scenario Comparison Engine**: Full-time vs. part-time, delivery mode comparisons
- **Schedule Optimization Engine**: Conflict detection, constraint satisfaction, optimal scheduling
- **Transfer Equivalency Engine**: Course equivalency lookups and policy validation
- **Budget Calculator**: Tuition, fees, books, housing, commuting cost calculations
- **Course Search API**: Filtered course and section queries

### **Data Storage**
- **Postgres + pgvector**: 
  - Application data (users, sessions, checkpoints)
  - Enrollment domain data (courses, sections, enrollments, majors, requirements)
  - Financial data (tuition, fees, costs, housing, commuting)
  - Vector embeddings (policy documents)
- **Neo4j**: Knowledge graph (prerequisites, requirements, transfer equivalencies, course relationships)

### **External Providers**
- **OpenAI**: LLM completions + embeddings
- **Observability**: Prometheus, Grafana, Langfuse

### Architecture Documentation

Comprehensive architecture diagrams are organized hierarchically in the `architecture/` folder:

**Quick Start Navigation:**
- `00_overview/A0_01_overview_simplified` - Start here for high-level understanding
- `02_service/A2_layer_service_microservices` - All three microservices overview
- `03_knowledge/A3_layer_knowledge_data_simple` - Data storage overview
- `04_flows/A4_00_flow_overview_simple` - Main workflow overviews

**Detailed Views:**
- `00_overview/` - System overviews (simplified & detailed)
- `01_client/` - Client layer components
- `02_service/` - Service layer with component details:
  - `agent_service/` - Agent Service components
  - `rag_service/` - RAG Service components
  - `enrollment_service/` - Enrollment Service components
- `03_knowledge/` - Knowledge/Data layer:
  - `neo4j/` - Knowledge graph structure
  - `database/` - Database schemas
- `04_flows/` - Runtime flow diagrams (ingestion, auth, chat, enrollment)

See `architecture/README.md` and `architecture/ARCHITECTURE_INDEX.md` for complete documentation and navigation guide.

**Regenerate diagrams:**
```bash
cd architecture
java -jar tools/plantuml.jar -tpng **/*.puml
```

---

## 2. Prerequisites

- Docker Engine and Docker Compose plugin (v2+ recommended)
- Python 3.10+ if you plan to run pieces outside containers
- `uv` package manager (optional) for local dependency work
- Valid API keys for any LLM or embedding providers you plan to use (e.g. Google Generative AI)

---

## 3. Environment Configuration

1. **Copy the provided examples**  
   - `services/agent_ai/.env.development` and `services/rag_api/.env` already contain template values. Adjust secrets before running in production.

2. **Align shared secrets**  
   - `RAG_JWT_SECRET` must match `JWT_SECRET` in the RAG service so the agent can authenticate its proxy calls.
   - Keep `COLLECTION_NAME`, `POSTGRES_*`, and API keys consistent between services.

3. **Tune chunking/embedding settings** (optional)  
   - `CHUNK_SIZE`, `CHUNK_OVERLAP`, and `PDF_EXTRACT_IMAGES` live in `services/rag_api/.env` and govern how documents are split.

---

## 4. Running the Stack

1. **Start all services**
   ```bash
   docker compose -f docker_compose.yml up --build
   ```
   This launches:
   - `db` – pgvector-backed Postgres database (exposed on `localhost:55432`)
   - `neo4j` – Knowledge graph database (exposed on `localhost:7474`)
   - `rag_api` – document ingestion and retrieval service (`http://localhost:8080`)
   - `enrollment_api` – enrollment domain service (`http://localhost:8090`)
   - `agent_api` – LangGraph agent API and testing UI (`http://localhost:8000`)

2. **Check health**
   - Agent health: `curl http://localhost:8000/health`
   - RAG docs: open `http://localhost:8080/docs`
   - Enrollment docs: open `http://localhost:8090/docs`

3. **Stop services**
   ```bash
   docker compose -f docker_compose.yml down
   ```

---

## 5. Loading Documents into the RAG Store

Use the agent’s document proxy or call the RAG API directly.

### Option A – Browser UI
1. Open `http://localhost:8000/ui`.
2. Register or use anonymous session creation.
3. In the “Knowledge Base” card, choose a `file_id` (e.g. `strategic_docs`) and upload PDFs or text files.
4. Hit “Refresh IDs” to confirm the file was indexed.

### Option B – Scripted Upload
```python
import os
import pathlib

import jwt
import requests

RAG_BASE = "http://localhost:8080"
JWT_SECRET = os.environ["RAG_JWT_SECRET"]
FILE_ID = "sjsu_enrollment_policies_2024"
FILE_PATH = pathlib.Path("data/sjsu/policies/enrollment_guide.pdf")

token = jwt.encode({"sub": "agent_service"}, JWT_SECRET, algorithm="HS256")
with FILE_PATH.open("rb") as fh:
    resp = requests.post(
        f"{RAG_BASE}/embed",
        data={"file_id": FILE_ID},
        files={"file": (FILE_PATH.name, fh, "application/pdf")},
        headers={"Authorization": f"Bearer {token}"},
        timeout=180,
    )
resp.raise_for_status()
print(resp.json())
```

### Verification
- `GET http://localhost:8080/ids` – list all stored file identifiers
- `GET http://localhost:8080/documents/{file_id}/context` – inspect chunk summaries
- Update `RAG_DEFAULT_FILE_IDS` in `services/agent_ai/.env.development` to make the agent query those documents by default.

---

## 6. Using SAM-E for Enrollment Assistance

### Chat Interface

1. Open `http://localhost:8000/ui` or call the API directly.
2. Create a session with `/api/v1/auth/session` (authentication is optional; a guest user is created automatically).
3. Send conversation payloads to `/api/v1/chatbot/chat` or stream responses from `/api/v1/chatbot/chat/stream`.
4. Use `/api/v1/chatbot/messages` to retrieve or clear the chat history stored in LangGraph checkpoints.

### Example Queries

SAM-E can handle various enrollment-related questions:

- **Course Search**: "What CS courses are available this fall?"
- **Degree Audit**: "How am I doing with my CS major requirements?"
- **Budget Planning**: "How much would full-time enrollment cost this semester?"
- **Schedule Help**: "Find me a schedule for CS 146, MATH 42, and ENGL 1B with no time conflicts"
- **Transfer Questions**: "Does MATH 30 at De Anza transfer to SJSU?"
- **Scenario Comparison**: "Compare full-time vs part-time enrollment for Fall 2025"
- **Deadline Questions**: "What are the enrollment deadlines for Spring 2025?"
- **Policy Questions**: "What's the policy on dropping classes after the deadline?"

---

## 7. Monitoring & Observability

- Prometheus metrics are exposed by the agent service; Grafana dashboards ship in `services/agent_ai/grafana`.
- Langfuse integration captures traces when `LANGFUSE_*` keys are configured.
- Structured logs follow the Structlog configuration in `services/agent_ai/app/core/logging.py`.

---

## 8. Repository Structure (Highlights)

```
architecture/                    Architecture diagrams (hierarchically organized)
  ├── README.md                  Diagram navigation guide
  ├── ARCHITECTURE_INDEX.md       Complete diagram index
  ├── ARCHITECTURE_REVIEW.md     Architecture review and recommendations
  ├── 00_overview/               System overviews (simplified & detailed)
  ├── 01_client/                 Client layer diagrams
  ├── 02_service/                Service layer (microservices)
  │   ├── agent_service/          Agent Service components
  │   ├── rag_service/            RAG Service components
  │   └── enrollment_service/     Enrollment Service components
  ├── 03_knowledge/               Knowledge/Data layer
  │   ├── neo4j/                  Neo4j knowledge graph
  │   └── database/               Database schemas
  ├── 04_flows/                   Runtime flow diagrams
  └── docs/                       Architecture documentation
data/                            Example documents to ingest
services/
  ├── agent_ai/                   Microservice 1: Agent orchestration
  │   ├── app/                    Source code (APIs, LangGraph, tools)
  │   ├── Dockerfile              Container definition
  │   └── README.md               Service documentation
  ├── rag_api/                    Microservice 2: RAG + Knowledge Graph
  │   ├── app/                    FastAPI routes, vector/graph stores
  │   ├── Dockerfile              Container definition
  │   └── README.md               Service documentation
  └── enrollment_api/             Microservice 3: Enrollment domain logic
      ├── app/                    Enrollment engines, APIs, repositories
      ├── Dockerfile              Container definition
      └── README.md               Service documentation
docker_compose.yml                Orchestrates all services
DESIGN_SPECS_SUMMARY.md           Service layer design specifications
```

---

## 9. Key Capabilities

### Enrollment Assistance Features

- ✅ **Course Search**: Filter by department, term, time, location, professor, accessibility
- ✅ **Degree Audits**: Track progress toward major, minor, and concentration requirements
- ✅ **Budget Planning**: Calculate tuition, fees, books, housing, and commuting costs
- ✅ **Schedule Optimization**: Find optimal schedules with conflict detection
- ✅ **Transfer Equivalency**: Look up transfer course equivalencies and policies
- ✅ **Scenario Comparison**: Compare enrollment scenarios (full-time vs. part-time, delivery modes)
- ✅ **Deadline Management**: Access enrollment deadlines and important dates
- ✅ **Policy Information**: Answer questions about enrollment, transfer, and financial aid policies

### Technical Features

- ✅ **Conversational AI**: Natural language interface powered by LangGraph and OpenAI
- ✅ **Hybrid RAG**: Vector search + knowledge graph for comprehensive retrieval
- ✅ **Real-Time Data**: Access to course availability, capacity, and schedules
- ✅ **Multi-Term Planning**: Long-term academic planning across semesters

## 10. Next Steps

- **Data Integration**: Set up ETL pipelines for SJSU SIS data (courses, enrollments, schedules)
- **Knowledge Graph Population**: Load prerequisites, requirements, and transfer equivalencies into Neo4j
- **Policy Document Ingestion**: Process enrollment, transfer, and financial aid policy documents
- **Testing & Validation**: Validate degree audit accuracy against official SJSU system
- **Performance Optimization**: Load testing, caching strategy, and query optimization

For detailed architecture information, see:
- `architecture/ARCHITECTURE_INDEX.md` - Complete diagram navigation
- `architecture/docs/ENROLLMENT_ASSISTANCE_ARCHITECTURE.md` - Full architecture specification
- `architecture/docs/FEASIBILITY_ASSESSMENT.md` - Implementation timeline and scope
- `architecture/ARCHITECTURE_REVIEW.md` - Architecture review and recommendations

---

**SAM-E** - Strategic Planning Assistant for Enrollment at SJSU
