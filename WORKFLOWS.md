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

### Workflow 7: Supplier Onboarding & AI KYC Verification

```mermaid
sequenceDiagram
    autonumber
    actor Applicant as New Supplier
    actor Admin as Compliance Admin
    participant App as BidSync Web App
    participant Gemini as Google Gemini Multimodal AI
    participant Platform as BidSync Core Platform
    participant DB as Persistence Layer

    Applicant->>App: Submit Registration & Attach Business Certificate
    Platform->>Gemini: Send Document for Multimodal Inspection
    Gemini->>Gemini: OCR & Semantic Legal Entity Extraction
    Gemini-->>Platform: Return Extracted Data, Validity & Recommendation
    Platform->>Platform: Cross-Validate Ground Truth with Applicant Form
    Platform->>DB: Save Applicant with AI Compliance Dossier

    Admin->>App: Open Compliance Dashboard & Review Dossier
    Admin->>App: Review AI Findings & Original Document Side-by-Side
    Admin->>Platform: Approve Applicant & Grant Verified Supplier Badge
    Platform->>DB: Update Supplier Status to Verified
    Platform-->>Applicant: Welcome Alert: Account Verified
```

---

<p align="center">
  <strong>BidSync Procurement Workflows & Lifecycle Diagrams</strong><br/>
  Authored by Monith Kumarasinghe • <a href="https://bidsync.online">bidsync.online</a>
</p>
