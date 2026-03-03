# CARE Integration Overview - Simple Guide

## 📚 Complete Documentation

| Document | Description | Link |
|----------|-------------|------|
| **Integration Overview** | Quick visual guide to all integrations (You are here) | [README.md](README.md) |
| **Detailed Integration Map** | Complete integration documentation with patterns, endpoints, and workflows | [integration-details/](integration-details/) |
| **Developer Onboarding** | CARE backend onboarding guide for new developers | [developer-onboarding/](developer-onboarding/) |
| **Apps Catalog** | Complete catalog of all 21 contact center applications | [apps-catalog/](apps-catalog/) |

---

## 1. Individual App Workflows with CARE

### Genesys Cloud ↔ CARE
```mermaid
sequenceDiagram
    Customer->>Genesys: Calls support
    Genesys->>IVRA: Route to IVR
    IVRA->>CARE: GET /customer/lookup
    CARE-->>IVRA: Customer data
    IVRA->>Genesys: Validated
    Genesys->>Agent: Screen pop
    Agent->>CARE: GET /customer/full-profile
    CARE-->>Agent: Complete data
```
**Pattern:** Bidirectional REST API  
**Endpoints:** `GET /customer/lookup`, `GET /customer/full-profile`

---

### BFF-chatbot.se → CARE
```mermaid
sequenceDiagram
    Customer->>Boost.ai: Start chat
    Boost.ai->>BFF: Request customer data
    BFF->>CARE: GET /customer/{id}
    CARE-->>BFF: Customer profile
    BFF-->>Boost.ai: Formatted data
    Boost.ai-->>Customer: Personalized response
```
**Pattern:** BFF (Backend for Frontend)  
**Endpoints:** `GET /customer/{id}`, `GET /orders/{customerId}`

---

### Agent Workspace → CARE
```mermaid
sequenceDiagram
    Agent->>Workspace: Search customer
    Workspace->>CARE: GET /customer/search
    CARE-->>Workspace: Search results
    Agent->>Workspace: Select customer
    Workspace->>CARE: GET /customer/{id}
    CARE-->>Workspace: Full profile
    Agent->>Workspace: Update info
    Workspace->>CARE: PUT /customer/{id}
    CARE-->>Workspace: Updated
```
**Pattern:** Synchronous REST API  
**Endpoints:** `GET /customer/search`, `GET /customer/{id}`, `PUT /customer/{id}`, `POST /customer`

---

### IVRA/NCCP → CARE
```mermaid
sequenceDiagram
    IVR->>IVRA: Customer enters account
    IVRA->>CARE: GET /account/validate
    CARE-->>IVRA: Valid/Invalid
    IVRA->>CARE: GET /customer/balance
    CARE-->>IVRA: Balance info
    IVRA->>IVR: Speak to customer
```
**Pattern:** API Gateway  
**Endpoints:** `GET /account/validate`, `GET /customer/balance`, `GET /customer/{id}`

---

### CARE → Zendesk (via BIF-Ticket)
```mermaid
sequenceDiagram
    Agent->>CARE: Create ticket
    CARE->>BIF: POST /ticket/create
    BIF->>Zendesk: Format & create
    Zendesk-->>BIF: Ticket #12345
    BIF-->>CARE: Success
    CARE-->>Agent: Ticket created
```
**Pattern:** REST via Middleware  
**Endpoints:** `POST /ticket/create`

---

### 3Speed → CARE
```mermaid
sequenceDiagram
    Agent->>3Speed: Trigger test
    3Speed->>Customer: Run speed test
    Customer-->>3Speed: Results
    3Speed->>CARE: POST /speed-test/result
    CARE-->>3Speed: Saved
    CARE->>Agent: Display results
```
**Pattern:** Asynchronous REST  
**Endpoints:** `POST /speed-test/result`, `GET /speed-test/{testId}`

---

### CARE → CEL
```mermaid
sequenceDiagram
    Agent->>CARE: Customer interaction
    CARE->>CARE: Log interaction
    CARE->>CEL: POST /events
    CEL-->>CARE: Logged
```
**Pattern:** Event-driven (Async)  
**Endpoints:** `POST /events`

---

### eMite → CARE
```mermaid
sequenceDiagram
    loop Every 30 seconds
        eMite->>CARE: GET /metrics
        CARE-->>eMite: Current metrics
        eMite->>Dashboard: Update display
    end
```
**Pattern:** Polling REST API  
**Endpoints:** `GET /metrics`

---

### CARE → Calabrio
```mermaid
sequenceDiagram
    loop Daily at 2 AM
        CARE->>CARE: Collect agent data
        CARE->>Calabrio: Export agent metrics
        Calabrio-->>CARE: Received
    end
```
**Pattern:** Batch Export  
**Endpoints:** Daily data export

---

### CARE ↔ Backend Systems (via IVRA/NCCP)
```mermaid
sequenceDiagram
    CARE->>IVRA: Query billing
    IVRA->>Backend: GET /billing/status
    Backend-->>IVRA: Billing data
    IVRA-->>CARE: Formatted response
```
**Pattern:** Service Mesh via Gateway  
**Endpoints:** Various via IVRA/NCCP

---

## 2. Integration Patterns

