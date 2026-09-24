# 🏥 HCLS – Health Insurance Enrollment & Medical Assessment

A **Pega Constellation-based healthcare insurance enrollment application** designed to manage member medical assessments, automate eligible approvals, prevent duplicate assessments, and route non-eligible applications to enrollment approvers for manual review.

---

## 📌 Overview

The HCLS Health Insurance Enrollment application streamlines the process of assessing members who apply for health insurance programs.

The application enables users to:

- Search and retrieve member information
- Create and manage enrollment assessment cases
- Detect duplicate active assessments
- Automatically approve eligible members
- Route cases requiring review to enrollment approvers
- Approve or reject enrollment applications
- Track case status and assessment history
- Manage application users and access permissions

The application is designed using **Pega Platform with Constellation**, leveraging Pega's case management, routing, data pages, security, and automation capabilities.

---

## 🎯 Business Objective

The primary objective is to create an application that manages the **approval and rejection assessment of members for health insurance enrollment programs**.

The system reduces manual processing by automatically approving members who satisfy predefined enrollment criteria while routing other cases to authorized enrollment approvers.

---

## ✨ Key Features

### 🔐 1. User Authentication & Access Management

- Secure user login using email ID
- Password policy enforcement
- Minimum and maximum password length validation
- Password complexity requirements
- First-login password creation
- Session timeout after 5 minutes of inactivity
- Role-based access using Pega Access Groups

### 🔎 2. Member & Case Search

Users can search for members and existing cases using:

**Case Search**
- Case ID
- Medical ID

**Member Search**
- Member Name
- Date of Birth

Search results provide relevant information such as:

- Case ID
- Medical ID
- Case Status
- Created Date
- Resolved Date
- Member Name
- Date of Birth
- Pre-existing Illness
- Smoking Status
- Annual Income

---

### 📝 3. Enrollment Case Creation

Users can create an enrollment assessment case for a member.

Cases are automatically assigned an identifier using the format:

```text
MED-123
```

The enrollment case progresses through multiple stages:

```text
Case Details
     ↓
Approval
     ↓
Assessment Information
     ↓
Complete
```

---

### 🔍 4. Duplicate Case Detection

The application prevents multiple active assessments from being created for the same member.

Before creating an assessment, the system checks whether an existing open case exists for the member's Medical ID.

If a duplicate case is detected, the application displays:

- Case ID
- Medical ID
- Case Status
- Created Date
- Resolved Date

The existing case can also be opened directly from the search results.

---

### 🤖 5. Automated Enrollment Approval

The system contains an automated approval rule.

A member is eligible for **automatic approval** when:

```text
Pre-existing Illness = No
AND
Smoking = No
```

For eligible members, the application:

1. Evaluates the member's medical information
2. Sets the AutoApproval flag
3. Displays an approval notification
4. Automatically resolves the case
5. Updates the case status to:

```text
Resolved-Approved
```

This reduces unnecessary manual assessment.

---

### 👨‍⚕️ 6. Manual Assessment & Approver Assignment

If the member does not satisfy the automatic approval conditions, the case is routed for manual assessment.

The user selects an **Enrollment Approver** from an alphabetically sorted list.

The selected approver receives the case for further evaluation.

Possible outcomes include:

```text
Approve
Reject
```

Rejected cases can capture the appropriate reason for denial.

---

## 🔄 Application Workflow

