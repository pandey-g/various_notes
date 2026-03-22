*** <================ Notes on Load Balancer ===================> ***

Alright — I’ll give you **senior-level, interview-ready notes on Load Balancers**. Not textbook — this is how you *think + explain* in interviews.

---

# 🔥 Load Balancer — Complete Notes (HLD Focus)

---

## 1. 🚀 What is a Load Balancer?

A **Load Balancer (LB)** distributes incoming requests across multiple servers to:

* Improve **availability**
* Improve **scalability**
* Reduce **latency**
* Prevent **overloading a single server**

👉 One-liner for interview:

> “A load balancer acts as a traffic manager that distributes client requests across multiple backend servers to ensure high availability and optimal resource utilization.”

---

## 2. 🧠 Why do we need it?

Without LB:

* Single server → bottleneck ❌
* Single point of failure ❌
* Poor scalability ❌

With LB:

* Horizontal scaling ✅
* Fault tolerance ✅
* Better performance ✅

---

## 3. 🏗️ Types of Load Balancers (VERY IMPORTANT)

### 3.1 Based on OSI Layer

#### 🔹 Layer 4 (Transport Layer)

* Works on **IP + Port**
* Does NOT look into request content

👉 Example:

* TCP / UDP routing

👉 Pros:

* Fast ⚡
* Low overhead

👉 Cons:

* No smart routing

👉 Interview line:

> “L4 load balancer makes routing decisions based on network-level information like IP and port.”

---

#### 🔹 Layer 7 (Application Layer)

* Works on **HTTP/HTTPS**
* Can inspect:

  * URL
  * Headers
  * Cookies

👉 Example:

* `/api/users` → Server A
* `/images` → Server B

👉 Pros:

* Smart routing 🧠
* Better flexibility

👉 Cons:

* Slightly slower than L4

👉 Interview line:

> “L7 load balancers enable content-based routing, which is critical for microservices.”

---

## 4. ⚙️ Load Balancing Algorithms

---

### 🔹 4.1 Round Robin

* Requests distributed sequentially

👉 A → B → C → A → B → C

✔ Simple
❌ Ignores server load

---

### 🔹 4.2 Least Connections

* Send request to server with **fewest active connections**

✔ Better for uneven load
✔ Real-world preferred

---

### 🔹 4.3 Weighted Round Robin

* Assign weights to servers

👉 Example:

* A (weight 3), B (weight 1)

✔ Useful when servers have different capacities

---

### 🔹 4.4 Consistent Hashing (VERY IMPORTANT 🔥)

Used in:

* Caching systems
* Distributed systems

👉 Key idea:

> Same user/request → same server

✔ Helps with:

* Session stickiness
* Cache locality

---

## 5. 🔐 Sticky Sessions (Session Affinity)

👉 Problem:
User logs in → request goes to different server → session lost ❌

👉 Solution:

* Route same user to same server

Ways:

* Cookies
* IP hashing

❗ Downside:

* Reduces load balancing efficiency

👉 Interview tip:

> “Sticky sessions are useful but hurt scalability; better to use distributed session stores like Redis.”

---

## 6. ❤️ Health Checks

LB continuously checks:

* Is server alive?
* Is it responding correctly?

Types:

* Active (ping, HTTP check)
* Passive (based on failures)

👉 If server fails → removed from pool

---

## 7. 🌍 Types of Deployment

---

### 🔹 7.1 Hardware Load Balancer

* Physical device (F5, etc.)
* Expensive 💰
* High performance

---

### 🔹 7.2 Software Load Balancer

Examples:

* Nginx
* HAProxy

✔ Flexible
✔ Cheap
✔ Most common today

---

### 🔹 7.3 Cloud Load Balancer

Examples:

* AWS ELB
* GCP LB

✔ Managed
✔ Auto-scaling
✔ Highly available

---

## 8. ⚡ Advanced Concepts (Senior Level)

---

### 🔹 8.1 Reverse Proxy

LB often acts as:

> Reverse proxy = client doesn’t know backend servers

✔ Security
✔ Abstraction

---

### 🔹 8.2 SSL Termination

Instead of each server handling HTTPS:

* LB decrypts request
* Sends HTTP internally

✔ Reduces backend load

---

### 🔹 8.3 Rate Limiting

LB can:

* Prevent abuse
* Protect backend

---

### 🔹 8.4 Global Load Balancing

Across regions:

👉 Techniques:

* DNS-based routing
* Geo-routing

---

### 🔹 8.5 Anycast Routing

Same IP → multiple locations
Traffic routed to nearest server

---

## 9. ⚠️ Single Point of Failure (CRITICAL)

Load balancer itself can fail ❌

👉 Solution:

* Multiple LBs
* DNS routing
* Active-passive / active-active setup

---

## 10. 🧠 Interview Thinking Framework

When LB comes in design:

Ask:

1. **What type?**

   * L4 or L7?

2. **What algorithm?**

   * Round robin? Least connections?

3. **Stateful or stateless?**

   * Need sticky sessions?

4. **Failure handling?**

   * Health checks?

5. **Scale?**

   * Single LB or multiple?

6. **Geo distribution?**

   * Needed or not?

---

## 11. 💥 Common Interview Mistakes

❌ Saying “just use load balancer”
❌ Not specifying type (L4 vs L7)
❌ Ignoring failure of LB
❌ Overusing sticky sessions
❌ Not considering health checks

---

