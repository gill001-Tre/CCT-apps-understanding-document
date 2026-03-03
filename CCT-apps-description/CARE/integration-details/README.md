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

### 2. CARE Complete Integration Architecture

```mermaid
graph TB
    Customer["👤 CUSTOMER<br/>Contacts support via<br/>phone, chat, or ticket"]
    
    subgraph Channel["📞 CUSTOMER CONTACT CHANNELS"]
        Phone["Genesys Cloud<br/>Call center platform<br/>Routes calls to CARE"]
        Web["Boost.ai Chatbot<br/>AI chat assistant<br/>Gets data via BFF"]
        Email["Zendesk<br/>Support tickets<br/>Receives from BIF"]
    end
    
    subgraph Integration["🔌 INTEGRATION LAYER - Bridge between channels and CARE"]
        IVRA["IVRA/NCCP<br/>Phone system gateway<br/>Fetches customer info for calls"]
        BFF["BFF-chatbot.se<br/>Chat data provider<br/>Supplies customer data to chatbot"]
        BIF["BIF-Ticket<br/>Ticket creator<br/>Sends issues to Zendesk"]
    end
    
    CARE["🎯 CARE<br/>Customer Administration System<br/>━━━━━━━━━━━━━━━━━━<br/>Central hub for all customer data"]
    
    DB[("💾 Database<br/>Stores customer<br/>records")]
    
    subgraph Support["🛠️ SUPPORT TOOLS - Connected to CARE"]
        Speed["3Speed<br/>Speed test tool<br/>Saves results to CARE"]
        Emite["eMite<br/>Monitoring dashboard<br/>Reads CARE metrics"]
        CEL["CEL Event Log<br/>Audit tracker<br/>Records CARE actions"]
    end
    
    subgraph Analytics["📊 ANALYTICS - Report on customer interactions"]
        IDF["IDF Dialer<br/>Call analytics<br/>Via Genesys"]
        Indicate["Indicate Me<br/>Call quality<br/>Via Genesys"]
        Brilliant["Brilliant<br/>Performance tracking<br/>Via Genesys"]
        Calabrio["Calabrio WFM<br/>Staff planning<br/>Gets agent data from CARE"]
    end
    
    Backend["🏢 Backend Systems<br/>Business operations<br/>Connects through IVRA"]
    
    Customer --> Phone
    Customer --> Web
    Customer --> Email
    
    Phone --> IVRA
    Web --> BFF
    Email --> BIF
    
    IVRA -->|Looks up customer| CARE
    BFF -->|Gets customer info| CARE
    BIF -->|Reads customer data| CARE
    
    CARE <-->|Stores/retrieves| DB
    
    CARE -->|Sends events| CEL
    Speed -->|Saves test results| CARE
    Emite -->|Polls metrics| CARE
    
    CARE <-->|Exchanges data| Backend
    Backend <-->|Via gateway| IVRA
    
    CARE -->|Creates tickets| BIF
    BIF --> Email
    
    Phone -->|Call recordings| IDF
    Phone -->|Audio files| Indicate
    Phone -->|Call events| Brilliant
    CARE -->|Agent activity| Calabrio
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:6px,color:#fff
    style DB fill:#fa5252,stroke:#c92a2a,stroke-width:4px,color:#fff
    style Customer fill:#4dabf7,stroke:#1971c2,stroke-width:3px
    style IVRA fill:#51cf66,stroke:#2f9e44,stroke-width:2px
    style BFF fill:#51cf66,stroke:#2f9e44,stroke-width:2px
    style BIF fill:#51cf66,stroke:#2f9e44,stroke-width:2px
```

---

### 3. Customer Service Agent Workflow

```mermaid
sequenceDiagram
    participant C as Customer
    participant G as Genesys Cloud
    participant I as IVRA
    participant CARE as CARE Backend
    participant A as Agent Desktop
    participant S as Support Tools
    participant Z as Zendesk
    participant CEL as Event Log
    
    Note over C,CEL: Phone Call Journey
    
    C->>G: Calls support number
    G->>I: Route to IVR
    I->>C: IVR Menu
    C->>I: Enter account number: 12345
    
    I->>CARE: GET /api/customer/lookup?account=12345
    CARE-->>I: Anna Svensson, Active, Premium
    
    I->>G: Customer validated
    G->>A: Screen pop with customer data
    A->>CARE: GET /api/customer/full-profile
    CARE-->>A: Complete customer details
    
    G->>C: Agent connected
    C->>A: Reports slow internet
    
    alt Speed Test Required
        A->>S: Trigger 3Speed test
        S->>S: Run test (30 seconds)
        S->>CARE: POST /api/speed-test/result
        S-->>A: Result: 10 Mbps (expected 100)
    end
    
    Note over A,Z: Create Support Ticket
    
    A->>CARE: POST /api/ticket/create
    CARE->>Z: Create ticket via BIF
    Z-->>CARE: Ticket #67890 created
    CARE-->>A: Confirmation
    
    A->>C: Ticket created, tech will call within 24h
    
    Note over G,CEL: Post-Call Processing
    
    G->>CARE: Log call interaction
    CARE->>CEL: Event: Call completed, 8 min, ticket created
    G->>S: Trigger survey
    S->>C: SMS: Rate your experience 1-5
```