| Pattern | Used By | Description |
|---------|---------|-------------|
| **REST API (Sync)** | Agent Workspace, IVRA/NCCP, BFF, eMite | Standard HTTP REST calls |
| **BFF** | BFF-chatbot.se | Backend For Frontend - specialized API layer |
| **API Gateway** | IVRA/NCCP | Single entry point to multiple backends |
| **Event-Driven** | CEL | Asynchronous event logging |
| **Batch Export** | Calabrio | Scheduled daily data export |
| **Polling** | eMite | Regular interval data fetching |
| **Service Mesh** | Backend Systems | Decoupled service communication via gateway |
| **REST via Middleware** | BIF-Ticket → Zendesk | Transformation layer between systems |

---

## 3. Integration Endpoints

### Customer Domain
```
GET    /willow/api/customer/{id}              - Get customer details
GET    /willow/api/customer/search            - Search customers
GET    /willow/api/customer/lookup            - Quick lookup (IVR)
GET    /willow/api/customer/full-profile      - Complete customer 360
POST   /willow/api/customer                   - Create customer
PUT    /willow/api/customer/{id}              - Update customer
DELETE /willow/api/customer/{id}              - Delete customer
```

### Account Domain
```
GET    /willow/api/account/{id}               - Get account info
GET    /willow/api/account/validate           - Validate account (IVR)
GET    /willow/api/account/balance            - Get balance
PUT    /willow/api/account/{id}               - Update account
```

### Orders Domain
```
GET    /willow/api/orders/{customerId}        - Get customer orders
GET    /willow/api/orders/{orderId}           - Get specific order
POST   /willow/api/orders                     - Create order
PUT    /willow/api/orders/{orderId}           - Update order status
```

### Communication Domain
```
POST   /willow/api/communication/log          - Log interaction
GET    /willow/api/communication/{customerId} - Get communication history
```

### Speed Testing Domain
```
POST   /willow/api/speed/test                 - Initiate speed test
POST   /willow/api/speed-test/result          - Store test results
GET    /willow/api/speed/results/{testId}     - Get test results
```

### Ticketing Domain
```
POST   /willow/api/ticket/create              - Create support ticket
GET    /willow/api/ticket/{ticketId}          - Get ticket status
PUT    /willow/api/ticket/{ticketId}          - Update ticket
```

### Metrics Domain
```
GET    /willow/api/metrics                    - Get current metrics (eMite)
GET    /willow/api/metrics/agent              - Get agent metrics
```

### Feature Toggles Domain
```
GET    /willow/api/featuretoggles             - Get enabled features
POST   /willow/api/featuretoggles             - Update feature state
```

---

## 4. Quick Reference - All Integrations

| System | Pattern | Direction | Priority | Key Endpoints |
|--------|---------|-----------|----------|---------------|
| **Genesys Cloud** | Bidirectional REST | BOTH | 🔴 CRITICAL | `/customer/lookup`, `/customer/full-profile` |
| **IVRA/NCCP** | API Gateway | INBOUND | 🟠 HIGH | `/account/validate`, `/customer/lookup` |
| **BFF-chatbot.se** | BFF | INBOUND | 🟠 HIGH | `/customer/{id}`, `/orders/{customerId}` |
| **Agent Workspace** | REST API | INBOUND | 🟠 HIGH | All CRUD endpoints |
| **Boost.ai** | Via BFF | INBOUND | 🟠 HIGH | Via BFF-chatbot.se |
| **BIF-Ticket** | REST Middleware | OUTBOUND | 🟠 HIGH | `/ticket/create` |
| **Zendesk** | Via BIF | OUTBOUND | 🟠 HIGH | Via BIF-Ticket |
| **3Speed** | Async REST | INBOUND | 🟡 MEDIUM | `/speed-test/result` |
| **eMite** | Polling REST | INBOUND | 🟡 MEDIUM | `/metrics` |
| **CEL** | Event-driven | OUTBOUND | 🟡 MEDIUM | `/events` |
| **Backend Systems** | Service Mesh | BOTH | 🟠 HIGH | Via IVRA/NCCP |
| **Calabrio** | Batch Export | OUTBOUND | 🟢 ASYNC | Daily export |
| **IDF Dialer** | Indirect | OUTBOUND | 🟢 ASYNC | Via Genesys |
| **Indicate Me** | Indirect | OUTBOUND | 🟢 ASYNC | Via Genesys |

---

## 5. Simple Architecture Diagram

```mermaid
graph LR
    subgraph INBOUND["📥 CALL CARE"]
        A1[Agent Workspace]
        A2[BFF-chatbot.se]
        A3[IVRA/NCCP]
        A4[eMite]
        A5[3Speed]
    end
    
    CARE["🎯 CARE"]
    DB[(Database)]
    
    subgraph OUTBOUND["📤 CARE CALLS"]
        B1[CEL]
        B2[BIF-Ticket]
        B3[Zendesk]
        B4[Backend Systems]
        B5[Calabrio]
    end
    
    A1 --> CARE
    A2 --> CARE
    A3 --> CARE
    A4 --> CARE
    A5 --> CARE
    
    CARE <--> DB
    
    CARE --> B1
    CARE --> B2
    B2 --> B3
    CARE <--> B4
    CARE --> B5
    
    style CARE fill:#ff6b6b,stroke:#c92a2a,stroke-width:4px,color:#fff
    style DB fill:#fa5252,stroke:#c92a2a,stroke-width:3px,color:#fff
```

---

**Document Version:** Simple 1.0  
**Last Updated:** March 3, 2026
