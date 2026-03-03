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
    
    %% CRITICAL DEPENDENCIES - Top
    DB[("💾 DATABASE<br/>━━━━━━━━━━<br/>🔴 CRITICAL<br/>━━━━━━━━━━<br/>Function: Data storage<br/>Endpoint: JDBC<br/>Pattern: Direct DB")]
    
    %% HIGH PRIORITY INTEGRATIONS - Right Side
    IVRA["IVRA/NCCP<br/>━━━━━━━━━━<br/>🟠 HIGH<br/>━━━━━━━━━━<br/>Function: IVR bridge<br/>Endpoints:<br/>GET /customer/lookup<br/>GET /account/validate<br/>Pattern: REST API<br/>Direction: INBOUND"]
    
    BFF["BFF-chatbot.se<br/>━━━━━━━━━━<br/>🟠 HIGH<br/>━━━━━━━━━━<br/>Function: Chat data layer<br/>Endpoints:<br/>GET /customer/{id}<br/>GET /orders/{id}<br/>Pattern: BFF<br/>Direction: INBOUND"]
    
    Genesys["Genesys Cloud<br/>━━━━━━━━━━<br/>🔴 CRITICAL<br/>━━━━━━━━━━<br/>Function: Contact center<br/>Endpoint: Screen pop API<br/>Pattern: Bidirectional<br/>Direction: BOTH"]
    
    %% MEDIUM PRIORITY - Right Bottom
    BIF["BIF-Ticket<br/>━━━━━━━━━━<br/>🟠 HIGH<br/>━━━━━━━━━━<br/>Function: Ticket handler<br/>Endpoint: POST /ticket<br/>Pattern: REST API<br/>Direction: OUTBOUND"]
    
    Zendesk["Zendesk<br/>━━━━━━━━━━<br/>🟠 HIGH<br/>━━━━━━━━━━<br/>Function: Ticketing<br/>Via: BIF-Ticket<br/>Pattern: REST via BIF<br/>Direction: OUTBOUND"]
    
    CEL["CEL Event Log<br/>━━━━━━━━━━<br/>🟡 MEDIUM<br/>━━━━━━━━━━<br/>Function: Audit logging<br/>Endpoint: POST /events<br/>Pattern: Event-driven<br/>Direction: OUTBOUND"]
    
    %% SUPPORT TOOLS - Left Side
    Speed["3Speed<br/>━━━━━━━━━━<br/>🟡 MEDIUM<br/>━━━━━━━━━━<br/>Function: Speed test<br/>Endpoint:<br/>POST /speed-test/result<br/>Pattern: REST API<br/>Direction: INBOUND"]
    
    Emite["eMite<br/>━━━━━━━━━━<br/>🟡 MEDIUM<br/>━━━━━━━━━━<br/>Function: Metrics dashboard<br/>Endpoint: GET /metrics<br/>Pattern: REST API<br/>Direction: INBOUND"]
    
    Agent["Agent Workspace<br/>━━━━━━━━━━<br/>🟠 HIGH<br/>━━━━━━━━━━<br/>Function: Agent UI<br/>Endpoints: All CRUD<br/>Pattern: REST API<br/>Direction: INBOUND"]
    
    %% ANALYTICS - Bottom
    Calabrio["Calabrio WFM<br/>━━━━━━━━━━<br/>🟢 ASYNC<br/>━━━━━━━━━━<br/>Function: Workforce mgmt<br/>Endpoint: Daily export<br/>Pattern: Batch export<br/>Direction: OUTBOUND"]
    
    IDF["IDF Dialer<br/>━━━━━━━━━━<br/>🟢 ASYNC<br/>━━━━━━━━━━<br/>Function: Analytics export<br/>Pattern: Indirect via Genesys<br/>Direction: OUTBOUND"]
    
    Indicate["Indicate Me<br/>━━━━━━━━━━<br/>🟢 ASYNC<br/>━━━━━━━━━━<br/>Function: Speech-to-text<br/>Pattern: Indirect via Genesys<br/>Direction: OUTBOUND"]
    
    %% BACKEND SYSTEMS - Left Bottom
    Backend["Backend Systems<br/>━━━━━━━━━━<br/>🟠 HIGH<br/>━━━━━━━━━━<br/>Function: Business ops<br/>Via: IVRA/NCCP<br/>Pattern: Service mesh<br/>Direction: BOTH"]
    
    Boost["Boost.ai<br/>━━━━━━━━━━<br/>🟠 HIGH<br/>━━━━━━━━━━<br/>Function: Chatbot<br/>Via: BFF-chatbot.se<br/>Pattern: BFF<br/>Direction: INBOUND"]
    
    %% CONNECTIONS - Database (Critical)
    DB <-->|"All data<br/>JDBC connection"| CARE
    
    %% CONNECTIONS - High Priority Inbound
    IVRA -->|"READ customer<br/>Sync REST"| CARE
    BFF -->|"READ customer/orders<br/>Sync REST"| CARE
    Agent -->|"CRUD all data<br/>Sync REST"| CARE
    Boost -->|"Via BFF layer<br/>Indirect"| BFF
    
    %% CONNECTIONS - High Priority Outbound
    CARE -->|"Screen pop data<br/>Real-time"| Genesys
    CARE -->|"Create tickets<br/>Via BIF"| BIF
    BIF -->|"POST ticket<br/>Sync REST"| Zendesk
    
    %% CONNECTIONS - Medium Priority
    Speed -->|"WRITE test results<br/>Async POST"| CARE
    Emite <-->|"READ metrics<br/>Polling"| CARE
    CARE -->|"Log all events<br/>Async POST"| CEL
    
    %% CONNECTIONS - Backend
    CARE <-->|"Via IVRA/NCCP<br/>Service mesh"| Backend
    
    %% CONNECTIONS - Analytics (Async)
    CARE -->|"Agent data<br/>Daily batch"| Calabrio
    Genesys -->|"Call data<br/>Scheduled export"| IDF
    Genesys -->|"Recordings<br/>On-demand"| Indicate
    
    %% STYLING
    style CARE fill:#ff4444,stroke:#cc0000,stroke-width:8px,color:#fff,font-weight:bold,font-size:16px
    style DB fill:#8b0000,stroke:#000,stroke-width:5px,color:#fff
    style Genesys fill:#dc143c,stroke:#8b0000,stroke-width:4px,color:#fff
    
    style IVRA fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    style BFF fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    style BIF fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    style Agent fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    style Backend fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    style Boost fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    style Zendesk fill:#ff8c00,stroke:#cc6600,stroke-width:3px
    
    style Speed fill:#ffd700,stroke:#daa520,stroke-width:2px
    style Emite fill:#ffd700,stroke:#daa520,stroke-width:2px
    style CEL fill:#ffd700,stroke:#daa520,stroke-width:2px
    
    style Calabrio fill:#90ee90,stroke:#32cd32,stroke-width:2px
    style IDF fill:#90ee90,stroke:#32cd32,stroke-width:2px
    style Indicate fill:#90ee90,stroke:#32cd32,stroke-width:2px
