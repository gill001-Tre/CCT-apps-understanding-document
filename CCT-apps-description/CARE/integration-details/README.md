# CARE Integration Map
**Complete overview of CARE's integrations with external systems and endpoints**

---

## Table of Contents
1. [Visual Integration Overview](#visual-integration-overview)
2. [CARE as Central Hub](#care-as-central-hub)
3. [Inbound Integrations (Systems calling CARE)](#inbound-integrations-systems-calling-care)
4. [Outbound Integrations (CARE calling other systems)](#outbound-integrations-care-calling-other-systems)
5. [Data Flow Diagrams](#data-flow-diagrams)
6. [Integration Patterns](#integration-patterns)

---

## Visual Integration Overview

### 1. CARE Central Hub - 360° Integration View

```mermaid
graph TD
    CARE["🎯 CARE<br/>Customer Administration System<br/>━━━━━━━━━━━━━━━━━━━━<br/>Source of Truth for Customer Data"]
    
    %% DATABASE
    DB[("💾 Database<br/>Stores all customer information and transaction data")]
    
    %% INBOUND INTEGRATIONS
    IVRA["IVRA/NCCP<br/>Connects IVR phone system to CARE for customer lookup during calls"]
    
    BFF["BFF-chatbot.se<br/>Provides customer data to the chatbot interface"]
    
    Genesys["Genesys Cloud<br/>Exchanges call information and shows customer details to agents"]
    
    Agent["Agent Workspace<br/>Main interface where agents view and update customer information"]
    
    Boost["Boost.ai<br/>AI chatbot that helps customers through BFF layer"]
    
    Speed["3Speed<br/>Stores customer internet speed test results in CARE"]
    
    Emite["eMite<br/>Dashboard that monitors CARE system performance and metrics"]
    
    %% OUTBOUND INTEGRATIONS
    BIF["BIF-Ticket<br/>Receives customer issues from CARE to create support tickets"]
    
    Zendesk["Zendesk<br/>Manages customer support tickets sent through BIF"]
    
    CEL["CEL Event Log<br/>Records all actions and changes made in CARE for audit purposes"]
    
    Calabrio["Calabrio WFM<br/>Receives agent activity data for workforce planning"]
    
    IDF["IDF Dialer<br/>Gets call data through Genesys for analytics"]
    
    Indicate["Indicate Me<br/>Analyzes call recordings through Genesys"]
    
    %% BACKEND SYSTEMS
    Backend["Backend Systems<br/>Handles business operations and processes through IVRA gateway"]
    
    %% CONNECTIONS
    DB <--> CARE
    
    IVRA --> CARE
    BFF --> CARE
    Agent --> CARE
    Speed --> CARE
    Emite <--> CARE
    Boost --> BFF
    
    CARE <--> Genesys
    CARE --> BIF
    CARE --> CEL
    CARE --> Calabrio
    CARE <--> Backend
    Backend <--> IVRA
    
    BIF --> Zendesk
    Genesys --> IDF
    Genesys --> Indicate
    
    %% STYLING
    style CARE fill:#ff4444,stroke:#cc0000,stroke-width:8px,color:#fff,font-weight:bold,font-size:16px
    style DB fill:#8b0000,stroke:#000,stroke-width:3px,color:#fff
    style Genesys fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    style IVRA fill:#ff8c00,stroke:#cc6600,stroke-width:2px
    style BFF fill:#ff8c00,stroke:#cc6600,stroke-width:2px
    style BIF fill:#ff8c00,stroke:#cc6600,stroke-width:2px
    style Agent fill:#ff8c00,stroke:#cc6600,stroke-width:2px
    style Backend fill:#ff8c00,stroke:#cc6600,stroke-width:2px
    style Boost fill:#ff8c00,stroke:#cc6600,stroke-width:2px
    style Zendesk fill:#ff8c00,stroke:#cc6600,stroke-width:2px
    style Speed fill:#ffd700,stroke:#daa520,stroke-width:2px
    style Emite fill:#ffd700,stroke:#daa520,stroke-width:2px
    style CEL fill:#ffd700,stroke:#daa520,stroke-width:2px
    style Calabrio fill:#90ee90,stroke:#32cd32,stroke-width:2px
    style IDF fill:#90ee90,stroke:#32cd32,stroke-width:2px
    style Indicate fill:#90ee90,stroke:#32cd32,stroke-width:2px
```

**CARE Integration Summary:**

| App Name | What It Does | How It Works with CARE | Real Life Example |
|----------|--------------|------------------------|-------------------|
| **Database** | Saves customer data | Stores every customer detail permanently | Like a filing cabinet that keeps all customer records |
| **IVRA/NCCP** | Handles phone calls | Fetches customer info when someone calls | Customer calls support, IVR looks up their account in CARE |
| **BFF-chatbot.se** | Powers the chatbot | Gets customer info for chat conversations | Customer chats online, bot retrieves their order history from CARE |
| **Genesys Cloud** | Manages call center | Shows agent who's calling with their info | Agent sees caller's name and account details pop up on screen |
| **Agent Workspace** | Agent's work screen | Where agents read and update customer info | Agent opens app to view customer's subscription and make changes |
| **BIF-Ticket** | Creates support tickets | Takes customer problems from CARE to ticketing system | Agent creates ticket for broken service, BIF sends it to Zendesk |
| **Zendesk** | Tracks support tickets | Receives tickets from CARE through BIF | Customer issue gets tracked from creation to resolution |
| **Backend Systems** | Runs business logic | Processes orders and services through IVRA | Customer orders new service, backend systems activate it |
| **Boost.ai** | AI chatbot helper | Talks to customers using data from BFF | Customer asks bot "what's my bill?", bot checks CARE via BFF |
| **3Speed** | Tests internet speed | Sends speed test results to customer record | Customer runs speed test, result saves to their CARE profile |
| **eMite** | Shows system health | Checks how well CARE is running | Dashboard shows CARE response time and active users |
| **CEL** | Keeps audit logs | Records every action for compliance | Tracks who changed what customer data and when |
| **Calabrio WFM** | Plans agent schedules | Gets agent work data for staffing decisions | Manager sees call volume to schedule enough agents |
| **IDF Dialer** | Analyzes calls | Collects call data from Genesys | Reports on call duration and customer satisfaction |
| **Indicate Me** | Reviews call quality | Listens to recorded calls from Genesys | Analyzes agent performance and customer sentiment |

---

## CARE as Central Hub

### Customer Service Flow

```mermaid
flowchart LR
    Customer["👤 Customer<br/>Needs help"]
    
    Contact["📞 Contact Channel<br/>Phone/Chat/Email"]
    
    CARE["🎯 CARE<br/>Gets customer info"]
    
    Agent["👨‍💼 Agent<br/>Helps customer"]
    
    Resolution["✅ Resolution<br/>Problem solved"]
    
    Customer --> Contact
    Contact --> CARE
    CARE --> Agent
    Agent --> Resolution
    Resolution --> Customer
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:4px,color:#fff
    style Customer fill:#4dabf7,stroke:#1971c2,stroke-width:2px
    style Agent fill:#51cf66,stroke:#2f9e44,stroke-width:2px
    style Resolution fill:#ffd700,stroke:#daa520,stroke-width:2px
```

**How it works:**
1. **Customer contacts support** - Via phone, chat, or email
2. **System fetches customer data** - CARE provides complete customer history
3. **Agent gets full context** - Sees account details, past issues, current services
4. **Problem gets resolved** - Agent fixes issue or creates ticket for follow-up
5. **Everything is logged** - All actions recorded for compliance and quality

---

### 2. Chatbot Customer Journey

```mermaid
flowchart LR
    Customer["👤 Customer<br/>On website"]
    
    Chatbot["💬 Boost.ai<br/>Chat assistant"]
    
    BFF["🔌 BFF Layer<br/>Gets data"]
    
    CARE["🎯 CARE<br/>Customer info"]
    
    Response["✅ Answer<br/>Provided"]
    
    Customer --> Chatbot
    Chatbot --> BFF
    BFF --> CARE
    CARE --> BFF
    BFF --> Chatbot
    Chatbot --> Response
    Response --> Customer
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:3px,color:#fff
    style Customer fill:#4dabf7,stroke:#1971c2,stroke-width:2px
    style Chatbot fill:#51cf66,stroke:#2f9e44,stroke-width:2px
```

**How it works:**
1. **Customer asks chatbot** - Types question on website
2. **Chatbot requests data** - Via BFF layer to CARE
3. **CARE provides info** - Customer balance, orders, services
4. **Chatbot responds** - Instant answer 24/7

---

### 3. CARE Data Flow - Inbound vs Outbound

```mermaid
graph LR
    subgraph IN["📥 INBOUND to CARE"]
        IN1[Agent Workspace]
        IN2[BFF-chatbot.se]
        IN3[IVRA/NCCP]
        IN4[eMite]
        IN5[3Speed]
    end
    
    CARE["🎯 CARE<br/>REST APIs"]
    DB[(Database)]
    
    subgraph OUT["📤 OUTBOUND from CARE"]
        OUT1[Zendesk]
        OUT2[CEL]
        OUT3[Backend Systems]
        OUT4[Genesys]
        OUT5[Calabrio]
    end
    
    IN1 --> CARE
    IN2 --> CARE
    IN3 --> CARE
    IN4 --> CARE
    IN5 --> CARE
    
    CARE <--> DB
    
    CARE --> OUT1
    CARE --> OUT2
    CARE <--> OUT3
    CARE --> OUT4
    CARE --> OUT5
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:5px,color:#fff
    style DB fill:#fa5252,stroke:#c92a2a,stroke-width:3px,color:#fff
```

**Simple explanation:**
- **Inbound**: These apps request data from CARE
- **Outbound**: CARE sends data to these apps
- **Database**: CARE stores all customer data here

---

## Inbound Integrations (Systems calling CARE)

These systems **call CARE APIs** to retrieve or update customer information:

### 1. **BFF-chatbot.se** → CARE
**Integration Type:** REST API calls  
**Direction:** BFF calls CARE  
**Purpose:** Fetch customer data for personalized chatbot responses  
**Data Flow:**
- Customer uses chatbot on website
- BFF-chatbot.se requests customer info from CARE
- CARE returns: name, account details, order history
- Chatbot uses data to personalize responses

**Example API Calls:**
- `GET /willow/api/customer/{id}` - Get customer profile
- `GET /willow/api/orders/{customerId}` - Get customer orders
- `GET /willow/api/account/{id}` - Get account details

---

### 2. **Boost.ai** → CARE (via BFF-chatbot.se)
**Integration Type:** Indirect via BFF  
**Direction:** Boost.ai → BFF-chatbot.se → CARE  
**Purpose:** AI chatbot needs customer context  
**Data Flow:**
- Boost.ai chatbot handles customer conversation
- Calls BFF-chatbot.se for customer data
- BFF retrieves from CARE
- Chatbot provides personalized assistance

---

### 3. **Agent Workspace/Frontend** → CARE
**Integration Type:** Direct REST API calls  
**Direction:** Frontend calls CARE backend  
**Purpose:** Agents view and update customer information  
**Data Flow:**
- Agent searches for customer in portal
- Frontend calls CARE API
- CARE returns complete customer profile
- Agent can view/edit customer details

**Common Operations:**
- Search customers
- View customer details
- Update account information
- View service subscriptions
- View contact history
- Manage customer cases

---

### 4. **IVRA/NCCP** → CARE
**Integration Type:** REST API calls from IVR system  
**Direction:** IVRA/NCCP calls CARE  
**Purpose:** IVR needs customer data during phone calls  
**Data Flow:**
- Customer calls in and enters account number
- IVR (via IVRA/NCCP) calls CARE to validate account
- CARE returns account status, balance, etc.
- IVR speaks information to customer

**Example Scenarios:**
- Account balance inquiry
- Service status check
- Account validation
- Recent order lookup

---

### 5. **eMite Dashboard** → CARE
**Integration Type:** Read-only data access  
**Direction:** eMite reads from CARE  
**Purpose:** Display live metrics to contact center teams  
**Data Flow:**
- eMite pulls customer service metrics from CARE
- Displays real-time dashboard data
- Shows agent activity and customer interactions

---

### 6. **3Speed** → CARE
**Integration Type:** Writes speed test results to CARE  
**Direction:** 3Speed updates CARE  
**Purpose:** Store network speed test results  
**Data Flow:**
- Agent initiates speed test for customer
- 3Speed runs test via third-party API
- Results written back to CARE
- Agent sees results in CARE portal

---

## Outbound Integrations (CARE calling other systems)

These are systems that **CARE calls** to perform actions or retrieve external data:

### 1. CARE → **Zendesk** (via BIF-Ticket)
**Integration Type:** REST API calls  
**Direction:** CARE → BIF-Ticket → Zendesk  
**Purpose:** Create and manage support tickets  
**Data Flow:**
- Agent creates ticket from CARE
- CARE calls BIF-Ticket service
- BIF-Ticket formats data and creates ticket in Zendesk
- Ticket ID returned to CARE

**Operations:**
- Create support tickets
- Update ticket status
- Add notes to tickets
- Link customer to ticket

---

### 2. CARE → **CEL (Communication Event Log)**
**Integration Type:** Event logging  
**Direction:** CARE → CEL  
**Purpose:** Log all customer communications  
**Data Flow:**
- Any customer interaction in CARE (call, email, chat)
- CARE sends event to CEL
- CEL stores complete audit trail
- Searchable history for compliance

**Logged Events:**
- Customer phone calls
- Emails sent/received
- Chat conversations
- SMS communications
- Agent interactions

---

### 3. CARE → **Genesys Cloud** (via NCCP/IVRA)
**Integration Type:** Two-way integration  
**Direction:** Bidirectional  
**Purpose:** Contact center operations  
**Data Flow:**

**CARE → Genesys:**
- Trigger outbound calls
- Update customer interaction records
- Screen pop customer info to agents

**Genesys → CARE:**
- Call metadata
- Call recordings references
- Agent activity logs

---

### 4. CARE → **External Backend Systems**
**Integration Type:** REST APIs via IVRA/NCCP  
**Direction:** CARE → IVRA/NCCP → Backend Systems  
**Purpose:** Access business systems for customer services  
**Examples:**
- Billing systems
- Provisioning systems
- Product catalog
- Order management systems
- CRM systems

---

### 5. CARE → **Calabrio**
**Integration Type:** Data export for WFM  
**Direction:** CARE → Calabrio  
**Purpose:** Workforce management and scheduling  
**Data Flow:**
- CARE provides agent activity data
- Calabrio uses for forecasting and scheduling
- Contact volume statistics
- Agent performance metrics

---

### 6. CARE → **Brilliant** (Indirectly)
**Integration Type:** Event trigger  
**Direction:** CARE → Genesys → Brilliant  
**Purpose:** Trigger post-call surveys  
**Data Flow:**
- Call ends in Genesys
- Call metadata includes customer ID from CARE
- Brilliant uses customer info to send survey

---

## Data Flow Diagrams

### Customer Service Interaction Flow

```
Customer Contact
    ↓
Genesys Cloud (Phone/Chat)
    ↓
IVRA/NCCP (Integration Layer)
    ↓
CARE Backend ← → Frontend (Agent Workspace)
    ↓
CARE retrieves/updates:
├─→ Customer Profile
├─→ Account Details  
├─→ Service Subscriptions
├─→ Order History
└─→ Interaction History
    ↓
Agent assists customer
    ↓
CARE logs interaction → CEL (Communication Event Log)
    ↓
If ticket needed → BIF-Ticket → Zendesk
    ↓
Call ends → Brilliant sends survey
```

---

### Chatbot Interaction Flow

```
Customer visits website
    ↓
Boost.ai Chatbot (3SE or Hallon instance)
    ↓
BFF-chatbot.se (Backend for Frontend)
    ↓
CARE API
    ↓
Returns customer data:
├─→ Name
├─→ Account status
├─→ Recent orders
├─→ Service issues
└─→ Support history
    ↓
Chatbot provides personalized help
    ↓
If issue detected → Can create ticket via BIF-Ticket → Zendesk
    ↓
Or trigger speed test → 3Speed → Results to CARE
```

---

### Agent Support Tools Flow

```
Agent Workspace (Frontend)
    ↓
CARE Backend APIs
    ↓
Integrates with support tools:
    ↓
├─→ eMite (Live Metrics Dashboard)
│   Shows call queue, agent status
│
├─→ 3Speed (Network Diagnostics)
│   Run speed test for customer
│   Results stored in CARE
│
├─→ CEL (Communication History)
│   Full timeline of customer contacts
│
└─→ Zendesk (via BIF-Ticket)
    Create/view support tickets
```

---

### Analytics & Reporting Flow

```
Genesys Cloud (Call Data)
    ↓
IDF Dialer (Data Fetcher)
    ↓
SAS Database
    ↓
Business Intelligence Reports
    -----
Genesys Cloud (Call Recordings)
    ↓
Indicate Me (Speech-to-Text AI)
    ↓
AI Insights & Coaching Reports
    -----
CARE (Customer Interaction Data)
    ↓
CEL (Communication Event Log)
    ↓
Compliance & Audit Reports
```

---

## Integration Patterns

### 1. **BFF Pattern (Backend for Frontend)**
- **Example:** BFF-chatbot.se
- **Purpose:** Specialized API layer for chatbot needs
- **Benefit:** Chatbot gets exactly the data format it needs without exposing all CARE APIs

### 2. **API Gateway Pattern**
- **Example:** IVRA/NCCP
- **Purpose:** Single entry point for IVR to access multiple backend systems
- **Benefit:** IVR doesn't need to know about individual backend services

### 3. **Event-Driven Integration**
- **Example:** CARE → CEL
- **Purpose:** Asynchronous logging of all events
- **Benefit:** No blocking, guaranteed audit trail

### 4. **Synchronous REST APIs**
- **Example:** Frontend → CARE, BFF → CARE
- **Purpose:** Real-time data retrieval and updates
- **Benefit:** Immediate response for user-facing operations

### 5. **Service Mesh Integration**
- **Example:** CARE ↔ External Backend Systems via IVRA/NCCP
- **Purpose:** Decoupled service communication
- **Benefit:** Services can evolve independently

---

## CARE API Endpoints Overview

### Customer Domain
- `GET /willow/api/customer/{id}` - Get customer details
- `POST /willow/api/customer` - Create customer
- `PUT /willow/api/customer/{id}` - Update customer
- `GET /willow/api/customer/search` - Search customers

### Account Domain
- `GET /willow/api/account/{id}` - Get account info
- `PUT /willow/api/account/{id}` - Update account

### Orders Domain
- `GET /willow/api/orders/{customerId}` - Get customer orders
- `POST /willow/api/orders` - Create order

### Communication Domain
- `POST /willow/api/communication/log` - Log communication event
- `GET /willow/api/communication/{customerId}` - Get communication history

### Speed Testing Domain
- `POST /willow/api/speed/test` - Initiate speed test
- `GET /willow/api/speed/results/{testId}` - Get test results

### Feature Toggles Domain
- `GET /willow/api/featuretoggles` - Get enabled features
- `POST /willow/api/featuretoggles` - Update feature state

---

## Integration Security

### Authentication Methods
- **OAuth 2.0** - Used for external SaaS integrations
- **API Keys** - Used for internal service-to-service calls
- **JWT Tokens** - Used for frontend-to-backend authentication

### Authorization
- **Role-Based Access Control (RBAC)** - Agents have different permission levels
- **Scope-Based Permissions** - External systems limited to specific API scopes

---

## System Dependencies Map

```
CARE depends on:
├─→ H2 Database (local dev) / Production Database
├─→ CEL (for logging)
├─→ External Backend Systems (via IVRA/NCCP)
└─→ Zendesk (via BIF-Ticket)

Systems that depend on CARE:
├─→ BFF-chatbot.se
├─→ Boost.ai (indirectly via BFF)
├─→ Agent Workspace Frontend
├─→ IVRA/NCCP (for IVR data)
├─→ eMite (for metrics)
├─→ 3Speed (stores results in CARE)
├─→ Genesys Cloud (screen pop data)
└─→ Reporting Systems (via CEL)
```

---

## Critical Integration Points

### 1. **CARE ↔ Genesys Cloud** (Most Critical)
- **Why:** Core contact center functionality
- **Impact if down:** Agents can't get customer context during calls
- **Fallback:** Manual customer lookup

### 2. **CARE ↔ BFF-chatbot.se** (High Priority)
- **Why:** Customer-facing chatbot
- **Impact if down:** Chatbot can't personalize responses
- **Fallback:** Generic chatbot responses

### 3. **CARE ↔ Zendesk** (High Priority)
- **Why:** Ticket creation and management
- **Impact if down:** Can't create support tickets from CARE
- **Fallback:** Manual ticket creation in Zendesk

### 4. **CARE ↔ CEL** (Compliance Critical)
- **Why:** Legal audit trail requirement
- **Impact if down:** No communication logging
- **Fallback:** Queue events, replay when CEL is back

---

## Integration Monitoring

### Health Check Endpoints
All CARE integrations should implement health checks:
- `/actuator/health` - Overall system health
- `/actuator/health/database` - Database connectivity
- `/actuator/health/external-apis` - External API status

### Monitoring Tools
- **APM (Application Performance Monitoring)** - Track API response times
- **Logging** - Centralized logging for troubleshooting
- **Alerts** - Notifications when integrations fail

---

## Future Integration Opportunities

### Planned Integrations
1. **RPA Integration** - Automate repetitive tasks with CARE data
2. **Advanced Analytics** - Real-time dashboards pulling from CARE
3. **Mobile App** - Native mobile access to CARE APIs
4. **WhatsApp/Social Media** - New contact channels integrated with CARE

---

**Document Version:** 2.0  
**Last Updated:** March 3, 2026  
**Maintained By:** CARE Backend Team  
**Contact:** CARE Architecture Team for integration questions