## 12. 🧩 Real-World Example (Use This in Interview)

> “In a system like Netflix, requests first hit a global DNS load balancer, then a regional L7 load balancer which routes traffic to microservices. They use consistent hashing for cache locality and health checks to ensure only healthy instances receive traffic.”

---

## 13. 🧭 Mental Model (VERY IMPORTANT)

Think of LB as:

> **Traffic Cop + Health Monitor + Smart Router**

---

If you want next level:

👉 I can:

* Give you **“Design a Load Balancer from scratch” (LLD)**
* Or ask a **real interview question where you must use LB properly**

Just say:
👉 *“Give me LB interview problem”*


*** <============== No tell me does individual server get an IP address or it's the load balancer ip address available and then internally sub netting happens? Explain me in AWS terminology !================> ***
Great — this is exactly the kind of question that separates **mid-level vs senior understanding**. Let’s clear this properly in **AWS terms + real mental model**.

---

# 🧠 Core Idea (Answer First)

👉 **Both are true — but at different layers**

* **Load Balancer has its own DNS (not fixed IP usually)**
* **Each backend server (EC2) has its own private IP**
* Traffic flows:

```
Client → Load Balancer → EC2 (private IP)
```

---

# 🏗️ 1. AWS Networking Basics (You MUST know this)

Before LB, understand:

### 🔹 VPC (Virtual Private Cloud)

* Your isolated network in AWS

### 🔹 Subnets

* Split VPC into smaller networks
* Example:

  * Public Subnet (internet-facing)
  * Private Subnet (backend servers)

---

# 🌐 2. Load Balancer in AWS

### Example: AWS Application Load Balancer (ALB)

👉 In AWS, you NEVER directly use IP
You use:

* DNS name like:

  ```
  my-app-alb-123.us-east-1.elb.amazonaws.com
  ```

---

## 🔹 Does Load Balancer have an IP?

👉 Yes, but:

* AWS manages multiple IPs internally
* They **change dynamically**
* You don’t rely on them

👉 So:

> “In AWS, load balancers are accessed via DNS, not fixed IPs.”

---

# 🖥️ 3. Backend Servers (EC2)

Each EC2 instance has:

* ✅ Private IP (inside VPC)
* ❌ Public IP (optional)

Example:

```
EC2-1 → 10.0.1.10
EC2-2 → 10.0.1.11
EC2-3 → 10.0.1.12
```

---

# 🔁 4. Full Request Flow (IMPORTANT)

```
User → Internet → ALB (Public Subnet)
                 ↓
          Target Group
                 ↓
     EC2 instances (Private Subnet)
```

---

## Step-by-step:

### 1. User hits:

```
myapp.com
```

### 2. DNS resolves to:

```
ALB DNS → AWS-managed IPs
```

### 3. ALB receives request

### 4. ALB forwards to:

```
EC2 private IP (10.x.x.x)
```

---

# 🎯 5. Key Concept — Target Groups

ALB doesn’t directly know EC2.

It uses:

👉 **Target Group**

* Collection of backend servers
* Can contain:

  * EC2 instances
  * IP addresses
  * Lambda (yes!)

---

# 🔐 6. Public vs Private Subnet Setup (VERY IMPORTANT)

---

## 🔹 Load Balancer

* Placed in **Public Subnet**
* Has internet access

---

## 🔹 EC2 Instances

* Placed in **Private Subnet**
* NO direct internet access

👉 This is best practice.

---

# 🧠 7. Your Original Question Answered Precisely

> “Does individual server get IP or only LB has IP and subnetting happens internally?”

### ✔ Correct understanding:

* Each server **HAS its own IP (private IP)**
* Load balancer also has **AWS-managed IPs (hidden behind DNS)**
* Subnetting is NOT done by LB — it's done at **VPC level**

---

# ⚡ 8. Important Clarifications (Senior Level)

---

### 🔹 LB is NOT doing NAT internally like you think

It’s not:

> “one IP → internally splitting via subnet logic”

Instead:

* It is a **reverse proxy**
* It actively forwards requests to specific backend IPs

---

### 🔹 Connection Flow

Two separate connections:

1. Client → LB
2. LB → EC2

👉 LB terminates and re-creates connection

---

### 🔹 Security Groups

* LB has its own SG
* EC2 has its own SG

Example:

* EC2 only allows traffic from LB SG

---

# 🔥 9. Real Interview Answer (Use This)

If interviewer asks:

> “How does AWS load balancer route traffic to instances?”

You say:

> “In AWS, the load balancer exposes a DNS endpoint which resolves to AWS-managed IPs. It sits in a public subnet and receives incoming traffic. It then forwards requests to backend EC2 instances using their private IPs via target groups. The instances typically reside in private subnets, and security groups ensure that only the load balancer can access them.”

---

# 💥 10. Common Misconception (You had this)

❌ “LB has one IP and internally subnet distributes”

✔ Reality:

* LB = smart proxy
* Subnet = network isolation (VPC concept)

---

# 🧩 Mental Model

Think like this:

> Load Balancer = **Receptionist**
> EC2 = **Employees in private rooms**
> Subnet = **Office layout**

Client never directly sees employees.

---

# 🚀 If you want next level

We can go deeper:

1. **NLB vs ALB vs CLB (deep differences)**
2. **How cross-zone load balancing works**
3. **How AWS scales load balancers internally**
4. **Design your own load balancer (LLD)**

Just say:
👉 *“Go deeper into LB internals”*
