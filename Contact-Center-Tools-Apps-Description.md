# CARE Ecosystem - Applications Catalog

This document provides a comprehensive overview of all applications and services in the CARE ecosystem, including their purpose, functionality, and technology stack.

---

## Table of Contents
1. [Java Applications](#java-applications)
2. [SaaS Platforms](#saas-platforms)
3. [Application Details](#application-details)

---

## Java Applications

These are custom-built applications developed and maintained internally.

## SaaS Platforms

These are third-party Software-as-a-Service platforms that we integrate with.

---

## Application Details

| # | App Name | Type | Description | Function | Purpose & Real-World Example |
|---|---|---|---|---|---|
| 1 | **3Speed** | Java | REST API that measures network speed | • Checks network connection speed through third-party API<br>• Customer service agents can view information in CARE portal | Runs speed tests of network connections and automatically updates CARE, supporting customer service agents in helping customers with connection issues.<br><br>**Example:** Agent sees customer complaining about slow internet → checks 3Speed results in CARE → sees connection is actually running at 10% of expected speed → escalates to technical team |
| 2 | **BFF-chatbot.se** | Java | Backend For Frontend (BFF) API for Swedish chatbot | Fetches customer information (name, account details, order history) when customer uses chatbot, enabling personalized responses | When customer chats → chatbot asks BFF → BFF gets customer data → chatbot responds with personalized information.<br><br>**Example:** "Hi Anna Svensson, I see you have an issue with order #12345. Let me help you with that." |
| 3 | **BIF-Ticket** | Java | Handles Zendesk forms functionality - ticket creation and form field management | • Displays correct form fields<br>• Handles related fields (dropdown options that change based on previous selections)<br>• Creates tickets in Zendesk | Manages the support form submission process.<br><br>**Example Flow:**<br>1. Select "Internet problem" from dropdown<br>2. New fields appear: "Connection type: Fiber/DSL/Mobile"<br>3. Fill form and submit<br>4. BIF-Ticket creates proper ticket in Zendesk system |
| 4 | **Boost.ai** | SaaS | Platform for conversational AI / chatbot for customer service | • Answers customer questions automatically<br>• Handles conversations in natural language<br>• Integrates with other systems (hallon and 3SE)<br>• Works as customer-facing widget AND backend tool for admins/trainers | Provides AI-powered chatbot capabilities.<br><br>**Example:**<br>• Customer: "I need help with my internet"<br>• Boost.ai: "I can help! What seems to be the issue?"<br>• Customer: "It's slow"<br>• Boost.ai: "Let me check..." (calls 3Speed API) → "Your speed is below normal. Would you like me to create a support ticket?" |
| 4a | **Boost.ai - Instance - 3SE** (Trefin) | SaaS | Specific Boost.ai instance configured for 3SE brand | • Customized for 3SE customers only<br>• Has 3SE-specific training data and responses<br>• Uses 3SE branding and tone of voice<br>• Connects to 3SE customer data | When customer visits 3SE website → sees 3SE-branded chatbot → this instance handles the conversation |
| 4b | **Boost.ai - Instance - hallon** (Berry) | SaaS | Specific Boost.ai instance configured for hallon brand | • Customized for hallon customers only<br>• Has hallon-specific training data and responses<br>• Uses hallon branding<br>• Connects to hallon customer data | When customer visits hallon.se → sees hallon-branded chatbot → this instance handles the conversation |
| 5 | **Brilliant** | Java | Sends transaction surveys to customers after phone call interactions with customer service | • Triggers automatically after support call ends<br>• Sends survey to customer (via Genesys Cloud or Outbound by Enreach)<br>• Collects feedback for quality monitoring | Gathers customer satisfaction feedback.<br><br>**Example:**<br>1. Customer calls support about internet issue<br>2. Agent helps and closes call<br>3. Brilliant detects call ended<br>4. Sends survey: "On scale of 1-10, how satisfied were you?"<br>5. Customer responds<br>6. Data goes to analytics/reporting |
| 6 | **Calabrio** | SaaS (AWS-hosted) | Workforce Management (WFM) system with scheduling, forecasting, adherence tracking, and time management for contact center staff | • Scheduling: Creates agent work schedules<br>• Forecasting: Predicts call volume and staffing needs<br>• Adherence: Tracks if agents follow schedules<br>• Time management: Tracks hours, breaks, attendance | Manages contact center operations and workforce optimization.<br><br>**Example:**<br>• Calabrio predicts: "Next Monday 500 calls expected between 9-11 AM"<br>• Manager schedules 20 agents for that time<br>• During day: alerts "Agent Anna scheduled but not logged in"<br>• End of week: generates report of all agent hours worked |
| 7 | **CARE - Customer Administration** | Java | Manages customer information and data for customer service agents - portal for agents | • Stores customer personal details<br>• Account information<br>• Service subscriptions<br>• Contact history<br>• Account changes<br>**Note:** On-call support handled by Customer Management Team during off-business hours | Main database/system containing all customer information.<br><br>**Example:**<br>• Agent searches "Anna Svensson"<br>• CARE returns: address, phone, active services, billing info<br>• Agent can update customer details<br>• All other systems pull customer data from here |
| 8 | **CEL - Communication Event Log** | Java | Logs all incoming and outgoing communication to and from H3G | • Records incoming calls, emails, chats<br>• Logs outgoing messages, notifications<br>• Captures timestamps and details<br>• Maintains audit trail for compliance | Complete history book of all communication.<br><br>**Example:**<br>• Customer sends email at 10:00 AM → CEL logs it<br>• Agent calls customer back at 2:00 PM → CEL logs it<br>• System sends SMS confirmation at 3:00 PM → CEL logs it<br>• Later: search CEL to see entire communication timeline |
| 9 | **Dialer Survey** | Java | Automatically sends SMS surveys to customers after outbound calls from Enreach platform, based on rules and user consent | • Monitors outbound calls made by agents<br>• Checks consent and rules<br>• Triggers SMS survey via Brilliant system<br>• Runs on scheduled jobs (automated, no manual intervention)<br>• No user interface (background process) | Automated trigger for outbound call surveys.<br><br>**Example:**<br>1. Agent calls customer about order<br>2. Call ends<br>3. Dialer Survey checks: "Does customer allow surveys? Was call long enough?"<br>4. If yes → tells Brilliant to send SMS: "Rate your experience 1-5"<br>5. Customer receives SMS survey |
| 10 | **eMite** | Java | Real-time business dashboard showing contact center traffic for Genesys Cloud | • Displays live call queue counts<br>• Shows agents available/busy<br>• Shows average wait time<br>• Displays call volume trends | Live metrics dashboard for call center (like airport departure boards).<br><br>**Example:** Big screen shows:<br>• "15 calls waiting"<br>• "8 agents available"<br>• "Average wait: 3 minutes"<br>Manager sees this → asks more agents to log in |
| 10a | **eMite - Instance - 3DK** | Java | Specific eMite dashboard instance configured for 3DK (Three Denmark brand) | Same as eMite but for Danish operations | Shows live metrics for 3DK contact center |
| 10b | **eMite - Instance - 3SE** | Java | Specific eMite dashboard instance configured for 3SE (Tre Sweden brand) | Same as eMite but for Swedish operations | Shows live metrics for 3SE contact center |
| 11 | **Exstream Contract** | Java | Generates PDF contracts for retail sales process (Försäljarten) and PDF Contract Summaries | • Creates formal contract documents<br>• Generates sales contracts in PDF format<br>• Produces contract summaries<br>• Used specifically in retail sales workflows | Automated contract generation.<br><br>**Example:**<br>1. Sales agent completes order for new customer<br>2. Exstream generates PDF with all terms, pricing, customer details<br>3. Customer receives and signs contract<br>4. Summary document created for internal records |
| 12 | **Genesys Cloud** | SaaS | Complete contact center solution that supports customer service processes (previously called PureCloud, renamed by vendor in 2020) | • IVR (Interactive Voice Response) - automated phone menus<br>• ACD (Automatic Call Distribution) - routes calls to right agents<br>• Inbound calls<br>• Outbound campaigns<br>• Chat, email, social media channels | Main phone system for entire contact center. Handles all customer calls - routes, queues, connects to agents.<br><br>**Example:** Customer calls → Genesys Cloud answers → plays IVR menu → customer presses option → routes to appropriate agent queue |
| 12a | **Genesys Cloud - Instance - 3DK** | SaaS | Specific Genesys Cloud instance configured for 3DK (Three Denmark) operations | Same capabilities as Genesys Cloud but configured for Danish market | Handles all customer calls for 3DK brand |
| 12b | **Genesys Cloud - Instance - 3SE** | SaaS | Specific Genesys Cloud instance configured for 3SE (Tre Sweden) operations | Same capabilities as Genesys Cloud but configured for Swedish market | Handles all customer calls for 3SE brand |
| 12c | **Genesys Cloud - Instance - QIST** | SaaS | Genesys Cloud instance for QIST environment (Quality/Integration/System Testing) | • Testing environment for Genesys Cloud<br>• Tests new features/configurations before production<br>• Allows experimentation without affecting live customers | Safe testing environment.<br><br>**Example:**<br>• Developer wants new IVR menu option<br>• Tests in QIST instance first<br>• Confirms it works<br>• Then deploys to production |
| 12d | **Genesys Cloud - Instance - Test** | SaaS | Genesys Cloud instance for Test environment | Same as QIST - for testing purposes | Additional test environment for Genesys Cloud configurations |
| 13 | **IDF Dialer - IVR Data Fetcher** | Java | Fetches conversations from PureCloud and feeds information to SAS DB | • Pulls conversation data from Genesys Cloud (PureCloud)<br>• Sends data to SAS Database for analytics | Data pipeline for call analytics.<br><br>**Example:**<br>• Customers interact with phone system throughout day<br>• IDF Dialer collects all conversation records<br>• Sends to SAS database<br>• Business analysts run reports on call patterns, volumes, trends |
| 14 | **Indicate Me** | SaaS | Access to Speech-to-Text transcriptions and AI insights from Genesys Cloud calls - coaching and analysis tool | • Converts voice calls to text (speech-to-text)<br>• Analyzes call conversations using AI<br>• Provides insights for coaching agents<br>• Identifies patterns, customer issues, agent performance | AI-powered call analysis for quality and coaching.<br><br>**Example:**<br>• Customer call is recorded in Genesys Cloud<br>• Indicate Me transcribes: "Customer complained about slow internet 3 times"<br>• AI spots pattern: "Agent didn't offer troubleshooting steps"<br>• Manager gets insight: "Coach this agent on problem-solving" |
| 15 | **IVRA - Interactive Voice Response API** | Java | Provides endpoints and integrations towards IT backend systems to serve the PureCloud IVR | • Creates APIs that IVR can call<br>• Connects IVR (phone menu system) to backend systems<br>• Allows IVR to get real-time data during customer calls<br>• Bridges Genesys Cloud IVR with internal IT systems | Enables IVR to access customer data and backend systems.<br><br>**Example:**<br>• Customer calls and enters account number via phone keypad<br>• IVR needs to check if account is valid<br>• IVR calls IVRA API endpoint<br>• IVRA fetches data from backend system<br>• Returns: "Yes, account exists, balance is $50"<br>• IVR speaks: "Your current balance is fifty dollars" |
| 16 | **NCCP - New Contact Center Platform** | Java | Provides endpoints and integrations towards IT backend systems to serve the PureCloud IVR | • Similar to IVRA<br>• Provides APIs for contact center integrations<br>• Connects Genesys Cloud to backend IT systems<br>• Serves PureCloud IVR with data and functionality | Modernized contact center integration infrastructure that connects phone system to business systems.<br><br>**Example:** When IVR or agents need customer data, order information, or need to trigger actions (like creating a ticket), NCCP provides the APIs and integration points |
| 17 | **Outbound by Enreach** | SaaS | Outbound dialing to customers and/or prospects based on call campaigns by telesales teams | • Manages outbound calling campaigns<br>• Dials customers automatically for sales teams<br>• Handles prospect lists<br>• Supports telesales operations | Automated dialing for sales campaigns.<br><br>**Example:**<br>1. Sales team campaign: "Call 1000 customers about new fiber internet offer"<br>2. Upload customer list to Outbound by Enreach<br>3. System automatically dials numbers<br>4. When customer answers, connects to sales agent<br>5. Agent pitches the offer |
| 17a | **Outbound by Enreach - Instance** | SaaS | Additional instance of Outbound by Enreach platform | Same outbound calling capabilities | Separate instance for different campaigns or markets |
| 18 | **Robotic Process Automation (RPA)** | RPA | Software technology that builds, deploys, and manages software robots that emulate human actions interacting with digital systems | • Automates repetitive manual tasks<br>• Software bots perform actions like humans would<br>• Clicks buttons, fills forms, copies data between systems<br>• Works 24/7 without human intervention | Software robots that handle repetitive tasks automatically.<br><br>**Example:**<br>• Manual task: Agent copies customer info from System A to System B (100 times/day)<br>• RPA bot: Automatically reads data from System A, opens System B, fills form, clicks submit<br>• Saves hours of repetitive work |
| 19 | **Test Data Generator (TDG)** | Java | Test Data Generator | • Creates fake/dummy test data<br>• Generates realistic customer records, orders, phone numbers for testing<br>• Ensures test data is realistic but not real customer data<br>• Used by developers and QA teams | Creates test data for development and QA purposes |
| 20 | **Test Data Service (TDS)** | Java | Component needed to fulfill automation of tests - serves test data reliably | • Provides test data on-demand for automated tests<br>• Serves data to test automation frameworks<br>• Ensures consistent, reliable test data availability<br>• Manages test data lifecycle | While TDG creates test data, TDS stores and delivers it when tests run.<br><br>**Example:**<br>1. Automated test runs: "I need customer with active subscription"<br>2. Test calls TDS API<br>3. TDS returns: customer ID, name, subscription details<br>4. Test uses that data<br>5. After test completes, TDS may clean up or reset data |
| 21 | **Zendesk** | SaaS | Provides customer support over email, ticketing, and other channels (chat, social) with integrations from other systems | • Customer support ticketing system<br>• Email support<br>• Live chat<br>• Social media support integration<br>• Help center management<br>• Ticket tracking and case management | Where customer support tickets are created, tracked, and resolved.<br><br>**Example:**<br>1. Customer emails support: "My internet is down"<br>2. Zendesk creates ticket #12345<br>3. Agent picks up ticket, investigates<br>4. Agent responds via Zendesk<br>5. Customer and agent communicate back and forth<br>6. Issue resolved, ticket closed |
| 21a | **Zendesk - Instance - 3DK** | SaaS | Specific Zendesk instance configured for 3DK (Three Denmark) operations | Same ticketing capabilities but for Danish market | Handles support tickets for 3DK customers |
| 21b | **Zendesk - Instance - 3SE** | SaaS | Specific Zendesk instance configured for 3SE (Three Sweden) operations | Same ticketing capabilities but for Swedish market | Handles support tickets for 3SE customers |
| 21c | **Zendesk - Instance - Hallon** | SaaS | Specific Zendesk instance configured for hallon brand operations | Same ticketing capabilities but for hallon brand | Handles support tickets for hallon customers |
| 21d | **Zendesk - Instance - QISTER/Flexi** | SaaS | Zendesk instance configured for QISTER/Flexi environment | • Could be for testing Zendesk integrations<br>• Or for specific product line/service called Flexi<br>• Separate workspace from production brands | Testing or specialized service environment |

---

## Quick Reference by Type

### Java Applications (Custom-Built)
- 3Speed
- BFF-chatbot.se
- BIF-Ticket
- Brilliant
- CARE - Customer Administration
- CEL - Communication Event Log
- Dialer Survey
- eMite (+ instances)
- Exstream Contract
- IDF Dialer
- IVRA
- NCCP
- Test Data Generator (TDG)
- Test Data Service (TDS)

### SaaS Platforms (Third-Party)
- Boost.ai (+ instances)
- Calabrio
- Genesys Cloud (+ instances)
- Indicate Me
- Outbound by Enreach (+ instances)
- Zendesk (+ instances)

### Other Technologies
- Robotic Process Automation (RPA)

---

## Integration Overview

```
Customer Interaction Flow:
┌─────────────┐
│  Customer   │
└──────┬──────┘
       │
       ├─► Genesys Cloud (Phone) ──► IVRA/NCCP ──► Backend Systems
       │
       ├─► Boost.ai (Chat) ──► BFF-chatbot.se ──► CARE
       │
       └─► Zendesk (Email/Ticket) ──► BIF-Ticket ──► Backend Systems

Post-Interaction Flow:
Call Ends ──► Brilliant/Dialer Survey ──► Customer Survey ──► Analytics

Data & Analytics Flow:
Genesys Cloud ──► IDF Dialer ──► SAS DB ──► Reports
Genesys Cloud ──► Indicate Me ──► AI Insights ──► Coaching

Agent Support Flow:
Agent Workspace ──► CARE ──► Customer Data
               └──► eMite ──► Live Metrics
               └──► 3Speed ──► Network Diagnostics
```

---

## Glossary

- **BFF**: Backend For Frontend - API layer specifically designed to serve a particular frontend application
- **IVR**: Interactive Voice Response - Automated phone menu system
- **ACD**: Automatic Call Distribution - Routes calls to appropriate agents
- **WFM**: Workforce Management - System for scheduling and managing contact center staff
- **RPA**: Robotic Process Automation - Software robots that automate repetitive tasks
- **SaaS**: Software as a Service - Third-party hosted applications
- **3SE**: Tre Sweden (Three Sweden brand)
- **3DK**: Three Denmark brand

---

**Document Version:** 1.0  
**Last Updated:** March 2, 2026  
**Maintained By:** CARE Backend Team
