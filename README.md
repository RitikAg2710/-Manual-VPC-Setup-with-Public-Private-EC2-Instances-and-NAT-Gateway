# -Manual-VPC-with-Public-Private-EC2-Instances-and-NAT-Gateway
AWS Project


This project demonstrates how to manually create a VPC in AWS with public and private subnets, configure internet access using an Internet Gateway and a NAT Gateway, and securely connect a private EC2 instance via a bastion (public) host.

---

## 🛠️ Setup Details

### ✅ Manual VPC
- Created a **new VPC**
- Added:
  - 2 **Public Subnets**
  - 2 **Private Subnets**
- Created **2 Route Tables**:
  - One for public subnets
  - One for private subnets

---

### 🌐 Internet Gateway (IGW)
- Created and attached to the VPC
- Public Route Table:
  - Route `0.0.0.0/0` → **IGW**
  - Associated with both **public subnets**

---

## 💻 EC2 Instances

### 🔓 Public EC2 Instance
- OS: Ubuntu  
- Subnet: Public  
- Auto-assign Public IP: ✅ Yes  
- Security Group:
  - SSH (port 22) from Anywhere
  - HTTPS (port 443) from Anywhere  
- Instance launched and accessed via SSH using `.pem` key

### 🔐 Private EC2 Instance
- OS: Ubuntu  
- Subnet: Private  
- Auto-assign Public IP: ❌ Disabled  
- Security Group:
  - SSH (port 22) from Anywhere
  - HTTPS (port 443) from Anywhere  
- Instance launched — **not directly accessible**

---

## 🔁 SSH into Private EC2 (via Public EC2)

1. Logged into **Public EC2**
2. Created a file named `private.pem`
3. Uploaded Private EC2's key content  
4. Ran:

   chmod 600 private.pem
   ssh -i private.pem ubuntu@<Private-IP>


5. Successfully connected to **Private EC2**



## 🌍 curl / ping Test (Failed First)

* Ran: `curl google.com` — ❌ Failed
* Reason: No internet access in private subnet

---

## 🔄 NAT Gateway Setup

* Created **NAT Gateway** in **Public Subnet**
* Allocated and attached an **Elastic IP**
* Private Route Table:

  * Added route `0.0.0.0/0` → **NAT Gateway**

---

## ✅ Final Test

* SSH into Private EC2 via public
* Ran:

  ```bash
  curl google.com
  ping google.com
  ```
* ✅ Worked successfully
