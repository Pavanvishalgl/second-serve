# 🍽️ Second Serve

### **Good food. Another chance.**

> An AI-powered surplus food coordination platform that helps community kitchens and nonprofits turn excess food into real impact.

<p align="center">

**Built with Strands Agents • React • FastAPI • Ollama • SQLite • Docker**

</p>

---

## 🌍 The Problem

Restaurants and food providers often have surplus food while community organizations nearby need meals.

The problem isn't always availability.

### **It's coordination.**

A coordinator needs to answer:

* Who can receive the food?
* How much can they take?
* Does it match their dietary requirements?
* Are there allergen restrictions?
* Can it be collected before the deadline?
* Which partner should be prioritized?

Second Serve brings these decisions together into **one intelligent workflow**.

---

# 🤖 What is Second Serve?

**Second Serve** is a surplus-food coordination platform designed for:

* 🏠 Community kitchens
* 🤝 Food-rescue volunteers
* ❤️ Neighbourhood nonprofits
* 🍱 Food donors and providers

A **Strands AI Agent** analyzes available surplus food and recipient needs, then creates a feasible rescue plan for the coordinator to review.

The system combines AI reasoning with deterministic constraints and human approval.

> ### **AI proposes. Deterministic logic validates. Humans approve.**

---

# 🔄 How It Works

```text
        🍱 SURPLUS FOOD
              │
              ▼
        🤖 STRANDS AGENT
              │
       ┌──────┴──────┐
       ▼             ▼
 🔎 Inspect Food   🔎 Inspect Needs
       │             │
       └──────┬──────┘
              ▼
        📋 RESCUE PLAN
              │
              ▼
       👤 HUMAN REVIEW
              │
              ▼
       ✅ APPROVE PLAN
              │
              ▼
      📦 RESERVE INVENTORY
              │
              ▼
          🚚 PICKUP
              │
              ▼
        🤝 HANDOFF
              │
              ▼
        📊 IMPACT TRACKING
```

---

# ✨ Key Features

| Feature                             | Description                                               |
| ----------------------------------- | --------------------------------------------------------- |
| 🤖 **AI Rescue Planning**           | Uses a real local LLM through Strands Agents              |
| 🧠 **Natural-Language Preferences** | Coordinators can express partner preferences naturally    |
| 📍 **Location-Aware Matching**      | Uses geographic coordinates to identify feasible partners |
| 🥗 **Dietary Constraints**          | Considers dietary requirements and allergen exclusions    |
| ⏰ **Deadline Awareness**            | Considers pickup deadlines                                |
| 📦 **Capacity-Aware Allocation**    | Prevents allocations from exceeding recipient capacity    |
| 👤 **Human Approval**               | AI plans require coordinator approval                     |
| 🔒 **Atomic Reservation**           | Inventory is revalidated and reserved during approval     |
| 🚚 **Pickup Management**            | Approved allocations become pickup missions               |
| ✅ **Handoff Tracking**              | Coordinators can confirm completed handoffs               |
| 📊 **Impact Dashboard**             | Tracks saved records and impact                           |
| 📄 **Exports**                      | CSV and JSON manifest exports                             |
| 👥 **Private Workspaces**           | Each account has its own coordinator workspace            |

The documented workflow covers food listing, partner management, AI planning, approval, pickup coordination and handoff confirmation.

---

# 🧠 Why an AI Agent?

Second Serve isn't simply a chatbot.

The Strands agent can:

1. Inspect available surplus food.
2. Inspect recipient needs and capacity.
3. Select the appropriate tools.
4. Interpret coordinator preferences.
5. Create a feasible rescue plan.

### Three bounded agent tools

```text
inspect_surplus
       ↓
inspect_recipient_needs
       ↓
create_rescue_plan
```

The AI **does not have unrestricted operational access**.

It cannot:

* ❌ Reserve food by itself
* ❌ Approve its own plan
* ❌ Mark a delivery complete
* ❌ Execute arbitrary SQL
* ❌ Access the shell
* ❌ Dispatch drivers
* ❌ Send external messages
* ❌ Access arbitrary files

Critical operations remain under deterministic application logic and human control.

---

# 💬 Natural-Language Planning

Coordinators don't need to specify every decision manually.

For example:

> **"Prioritize Night Shelter for dinner tonight, then send the rest to other partners."**

The agent can identify the named partner and incorporate that preference into its rescue plan.

However, preferences **never override hard constraints**.

The planner still respects:

