# 🔄 BidSync: Procurement Workflows & Lifecycle Diagrams

This document illustrates the operational workflows governing the procurement lifecycle on the [BidSync Platform](https://bidsync.online).

---

## 📑 Workflow Index

1. [Workflow 1: Tender Creation & BOQ Definition](#workflow-1-tender-creation--boq-definition)
2. [Workflow 2: Sealed Bid Submission](#workflow-2-sealed-bid-submission)
3. [Workflow 3: Automated Tender Unsealing Ceremony](#workflow-3-automated-tender-unsealing-ceremony)
4. [Workflow 4: AI Comparative Evaluation & Award](#workflow-4-ai-comparative-evaluation--award)
5. [Workflow 5: Delivery & Goods Received Notes (GRN)](#workflow-5-delivery--goods-received-notes-grn)
6. [Workflow 6: Commercial Dispute Resolution](#workflow-6-commercial-dispute-resolution)
7. [Workflow 7: Supplier Onboarding & AI KYC Verification](#workflow-7-supplier-onboarding--ai-kyc-verification)
8. [Workflow 8: Public AI Support & Trade Inquiries (Zero-Leakage)](#workflow-8-public-ai-support--trade-inquiries-zero-leakage)

---

### Workflow 1: Tender Creation & BOQ Definition

```mermaid
sequenceDiagram
    autonumber
    actor Buyer
    participant App as BidSync Web App
    participant Platform as BidSync Core Platform
    participant DB as Persistence Layer

    Buyer->>App: Select Category & Load Industry Template
    Buyer->>App: Enter Requirements & Use AI Scope Assistant
    App->>Platform: Generate Structured Procurement Scope
    Platform-->>App: Return Comprehensive Specifications & Standards
    Buyer->>App: Define BOQ Line Items (Quantities, Units, Specs)
    Buyer->>App: Publish Sealed Tender
    Platform->>Platform: Generate Tender Cryptographic Keypair
    Platform->>DB: Store Tender Specifications & Key Escrow
    Platform-->>Buyer: Confirm Tender Publication (Live Status)
```

---

### Workflow 2: Sealed Bid Submission

```mermaid
sequenceDiagram
    autonumber
    actor Vendor
    participant App as BidSync Web App
    participant Platform as BidSync Core Platform
    participant DB as Persistence Layer

    Vendor->>App: Browse Open Tenders & Inspect BOQ Requirements
    Vendor->>App: Enter Line-Item Unit Prices, Taxes & Delivery Days
    Vendor->>App: Submit Sealed Bid
    Platform->>Platform: Cryptographically Encrypt Financial Proposal
    Platform->>Platform: Generate SHA-256 Digital Fingerprint
    Platform->>DB: Store Encrypted Proposal & Receipt
    Platform-->>Vendor: Return Cryptographic Bid Submission Receipt
    Platform-->>App: Broadcast Updated Bid Count (Privacy-Preserving)
```

---

### Workflow 3: Automated Tender Unsealing Ceremony

```mermaid
sequenceDiagram
    autonumber
    participant Scheduler as Automated Background Scheduler
    participant Platform as BidSync Core Platform
    participant DB as Persistence Layer
    actor Buyer
    actor Vendors

    Scheduler->>Scheduler: Continuous Deadline Check
    Scheduler->>DB: Detect Expired Tender Deadline
    Scheduler->>Platform: Initiate Unsealing Ceremony
    Platform->>DB: Retrieve Tender Private Key from Escrow
    loop For Every Submitted Bid
        Platform->>Platform: Decrypt Financial Numbers & Proposals
        Platform->>DB: Update Bid Status to Unsealed
    end
    Platform->>DB: Record Tamper-Evident Audit Event
    Platform-->>Buyer: Real-Time Live Notification: Tender Unsealed
    Platform-->>Vendors: Real-Time Live Notification: Tender Unsealed
```

---

### Workflow 4: AI Comparative Evaluation & Award

```mermaid
sequenceDiagram
    autonumber
    actor Buyer
    participant App as BidSync Web App
    participant AI as AI Statistical Engine
    participant Platform as BidSync Core Platform
    participant DB as Persistence Layer

    Buyer->>App: Open Tender Evaluation Console
    App->>AI: Request Comparative Analysis
    AI->>AI: Evaluate Price Distributions & Flag Outliers / Dumping
    AI->>AI: Calculate Multi-Criteria Composite Scorecards
    AI-->>App: Return Comparative Statements & Best-Value Ranking
    Buyer->>App: Select Winning Contractor & Issue Award
    Platform->>DB: Set Tender Status to Awarded
    Platform->>DB: Instantiate Contract & Delivery Milestones
    Platform-->>Buyer: Contract Initialized Successfully
```

---

### Workflow 5: Delivery & Goods Received Notes (GRN)

```mermaid
sequenceDiagram
    autonumber
    actor Vendor
    actor Buyer
    participant App as BidSync Web App
    participant Platform as BidSync Core Platform
    participant DB as Persistence Layer

    Note over Vendor, Platform: Step 1: Deliverable Submission
    Vendor->>App: Enter Dispatch Waybill & Proof of Delivery
    Vendor->>App: Submit Milestone Completion
    Platform->>DB: Update Milestone Status to Delivered
    Platform-->>Buyer: Alert: Delivery Submitted for Inspection

    Note over Buyer, Platform: Step 2: Inspection & GRN Issuance
    Buyer->>App: Perform Physical Inspection & Record Remarks
    Buyer->>App: Approve Milestone & Issue GRN
    Platform->>DB: Mark Milestone Approved & Generate Official GRN
    Platform-->>App: Enable Vector GRN PDF Certificate Download
```

---

### Workflow 6: Commercial Dispute Resolution

```mermaid
sequenceDiagram
    autonumber
    actor Claimant as Initiating Party
    actor Respondent as Counterparty
    actor Admin as Platform Arbitrator
    participant Platform as BidSync Core Platform
    participant DB as Persistence Layer

    Claimant->>Platform: File Commercial Grievance & Upload Evidence
    Platform->>DB: Mark Contract Milestone Disputed
    Platform-->>Respondent: Notify of Commercial Claim
    
    loop Negotiation Room
        Claimant->>Platform: Post Evidentiary Memo & Updates
        Respondent->>Platform: Post Response & Replacement Tracking
    end

    alt Escalated for Binding Arbitration
        Claimant->>Platform: Request Administrative Escalation
        Admin->>Platform: Review Case Files & Render Formal Ruling
        Platform->>DB: Resolve Dispute & Update Contract Status
        Platform-->>Claimant: Formal Resolution Issued
        Platform-->>Respondent: Formal Resolution Issued
    end
```

---

### Workflow 7: Supplier Onboarding, AI KYC & Administrative Governance

```mermaid
sequenceDiagram
    autonumber
    actor Applicant as New Supplier
    actor Admin as Compliance Admin
    participant App as BidSync Web App
    participant Platform as BidSync Core Platform
    participant Gemini as Google Gemini Multimodal AI
    participant DB as Persistence Layer

    Note over Applicant, DB: Phase 1: Non-Blocking Registration (< 1s Response)
    Applicant->>App: Submit Signup & Attach Business Certificate
    App->>Platform: POST /api/auth/signup (with multipart file)
    Platform->>Platform: Persist Certificate to Storage Vault
    Platform->>DB: Save User (approvalStatus: 'pending', isEmailVerified: false)
    Platform-->>App: Registration Success (< 1s Latency)
    App-->>Applicant: Display Activation Prompt ("Check Your Inbox")

    Note over Platform, DB: Phase 2: Asynchronous AI KYC Verification
    par Asynchronous Processing
        Platform->>Platform: Deliver Verification Link via Resend REST API
    and Background Multimodal Inspection
        Platform->>Gemini: Inspect Certificate Buffer (Multimodal Flash)
        Gemini->>Gemini: Extract Legal Name, BRN/TIN & Validity
        Gemini-->>Platform: Structured Compliance Recommendations
        Platform->>DB: Update User record with AI Verification Dossier
    end

    Note over Admin, DB: Phase 3: Administrative Governance Lifecycle
    Admin->>App: Open Compliance Desk (pendingApprovals badge: 1)
    Admin->>App: Review AI Findings & Original Document Side-by-Side

    alt Approved
        Admin->>Platform: PUT /api/admin/users/:id/approve
        Platform->>DB: Set approvalStatus = 'approved', isApproved = true
        Platform->>DB: Record Tamper-Evident Audit Entry
        Platform-->>Admin: Decrement pendingApprovals Counter
        Platform-->>Applicant: Notify: Account Verified & Supplier Badge Granted
    else Rejected
        Admin->>App: Click Reject & Enter Mandatory Rejection Reason
        Admin->>Platform: PUT /api/admin/users/:id/reject { reason }
        Platform->>DB: Set approvalStatus = 'rejected', rejectionReason = reason
        Platform->>DB: Record Tamper-Evident Audit Entry
        Platform-->>Admin: Decrement pendingApprovals Counter & Move to Rejected Accounts Desk
        Note over Admin, DB: Later Re-Approval (Optional)
        Admin->>Platform: Click Re-Approve from Rejected Desk
        Platform->>DB: Clear rejectionReason & Set approvalStatus = 'approved'
        Platform-->>Applicant: Notify: Account Re-Approved
    end
```

---

### Workflow 8: Public AI Support & Trade Inquiries (Zero-Leakage)

```mermaid
sequenceDiagram
    autonumber
    actor Visitor as Visitor / Guest / Prospective Vendor (No Auth Required)
    participant Widget as Floating Neo-Brutalist Chatbot
    participant Gateway as Express Gateway (/api/chatbot)
    participant DB as MongoDB Atlas Cluster
    participant Sanitizer as Zero-Leakage Sanitizer
    participant Gemini as Google Gemini Free AI Engine

    Visitor->>Widget: Ask question ("What vehicle spare parts tenders are open?")
    Widget->>Gateway: POST /api/chatbot/message { message }
    Gateway->>DB: Query Active Public RFQs (status: 'open', isPrivate: false)
    DB-->>Gateway: Active Tender Records

    Gateway->>Sanitizer: Strip Bids, Internal Estimates, Cryptographic Keys & Private Budgets
    Sanitizer-->>Gateway: Sanitized Public Trade Catalog

    alt Gemini API Operational
        Gateway->>Gemini: Prompt with User Message + Sanitized Catalog Context
        Gemini-->>Gateway: Context-Grounded Answer with Tender Highlights
    else API Quota Exceeded / Unavailable
        Gateway->>Gateway: Execute Offline Semantic Keyword Matching Engine
        Gateway-->>Gateway: Matched Public Tender Summary
    end

    Gateway-->>Widget: Natural Language Response + Safe Markdown Links
    Widget-->>Visitor: Display Answer with [View Tender](/tenders) Links
```

---

<p align="center">
  <strong>BidSync Procurement Workflows & Lifecycle Diagrams</strong><br/>
  Authored by Monith Kumarasinghe • <a href="https://bidsync.online">bidsync.online</a>
</p>