**Workflow Details:**
- **Average call duration:** 5-8 minutes
- **IVR lookup time:** < 200ms
- **Screen pop data:** Customer name, account status, recent orders, open tickets
- **Common diagnostics:** 3Speed network test (30 sec), account balance check
- **Escalation:** Creates Zendesk ticket with full context
- **Compliance:** All interactions logged to CEL for GDPR/audit

---

### 3. Chatbot Customer Journey

```mermaid
flowchart LR
    C[Customer on Website]
    Bot[Boost.ai Widget]
    BFF[BFF-chatbot.se]
    CARE[CARE APIs]
    DB[(Database)]
    
    Actions{Customer Request}
    
    C --> Bot
    Bot --> BFF
    BFF --> CARE
    CARE <--> DB
    
    Bot --> Actions
    
    Actions -->|Check Balance| Balance[Show Balance]
    Actions -->|Speed Test| Test[Run 3Speed]
    Actions -->|Order Status| Orders[Show Orders]
    Actions -->|Need Help| Ticket[Create Ticket]
    
    Balance --> CARE
    Test --> 3Speed[3Speed Tool]
    Orders --> CARE
    Ticket --> Zendesk[Zendesk]
    
    3Speed --> CARE
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:3px,color:#fff
    style C fill:#4dabf7,stroke:#1971c2,stroke-width:2px
```

**Chatbot Capabilities:**
- **Self-service:** Balance inquiry, order status, service info
- **Diagnostics:** Trigger speed tests, check service status
- **Escalation:** Create tickets for agent follow-up
- **Languages:** Swedish, English
- **Context:** Pulls customer history from CARE
- **Availability:** 24/7 automated support

---

### 4. CARE Data Flow - Inbound vs Outbound

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
    
    IN1 -->|READ/WRITE All endpoints| CARE
    IN2 -->|READ Customer & orders| CARE
    IN3 -->|READ Account validation| CARE
    IN4 -->|READ Metrics| CARE
    IN5 -->|WRITE Test results| CARE
    
    CARE <--> DB
    
    CARE -->|Create tickets| OUT1
    CARE -->|Log events| OUT2
    CARE <-->|Query services| OUT3
    CARE -->|Customer context| OUT4
    CARE -->|Agent metrics| OUT5
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:5px,color:#fff
    style DB fill:#fa5252,stroke:#c92a2a,stroke-width:3px,color:#fff
```

**API Operations Summary:**

**Inbound (Reading from CARE):**
- Agent Workspace: Full CRUD access to all customer data
- BFF-chatbot.se: `GET /customer`, `GET /orders` (read-only)
- IVRA/NCCP: `GET /account/validate` (read-only)
- eMite: `GET /metrics` (read-only)
- 3Speed: `POST /speed-test/result` (write only)

**Outbound (CARE calling others):**
- Zendesk: `POST /api/tickets` via BIF-Ticket
- CEL: `POST /events` for all interactions
- Backend Systems: Various REST APIs for billing, provisioning, orders
- Genesys: Screen pop data on call connect
- Calabrio: Daily export of agent activity

---

### 5. Real-Time Call Center Operations

```mermaid
stateDiagram-v2
    [*] --> CallReceived
    
    CallReceived --> IVR: Route call
    IVR --> Validation: Customer enters account
    
    Validation --> LookupCARE: Query customer
    LookupCARE --> Found: 200 OK
    LookupCARE --> NotFound: 404 Not Found
    
    NotFound --> IVR: Retry
    Found --> SecurityCheck: Validate PIN
    
    SecurityCheck --> Passed: PIN correct
    SecurityCheck --> Failed: PIN wrong
    
    Failed --> SecurityCheck: Retry (max 3)
    Failed --> TransferAgent: Max attempts
    
    Passed --> AgentQueue: Route to agent
    AgentQueue --> Connected: Agent answers
    
    state Connected {
        [*] --> ScreenPop: View customer 360
        ScreenPop --> HandleIssue: Customer explains
        HandleIssue --> Diagnostics: Run tests if needed
        Diagnostics --> CreateTicket: Escalate
        CreateTicket --> LogInteraction: Update CARE
        LogInteraction --> [*]
    }
    
    Connected --> CallEnds: Resolve issue
    CallEnds --> LogCEL: Compliance logging
    LogCEL --> Survey: Send feedback request
    Survey --> [*]
