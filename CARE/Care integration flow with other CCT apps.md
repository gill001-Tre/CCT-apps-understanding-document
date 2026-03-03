# CARE Integration Overview - Simple Guide

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
**What happens:** When a customer calls, the IVR system asks CARE to look up their account. CARE finds the customer and sends back their info. When the call reaches an agent, their screen automatically shows the customer's full profile from CARE, so the agent knows who they're talking to and can help immediately.

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
**What happens:** When a customer starts chatting on the website, the chatbot asks the BFF layer to get customer information. BFF requests the customer's profile and order history from CARE, formats it nicely for the chatbot, and the chatbot uses this to give personalized answers like "Hi Anna, I see your order #12345 is arriving tomorrow."

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
**What happens:** An agent types a customer name or phone number to search. CARE gets customer data from CRM PeopleSoft (through Karma) and shows matching customers. The agent clicks on the right customer, and CARE displays their complete profile. If the agent needs to update something (like a new email address), they make the change and CARE sends the update to PeopleSoft through XML.

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
**What happens:** When a customer calls and enters their account number using the phone keypad, the IVR system checks with CARE if it's a valid account. If it is, CARE sends back the account balance and the IVR reads it out loud to the customer. This all happens automatically without needing an agent.

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
**What happens:** When an agent needs to create a support ticket for a customer issue, they click "create ticket" in CARE. CARE sends the customer information to BIF-Ticket, which formats it properly and creates the ticket in Zendesk. Zendesk gives back a ticket number (like #12345), and the agent can see it was successfully created.

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
**What happens:** When a customer complains about slow internet, the agent clicks a button to run a speed test. 3Speed tests the customer's connection speed and gets the results. It then saves these results to CARE so they become part of the customer's history. The agent can immediately see if the internet is actually slow or working normally.

---

### CARE → CEL
```mermaid
sequenceDiagram
    Agent->>CARE: Customer interaction
    CARE->>CARE: Log interaction
    CARE->>CEL: POST /events
    CEL-->>CARE: Logged
```
**What happens:** Every time an agent talks to a customer (phone call, chat, email), CARE automatically records what happened. It sends this information to CEL (Communication Event Log), which keeps a permanent record. This is important for compliance and so anyone can look back and see the complete history of customer interactions.

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
**What happens:** Every 30 seconds, the eMite dashboard checks in with CARE asking "how are things going?" CARE responds with current statistics like how many agents are working, how many customers are being helped, and system response times. eMite displays this on big screens so managers can monitor the contact center in real-time.

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
**What happens:** Every night at 2 AM when call volume is low, CARE packages up all the agent activity data from the day (how many calls handled, talk time, etc.) and sends it to Calabrio. Calabrio uses this data to help managers plan how many agents to schedule for different times and days based on patterns.

---

### CARE ↔ Backend Systems (via IVRA/NCCP)
```mermaid
sequenceDiagram
    CARE->>IVRA: Query billing
    IVRA->>Backend: GET /billing/status
    Backend-->>IVRA: Billing data
    IVRA-->>CARE: Formatted response
```
**What happens:** When CARE needs information from other business systems (like billing, orders, or service provisioning), it asks IVRA/NCCP to get it. IVRA acts like a translator and messenger, fetching the data from the right backend system and bringing it back to CARE in a format that CARE understands. This way CARE doesn't need to know about every single backend system.