* 📦 Recipient capacity
* 🥗 Dietary requirements
* ⚠️ Allergen exclusions
* ⏰ Pickup deadlines
* 📍 Maximum planning distance

This gives the coordinator the flexibility of natural language while keeping critical decisions constrained.

---

# 🏗️ Architecture

![Second Serve Architecture](docs/architecture.svg)

## Technology Stack

| Layer         | Technology              |
| ------------- | ----------------------- |
| 🎨 Frontend   | React + TypeScript      |
| ⚙️ Backend    | FastAPI + Python        |
| 🤖 Agent      | Strands Agents SDK      |
| 🧠 Local LLM  | Qwen3 4B Instruct       |
| ⚡ Inference   | Ollama                  |
| 🗄️ Database  | SQLite                  |
| 🗺️ Mapping   | Leaflet + OpenStreetMap |
| 📍 Geocoding  | Open-Meteo              |
| 🐳 Deployment | Docker Compose          |

---

# 🧩 AI Architecture

The default configuration uses a **real local Qwen3 4B Instruct model** running through Ollama.

```text
                 ┌───────────────────┐
                 │    React UI       │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │     FastAPI       │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │  Strands Agent    │
                 └─────────┬─────────┘
                           │
                    Tool Selection
                           │
            ┌──────────────┼──────────────┐
            ▼              ▼              ▼
     inspect_surplus   inspect_needs   create_plan
            │              │              │
            └──────────────┼──────────────┘
                           ▼
                 ┌───────────────────┐
                 │ Deterministic     │
                 │ Planner / Rules   │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ SQLite Workspace  │
                 └───────────────────┘
```

The LLM performs reasoning and tool selection, while deterministic logic performs constraint checking and allocation.

---

# 🛡️ Human-in-the-Loop Design

One of the core design principles is:

### **The AI recommends. The human decides.**

The rescue agent generates a proposal.

The coordinator reviews it.

Only after approval does the backend revalidate:

* Food quantities
* Recipient capacity
* Dietary constraints
* Deadlines
* Planning distance

Then the system atomically reserves the inventory.

This prevents an AI-generated plan from directly making irreversible operational decisions.

---

# 📍 Location & Mapping

Second Serve uses:

* **Open-Meteo** for city/postal-code search
* **Leaflet** for the interactive map
* **OpenStreetMap** for map tiles

The planner uses saved coordinates for location-aware matching.

Distance and travel estimates help determine feasibility, but they are **not turn-by-turn navigation**.

---

# 🔐 Security & Account Isolation

Second Serve provides private coordinator workspaces.

Security mechanisms include:

* 🔑 Passwords salted with **scrypt**
* 🔒 Hashed session tokens
* ⏳ Seven-day session expiration
* 🚪 Session revocation on logout
* 🍪 HttpOnly cookies
* 🛡️ SameSite cookie protection
* 🌐 Request-origin validation
* 🚦 Authentication rate limiting
* 🗄️ Separate SQLite domain data per account

The default local deployment keeps operational data and LLM inference on the local machine.

---

# 🧠 AI Modes

| Mode      | Model                     | Credentials     | Purpose                        |
| --------- | ------------------------- | --------------- | ------------------------------ |
| `ollama`  | Qwen3 4B Instruct         | None            | **Default local AI agent**     |
| `bedrock` | Amazon Bedrock model      | AWS credentials | Optional cloud inference       |
| `demo`    | Scripted Strands provider | None            | Testing and workflow rehearsal |

The default `ollama` mode runs the model locally.

---

# 🚀 Run Locally

## Prerequisites

You need:

* Docker Desktop with Linux containers
* Docker Compose v2.20+
* At least **8 GB RAM** available to Docker
* Approximately **8 GB free disk space**
* Internet access for the initial model/package download