```text
                    ┌──────────────────┐
                    │   User Login     │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │  Search Member   │
                    │   / Existing     │
                    │      Case        │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Duplicate Case   │
                    │      Check       │
                    └────────┬─────────┘
                             │
                  ┌──────────┴──────────┐
                  │                     │
                Yes                    No
                  │                     │
                  ▼                     ▼
          ┌──────────────┐      ┌────────────────┐
          │ Show Existing│      │ Create Enrollment│
          │     Case     │      │      Case       │
          └──────────────┘      └───────┬────────┘
                                        │
                                        ▼
                              ┌────────────────────┐
                              │ Medical Assessment │
                              └─────────┬──────────┘
                                        │
                                        ▼
                              ┌────────────────────┐
                              │ Auto-Approval Rule │
                              └─────────┬──────────┘
                                        │
                         ┌──────────────┴──────────────┐
                         │                             │
                    Eligible                     Not Eligible
                         │                             │
                         ▼                             ▼
                ┌─────────────────┐          ┌──────────────────┐
                │ Auto Approved   │          │ Assign Approver  │
                │                 │          └────────┬─────────┘
                └────────┬────────┘                   │
                         │                            ▼
                         │                   ┌──────────────────┐
                         │                   │ Manual Assessment│
                         │                   └────────┬─────────┘
                         │                            │
                         │                     ┌──────┴──────┐
                         │                     │             │
                         │                  Approve        Reject
                         │                     │             │
                         ▼                     ▼             ▼
                  ┌────────────────────────────────────────────┐
                  │              Case Completed                │
                  └────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| **Pega Platform** | Application development & case management |
| **Pega Constellation** | Modern application UI |
| **App Studio** | Application and case configuration |
| **Dev Studio** | Advanced configuration and development |
| **Data Pages** | Data retrieval and integration |
| **Insights** | Search and reporting |
| **Access Groups** | Role-based access control |
| **Case Types** | Enrollment workflow management |
| **Routing** | Assignment of cases to users |
| **Decision Logic** | Automated enrollment assessment |

---

## 🏗️ Application Architecture

The application follows a case-management architecture built around the Pega Platform.

```text
                    ┌─────────────────────────┐
                    │      Pega UI Layer      │
                    │   Constellation Views   │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │     Case Management      │
                    │   Enrollment Case Type   │
                    └────────────┬────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
              ▼                  ▼                  ▼
       ┌────────────┐     ┌─────────────┐    ┌─────────────┐
       │ Member     │     │ Assessment  │    │ Approval &  │
       │ Search     │     │ Logic       │    │ Routing     │
       └────────────┘     └─────────────┘    └─────────────┘
              │                  │                  │
              └──────────────────┼──────────────────┘
                                 ▼
                    ┌─────────────────────────┐
                    │      Data Layer         │
                    │ Data Pages / Case Data  │
                    └─────────────────────────┘
```

---

## 📊 Sample Member Data

The application uses member information including:

| Field | Example |
|---|---|
| Medical ID | 748390381 |
| First Name | Tafi |
| Last Name | Roberts |
| Date of Birth | 1967-10-03 |
| Pre-existing Illness | No |
| Smoking | No |

Enrollment approvers are maintained separately and can be retrieved through a dedicated data page.

---

## 🔑 Pega Components Used

### Case Types

```text
Enrollment
```

### Enrollment Stages

```text
1. Case Details
2. Approval
3. Assessment Information
4. Complete
```

### Access Groups

```text
Admin
Enrollment Approver
Regular User
```

### Data Pages

Example:

```text
D_EnrollmentApprovers
```

Used to retrieve enrollment approvers and populate the approver selection field.

### Insights

Two primary search experiences are provided:

```text
Case Search
Member Search
```

---

## ⚙️ Business Rules

### Automatic Approval

```text
IF
    Pre-existing Illness = "No"
    AND
    Smoking = "No"

THEN
    AutoApproval = True
    Case Status = "Resolved-Approved"
```

### Manual Assessment

```text
IF
    AutoApproval = False

THEN
    Assign Enrollment Approver
    ↓
    Manual Assessment
    ↓
    Approve / Reject
```

### Duplicate Prevention

```text
IF
    Open Case exists
    AND
    Medical ID matches

THEN
    Prevent duplicate active assessment
```

---

## 🔐 Security

The application incorporates role-based security using Pega Access Groups.

Different users receive access to functionality according to their responsibilities.

Security considerations include:

- Authentication
- Role-based authorization
- Password complexity
- Session timeout
- Restricted administrative functionality
- Controlled case assignment
- Access-group-based approver management

---

## 📈 Future Enhancements

Potential future improvements include:

- AI-assisted medical assessment
- Risk-based enrollment scoring
- Advanced fraud and duplicate detection
- Automated document verification
- Integration with external healthcare databases
- REST API integration with insurance providers
- Analytics dashboard for enrollment trends
- Automated email/SMS notifications
- Predictive approval recommendations
- Audit and compliance reporting
- Multilingual user interface

---

## 🚀 Project Outcomes

The application demonstrates how **Pega Constellation and case management** can be used to automate a healthcare insurance enrollment workflow.

Key outcomes include:

- Reduced manual assessment effort
- Automated processing of eligible members
- Prevention of duplicate active cases
- Structured case lifecycle management
- Role-based workflow routing
- Centralized member and case search
- Consistent application of enrollment rules

---

## 🧪 Testing

The application should be tested against scenarios including:

- Valid user login
- Invalid credentials
- Password policy validation
- Session timeout
- Member search
- Case search
- New enrollment case creation
- Duplicate case detection
- Auto-approval
- Manual approval
- Case rejection
- Approver assignment
- Role-based access restrictions

---

## 👩‍💻 Project

**HCLS – Health Insurance Enrollment & Medical Assessment**

Built as a **Pega Constellation case-management application** for automating health insurance enrollment assessment and approval workflows.