```

**State Descriptions:**

| State | Duration | CARE Interaction | Notes |
|-------|----------|------------------|-------|
| IVR | 30-60 sec | None | Menu navigation |
| Validation | 2-5 sec | GET /customer/lookup | < 200ms API response |
| Security Check | 10-20 sec | Compare PIN hash | Max 3 attempts |
| Agent Queue | 0-120 sec | None | Target < 2 min wait |
| Screen Pop | Instant | GET /full-profile | Shows customer 360 |
| Handle Issue | 3-7 min | Various reads/writes | Main conversation |
| Create Ticket | 5-10 sec | POST /ticket/create | Via BIF to Zendesk |
| Log CEL | < 1 sec | POST /events | Compliance requirement |
| Survey | Async | None | Brilliant sends SMS |

---

### 6. System Dependencies & Priority

```mermaid
graph TD
    subgraph CRITICAL["⚠️ CRITICAL"]
        DB[Database]
        NET[Network]
        GEN[Genesys Cloud]
    end
    
    CARE["🎯 CARE"]
    
    subgraph HIGH["🟠 HIGH PRIORITY"]
        BFF[BFF-chatbot.se]
        IVRA[IVRA/NCCP]
        BIF[BIF-Ticket]
    end
    
    subgraph MEDIUM["🟡 MEDIUM PRIORITY"]
        CEL[CEL Event Log]
        SPEED[3Speed]
        EMITE[eMite]
    end
    
    subgraph ASYNC["🟢 ASYNC - Can Delay"]
        IDF[IDF Dialer]
        IND[Indicate Me]
        CAL[Calabrio]
    end
    
    DB -.->|No data = Outage| CARE
    NET -.->|No network = Offline| CARE
    GEN -.->|No calls possible| CARE
    
    CARE ==>|Customer context| BFF
    CARE ==>|Account validation| IVRA
    CARE ==>|Ticket data| BIF
    
    CARE -->|Event logging| CEL
    CARE -->|Test storage| SPEED
    CARE -->|Metrics| EMITE
    
    CARE -.->|Batch export| IDF
    CARE -.->|Agent metrics| CAL
    GEN -.->|Recordings| IND
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:6px,color:#fff
    style DB fill:#e03131,stroke:#8b0000,stroke-width:4px,color:#fff
    style NET fill:#e03131,stroke:#8b0000,stroke-width:4px,color:#fff
    style GEN fill:#e03131,stroke:#8b0000,stroke-width:4px,color:#fff
    style BFF fill:#ff922b,stroke:#e67700,stroke-width:3px
    style IVRA fill:#ff922b,stroke:#e67700,stroke-width:3px
    style BIF fill:#ff922b,stroke:#e67700,stroke-width:3px
```

**Dependency Impact Analysis:**

| System | Priority | Impact if Down | Fallback Strategy | Recovery |
|--------|----------|----------------|-------------------|----------|
| **Database** | 🔴 CRITICAL | Complete outage | None - immediate escalation | < 15 min SLA |
| **Network** | 🔴 CRITICAL | No connectivity | None - infrastructure team | Immediate |
| **Genesys Cloud** | 🔴 CRITICAL | No calls | Vendor hotline | Vendor SLA |
| **IVRA/NCCP** | 🟠 HIGH | IVR can't validate | Route all to agents | 1-2 hours |
| **BFF-chatbot** | 🟠 HIGH | Chatbot degraded | Generic responses | 1-2 hours |
| **BIF-Ticket** | 🟠 HIGH | Can't auto-create tickets | Manual Zendesk entry | 2-4 hours |
| **CEL** | 🟡 MEDIUM | No audit logging | Queue events, replay later | 4-8 hours |
| **3Speed** | 🟡 MEDIUM | No diagnostics | Manual external tests | 4-8 hours |
| **eMite** | 🟡 MEDIUM | No live metrics | Use Genesys reports | Next day |
| **IDF Dialer** | 🟢 ASYNC | Delayed analytics | Batch catch-up | Next batch |
| **Indicate Me** | 🟢 ASYNC | No AI insights | Process backlog | Not time-critical |
| **Calabrio** | 🟢 ASYNC | No auto-scheduling | Manual schedules | Next day |

---

## CARE as Central Hub

**CARE - Customer Administration** is the central customer data repository that serves as the main hub for customer information across the entire ecosystem.

**Core Function:** 
- Stores and manages all customer information (personal details, account info, service subscriptions, contact history)
- Acts as the **source of truth** for customer data
- All other systems pull customer data from CARE or push updates to CARE

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
