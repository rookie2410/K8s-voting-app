# 🚀 Building a Cloud-Native Web App on Kubernetes (EKS) with MongoDB, API & Frontend Services

As a DevOps engineer passionate about Kubernetes and cloud-native architectures, I recently built and deployed a complete web application stack on **Amazon EKS** (Elastic Kubernetes Service). This project was a hands-on dive into managing containerized workloads with Kubernetes primitives like Deployments, StatefulSets, Services, and ELBs — all within a VPC to ensure secure, scalable, and resilient operations.

Below is a breakdown of the system’s architecture and key design choices, with visuals to tie it all together (imagery recommended for future inclusion).

---

## 🧱 Architecture Overview
The application consists of three core components, each deployed in a **Kubernetes cluster** hosted on **Amazon EKS**:

- **Frontend**: React-based UI served from a container
- **API**: Go service handling AJAX voting requests
- **Database**: MongoDB ReplicaSet storing language data and votes
- **Infra**: Amazon EKS cluster (Managed Node Group), EBS volumes, ALB

These are decoupled microservices that interact via **internal Kubernetes networking**.

---

## 🔍 Deep Dive: Kubernetes Architecture Components

### 1️⃣ Frontend Service
- **Type**: Kubernetes Deployment
- **Pods**: 
  - Multiple replicas for high availability and load distribution.
- **Service**: 
  - Exposed via a Kubernetes Service of type `LoadBalancer`, which routes traffic through an **AWS ELB**.
- **Ingress**: 
  - Handles the `GET /` route to serve the application UI to users.
- **Highlights**:
  - External traffic flows: `AWS ELB → Kubernetes Service (type LoadBalancer) → Frontend Pods`.
  - Stateless layer designed for horizontal scaling.

### 2️⃣ API Layer
- **Type**: Kubernetes Deployment
- **Pods**: 
  - Multiple replicas for redundancy.
- **Service**: 
  - Exposed via another `LoadBalancer`-type Service with an attached ELB.
- **Routes**: 
  - Handles RESTful endpoints like `GET /languages/{name}` and `GET /languages/{name}/vote`.
- **Highlights**:
  - Internal microservice communicating with both frontend and database.
  - Auto-scaling and health checks ensure resilience.
  - Stateless architecture for effortless scaling.

### 3️⃣ MongoDB
- **Type**: Kubernetes StatefulSet
- **Pods**: 
  - 3 replicas (`pod0`, `pod1`, `pod2`).
- **Storage**: 
  - Each pod uses a dedicated `Persistent Volume Claim (PVC)` backed by **EBS volumes** (`pv0`, `pv1`, `pv2`).
- **Service**: 
  - Headless service to manage stateful communication between pods.
- **Highlights**:
  - `StatefulSet` ensures stable network identities and persistent storage across pod restarts.
  - Dedicated persistent volumes per pod preserve data integrity.
  - MongoDB’s disk identity and ordering requirements are gracefully handled.

---

## 🌐 Networking & Load Balancing
- **Two distinct ELBs**:
  - **Frontend ELB**: Routes traffic to frontend pods.
  - **API ELB**: Directs requests to the backend API.
- **Separation of concerns**:
  - Independent scaling and fault tolerance between UI and backend services.

---

## 🛡️ Security & Isolation
- All components deployed within a **secure VPC**.
- **Kubernetes namespaces** (e.g., `cloudacademy`) logically isolate resources.
- **MongoDB access** restricted to internal services only (no external exposure).

---

## 💬 Conclusion
This project was an insightful deep-dive into designing and operating a real-world **microservices application** on Kubernetes. From provisioning infrastructure on AWS to managing deployments and stateful services, every piece of this puzzle helped solidify expertise in **cloud-native DevOps**.

*(Include architecture diagrams here for visual context in a real-world scenario.)*