## Start

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/second-serve.git
cd second-serve
```

Start the application:

```bash
docker compose up --build
```

The first startup downloads the local Qwen3 model, so the initial setup may take several minutes.

### Open the application

**Frontend:**

```text
http://localhost:8080
```

**API Documentation:**

```text
http://localhost:8000/docs
```

## Stop

To stop the containers without removing persistent data:

```bash
docker compose down
```

---

# 🧪 Try the Demo Workflow

For the fastest demonstration:

### 1. Create an account

Choose **Create account** and enter your details.

### 2. Select a location

Search for a city/postal code or manually provide coordinates.

### 3. Load sample data

In an empty workspace, choose:

**Try a sample rescue**

### 4. Run the AI agent

Click:

**Run rescue agent**

### 5. Inspect the result

Review:

* Agent tool events
* Proposed allocations
* Recipient selection
* Constraint results
* AI-generated summary

### 6. Approve

Click:

**Approve & create pickups**

### 7. Complete the workflow

Coordinate the pickup and click:

**Confirm handoff**

### 8. View impact

The saved handoff updates the impact information and exportable records.

---

# 🧪 Verification

The project has been tested with:

* ✅ **41 backend tests**
* ✅ **14 frontend tests**
* ✅ Docker image builds
* ✅ Healthy Docker Compose startup
* ✅ Real local Ollama inference
* ✅ Natural-language preference testing
* ✅ Approval workflow testing
* ✅ Handoff workflow testing
* ✅ Sample rescue workflow testing

Live Ollama acceptance testing demonstrated that a natural-language partner preference could change recipient allocation before approval and handoff.

---

# 📊 API

Second Serve exposes a REST API through FastAPI.

| Method       | Endpoint                      | Purpose                 |
| ------------ | ----------------------------- | ----------------------- |
| `POST`       | `/api/auth/register`          | Create account          |
| `POST`       | `/api/auth/login`             | Sign in                 |
| `POST`       | `/api/auth/logout`            | Sign out                |
| `GET`        | `/api/auth/me`                | Restore profile         |
| `POST`       | `/api/auth/location`          | Save workspace location |
| `GET`        | `/api/locations/search`       | Search locations        |
| `GET`        | `/api/agent/status`           | Agent/model status      |
| `GET`        | `/api/overview`               | Workspace statistics    |
| `GET / POST` | `/api/donations`              | Manage surplus          |
| `GET / POST` | `/api/recipients`             | Manage partners         |
| `GET / POST` | `/api/runs`                   | Rescue planning runs    |
| `GET`        | `/api/runs/{id}`              | Run status and plan     |
| `POST`       | `/api/runs/{id}/approve`      | Approve and reserve     |
| `GET`        | `/api/runs/{id}/manifest`     | JSON manifest           |
| `GET`        | `/api/missions`               | Pickup records          |
| `POST`       | `/api/missions/{id}/complete` | Confirm handoff         |
| `GET`        | `/api/missions/export.csv`    | CSV manifest            |

Interactive API documentation:

```text
http://localhost:8000/docs
```

---

# 📁 Project Structure

```text
second-serve/
│
├── backend/
│   ├── app/
│   ├── tests/
│   └── requirements.txt
│
├── frontend/
│   ├── src/
│   ├── public/
│   └── package.json
│
├── docs/
│   ├── architecture.svg
│   ├── architecture.md
│   ├── research.md
│   ├── demo-script.md
│   ├── submission.md
│   ├── verification.md
│   └── live-agent.md
│
├── compose.yaml
├── docker.yaml
├── .env.example
├── LICENSE
└── README.md
```

---

# 🗺️ Roadmap

Second Serve is currently a coordination prototype.

Future improvements include:

* 🚚 Driver and volunteer scheduling
* 🧭 Advanced route optimization
* 🔔 External partner notifications
* 📈 Advanced operational analytics
* 👥 Organization roles and invitations
* 🔐 Account recovery and email verification
* 📦 Capacity rollover
* ☁️ Production-ready hosted deployment
* ⚙️ AgentCore deployment

---

# ⚠️ Current Limitations

Second Serve does **not** currently:

* Certify food safety
* Independently verify meals
* Assign physical drivers
* Send external messages
* Model traffic or refrigeration
* Provide turn-by-turn navigation
* Operate as a shared public donor marketplace

Physical transport, communication, and food-safety checks remain human responsibilities.

---

# 🏆 Hackathon Track

## **Good Neighbor Agents**

Second Serve is built for the **Good Neighbor Agents** track.

The project helps groups of people by coordinating surplus food between donors and community organizations.

Instead of simply answering a question, the agent participates in a complete workflow:

```text
UNDERSTAND
    ↓
INSPECT
    ↓
PLAN
    ↓
VALIDATE
    ↓
APPROVE
    ↓
RESERVE
    ↓
HANDOFF
```

---

# 🌱 Why It Matters

Food shouldn't go to waste simply because coordination is difficult.

Second Serve connects surplus food with organizations that can put it to use, while combining AI assistance with deterministic safeguards and human oversight.

### **Good food. Another chance. 🍽️**

---

# 📄 License

This project is licensed under the **MIT License**.

See [`LICENSE`](LICENSE) for details.

---

## ❤️ Built with Purpose

**Second Serve**

*Good food. Another chance.*
