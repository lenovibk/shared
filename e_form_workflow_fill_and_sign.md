# eForm Workflow – Fill Data then Sign

## Overview
This document explains how the eForm system handles **multiple recipients**, ensuring that:
- Data is filled **once and only once**
- PDF is generated **after data completion**
- Signatures happen **in the correct order**

This approach avoids data conflicts and is suitable for **medical, legal, and business workflows**.

---

## Recommended Approach – Role-Based Workflow

### Roles
- **Data Filler** (e.g. Patient)
- **Signer 1** (e.g. Doctor)
- **Signer 2** (e.g. Witness / Admin)

Only the **Data Filler** can enter data. Signers must wait until the PDF is generated.

---

## Business Flow Diagram (Customer-Friendly)

```mermaid
flowchart TD
    A[Email sent with secure link] --> B[Patient opens link]
    B --> C[Fill form data]
    C --> D[Submit form]
    D --> E[System generates PDF]
    E --> F[Signer 1 signs PDF]
    F --> G[Signer 2 signs PDF]
    G --> H[Document completed]
```

---

## Status-Based Workflow (System View)

```mermaid
stateDiagram-v2
    [*] --> WAITING_FOR_DATA
    WAITING_FOR_DATA --> DATA_COMPLETED : Patient submits form
    DATA_COMPLETED --> PDF_GENERATED
    PDF_GENERATED --> WAITING_FOR_SIGNATURE_1
    WAITING_FOR_SIGNATURE_1 --> SIGNED_BY_1
    SIGNED_BY_1 --> WAITING_FOR_SIGNATURE_2
    WAITING_FOR_SIGNATURE_2 --> COMPLETED
    COMPLETED --> [*]
```

---

## What Happens If a Signer Opens the Link Too Early

```mermaid
sequenceDiagram
    participant Patient
    participant Signer
    participant System

    Signer->>System: Open signing link
    System-->>Signer: Document waiting for data
    Note right of Signer: Cannot sign yet

    Patient->>System: Fill & submit data
    System->>System: Generate PDF

    System-->>Signer: Ready to sign notification
    Signer->>System: Sign PDF
```

---

## Not Recommended – First Click Wins

This approach is shown only to explain **why it is not allowed**.

```mermaid
flowchart TD
    A[Same link sent to many users] --> B[User A opens first]
    B --> C[Fills data & signs]
    C --> D[PDF generated]
    D --> E[User B opens later]
    E --> F[Can only sign existing PDF]

    C -. Risk: wrong person fills data .- B
```

---

## Key Message for Customers

> “In our system, data must be completed before signing.
> Therefore, we define **who fills the data** and **who signs**.
> Signers will automatically wait until the PDF is ready.”

---

## Benefits

- Clear responsibility
- No data conflicts
- Audit & compliance friendly
- Predictable signing order
- Better user experience

---

_End of document_