```

**CARE Integration Summary:**

| System | Priority | Function | Integration Pattern | Endpoint Examples | Direction |
|--------|----------|----------|---------------------|-------------------|-----------|
| **Database** | 🔴 CRITICAL | Data persistence | Direct JDBC | N/A (database connection) | Bidirectional |
| **Genesys Cloud** | 🔴 CRITICAL | Contact center platform | Bidirectional API | Screen pop, call metadata | BOTH |
| **IVRA/NCCP** | 🟠 HIGH | IVR backend bridge | Synchronous REST | `GET /customer/lookup`<br>`GET /account/validate` | INBOUND |
| **BFF-chatbot.se** | 🟠 HIGH | Chat aggregation layer | BFF Pattern | `GET /customer/{id}`<br>`GET /orders/{customerId}` | INBOUND |
| **Agent Workspace** | 🟠 HIGH | Agent UI | Synchronous REST | All CRUD endpoints | INBOUND |
| **BIF-Ticket** | 🟠 HIGH | Ticket creation handler | REST via middleware | `POST /ticket/create` | OUTBOUND |
| **Zendesk** | 🟠 HIGH | Ticket management | REST via BIF | Via BIF-Ticket only | OUTBOUND |
| **Backend Systems** | 🟠 HIGH | Business operations | Service mesh via IVRA/NCCP | Various via gateway | BOTH |
| **Boost.ai** | 🟠 HIGH | AI chatbot | Indirect via BFF | Via BFF-chatbot.se | INBOUND (indirect) |
| **3Speed** | 🟡 MEDIUM | Network diagnostics | Asynchronous REST | `POST /speed-test/result` | INBOUND |
| **eMite** | 🟡 MEDIUM | Live metrics | Polling REST | `GET /metrics` | INBOUND |
| **CEL** | 🟡 MEDIUM | Compliance logging | Event-driven | `POST /events` | OUTBOUND |
| **Calabrio WFM** | 🟢 ASYNC | Workforce management | Batch export | Daily data export | OUTBOUND |
| **IDF Dialer** | 🟢 ASYNC | Call analytics | Indirect via Genesys | N/A (indirect) | OUTBOUND (indirect) |
| **Indicate Me** | 🟢 ASYNC | Speech analytics | Indirect via Genesys | N/A (indirect) | OUTBOUND (indirect) |

**Integration Patterns Used:**
- **BFF (Backend for Frontend):** BFF-chatbot.se provides specialized API for Boost.ai chatbot
- **API Gateway:** IVRA/NCCP acts as gateway to backend systems
- **Event-Driven:** CEL receives events asynchronously
- **REST API:** Most integrations use synchronous REST
- **Service Mesh:** Backend systems accessed via IVRA/NCCP gateway
- **Direct Database:** CARE has direct JDBC connection to database
- **Batch Processing:** Calabrio receives daily exports

---

### 2. CARE Complete Integration Architecture

```mermaid
graph TB
    Customer["👤 CUSTOMER"]
    
    subgraph Channel["📞 CUSTOMER CONTACT CHANNELS"]
        Phone["Genesys Cloud"]
        Web["Boost.ai Chatbot"]
        Email["Zendesk"]
    end
    
    subgraph Integration["🔌 INTEGRATION LAYER"]
        IVRA["IVRA / NCCP"]
        BFF["BFF-chatbot.se"]
        BIF["BIF-Ticket"]
    end
    
    CARE["🎯 CARE<br/>Customer Administration System"]
    
    DB[("💾 Database")]
    
    subgraph Support["🛠️ SUPPORT TOOLS"]
        Speed["3Speed"]
        Emite["eMite"]
        CEL["CEL Event Log"]
    end
    
    subgraph Analytics["📊 ANALYTICS"]
        IDF["IDF Dialer"]
        Indicate["Indicate Me"]
        Brilliant["Brilliant"]
        Calabrio["Calabrio WFM"]
    end
    
    Backend["🏢 Backend Systems"]
    
    Customer --> Phone
    Customer --> Web
    Customer --> Email
    
    Phone --> IVRA
    Web --> BFF
    Email --> BIF
    
    IVRA -->|READ customer data| CARE
    BFF -->|READ customer data| CARE
    BIF -->|READ customer data| CARE
    
    CARE <--> DB
    
    CARE -->|Log events| CEL
    Speed -->|Write test results| CARE
    CARE -->|Read metrics| Emite
    
    CARE <-->|Query services| Backend
    
    CARE -->|Send data| BIF
    BIF --> Email
    
    Phone -->|Recordings| IDF
    Phone -->|Audio| Indicate
    Phone -->|Events| Brilliant
    CARE -->|Agent data| Calabrio
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:6px,color:#fff
    style DB fill:#fa5252,stroke:#c92a2a,stroke-width:4px,color:#fff
    style Customer fill:#4dabf7,stroke:#1971c2,stroke-width:3px
    style IVRA fill:#51cf66,stroke:#2f9e44,stroke-width:2px
    style BFF fill:#51cf66,stroke:#2f9e44,stroke-width:2px
    style BIF fill:#51cf66,stroke:#2f9e44,stroke-width:2px
```

**System Functions:**

| System | Function | Dependencies |
|--------|----------|--------------|
| **CARE** | Customer profile, orders, subscriptions, history | Database |
| **Genesys Cloud** | Call routing, IVR, agent desktop, recordings | CARE APIs |
| **Boost.ai** | AI chatbot, 24/7 automated support | BFF layer |
| **IVRA/NCCP** | IVR to backend bridge, account validation | CARE APIs |
| **BFF-chatbot.se** | Chat data layer, customer lookup | CARE APIs |
| **BIF-Ticket** | Ticket creation handler  | CARE + Zendesk |
| **3Speed** | Network speed testing | CARE storage |
| **eMite** | Live metrics dashboard | CARE metrics API |
| **CEL** | Event logging for compliance | CARE events |
| **Calabrio** | Workforce management | CARE agent data |

---

### 2. Customer Service Agent Workflow

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
