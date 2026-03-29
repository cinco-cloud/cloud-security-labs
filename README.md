# 🔐 Lab 01 — IAM: Users, Groups & Roles
**Cloud Security Lab Series | cinco-cloud**

| Field | Details |
|---|---|
| **Difficulty** | Beginner |
| **Platforms** | Microsoft Azure + Amazon Web Services |
| **Environment** | KodeKloud Azure Sandbox + KodeKloud AWS Sandbox |
| **Time Estimate** | 45–60 minutes |
| **Skills Demonstrated** | IAM, RBAC, Identity Management, Least Privilege |

---

## Lab Overview

Identity and Access Management (IAM) is the foundation of cloud security. Before securing networks, storage, or compute — you must control who can access what.

| Concept | Azure Term | AWS Term |
|---|---|---|
| User directory | Entra ID | IAM |
| Individual identity | User | IAM User |
| Collection of users | Group | IAM Group |
| Set of permissions | Role (RBAC) | IAM Role / Policy |
| Permission document | Role definition | IAM Policy (JSON) |

---

## Launching the Sandboxes

**Azure**
1. Log in to [kodekloud.com](https://kodekloud.com)
2. Go to **Playgrounds** → **Azure Sandbox** → **Start Sandbox**
3. Use the provided portal URL, username, and password to sign in

**AWS**
1. Go to **Playgrounds** → **AWS Sandbox** → **Start Sandbox**
2. Use the provided console URL, username, and password to sign in

---

## PART A — Azure: Entra ID

### Step 1: Create a User

1. In the Azure Portal search bar, type `Microsoft Entra ID` → click it
2. Click **Users** in the left sidebar
3. Click **+ New user** → **Create new user**
4. Fill in:

| Field | Value |
|---|---|
| User principal name | `cloudlab-user01@[sandbox-domain].onmicrosoft.com` |
| Display name | `Cloud Lab User 01` |
| Password | Auto-generate — copy it |

5. Click **Review + create** → **Create**

> ✅ `Cloud Lab User 01` appears in the Users list.

---

### Step 2: Create a Group

1. In **Microsoft Entra ID**, click **Groups** → **+ New group**
2. Fill in:

| Field | Value |
|---|---|
| Group type | Security |
| Group name | `CloudLab-ReadOnly` |
| Membership type | Assigned |

3. Under **Members**, click **No members selected**
4. Search for `Cloud Lab User 01` → select → **Select**
5. Click **Create**

> ✅ Group `CloudLab-ReadOnly` created with 1 member.

---

### Step 3: Assign a Role (RBAC)

1. Search for `Subscriptions` → click your subscription
2. Click **Access control (IAM)** in the left sidebar
3. Click **+ Add** → **Add role assignment**
4. Search for `Reader` → select it → **Next**
5. Click **+ Select members** → search `CloudLab-ReadOnly` → select → **Select**
6. Click **Review + assign** → **Assign**

> ✅ `CloudLab-ReadOnly` group has Reader access to the subscription.

---

### Step 4: Verify Permissions

1. Open a private browser window
2. Go to [portal.azure.com](https://portal.azure.com)
3. Sign in as `cloudlab-user01` with the copied password
4. Browse resources — viewing works, creating or deleting is denied

> ✅ Reader role confirmed working.

---

## PART B — AWS: IAM Console

### Step 1: Create a User

1. In the AWS Console search bar, type `IAM` → click **IAM**
2. Click **Users** → **Create user**
3. Fill in:

| Field | Value |
|---|---|
| User name | `cloudlab-user-01` |
| Provide user access to the console | ✅ Enabled |
| Console password | IAM generated password |
| Users must create a new password | Unchecked |

4. Click **Next** → **Next** → **Create user**
5. Click **Download .csv** and save the file

> ✅ User `cloudlab-user-01` created in IAM.

---

### Step 2: Create a Group

1. Click **User groups** → **Create group**
2. Group name: `CloudLab-ReadOnly`
3. Under **Attach permissions policies**, search `ReadOnlyAccess` → check the box
4. Under **Add users to the group**, select `cloudlab-user-01`
5. Click **Create user group**

> ✅ Group created with ReadOnlyAccess policy and user attached.

---

### Step 3: Create a Role

1. Click **Roles** → **Create role**
2. Trusted entity type: **AWS service**
3. Use case: **EC2** → **Next**
4. Search `AmazonS3ReadOnlyAccess` → check the box → **Next**
5. Fill in:

| Field | Value |
|---|---|
| Role name | `CloudLab-EC2-S3ReadOnly` |
| Description | `Allows EC2 instances to read from S3` |

6. Click **Create role**

> ✅ Role `CloudLab-EC2-S3ReadOnly` created.

---

### Step 4: Verify Permissions

1. Open a private browser window
2. Go to [console.aws.amazon.com](https://console.aws.amazon.com)
3. Sign in using the Account ID and credentials from the .csv file
4. Browse the console — viewing works, creating or deleting is denied

> ✅ ReadOnlyAccess confirmed.

---

## Comparison: Azure vs AWS

| Concept | Azure | AWS |
|---|---|---|
| **User directory** | Microsoft Entra ID | IAM |
| **Permission model** | RBAC (Role-Based) | Policy-Based (JSON) |
| **Read-only built-in** | `Reader` role | `ReadOnlyAccess` managed policy |
| **Service identity** | Managed Identity | IAM Role |
| **Group-based access** | ✅ Entra ID groups | ✅ IAM User Groups |

> A
