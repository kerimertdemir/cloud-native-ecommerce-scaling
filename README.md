# Cloud-Native Auto Scaling & Load Balanced E-Commerce Infrastructure

**Deployment Status:** `Production-Ready`  
**Region:** AWS Stockholm (`eu-north-1`)

---

## 📌 Architecture Overview
This repository contains the enterprise-grade architectural blueprint and deployment configuration for a highly available, fault-tolerant, and horizontally scalable cloud infrastructure designed for high-traffic e-commerce platforms.

The primary engineering objective is to mitigate single points of failure (SPoF), prevent hardware bottlenecks during volatile traffic spikes, and minimize infrastructure overhead through intelligent resource orchestration. The entire stack is fully managed and provisioned utilizing core **Amazon Web Services (AWS)** compute and networking abstractions.

---

## 🛠 Tech Stack & Core Infrastructure Components
- **Cloud Provider:** Amazon Web Services (AWS)
- **Compute Layer:** AWS EC2 (Elastic Compute Cloud) & Customized Amazon Machine Images (AMI)
- **Elasticity Engine:** AWS Auto Scaling Groups (ASG)
- **Traffic Routing:** AWS Elastic Load Balancing (ELB) / Application Load Balancer (ALB)
- **Architectural Topology:** Multi-AZ (Availability Zone) Subnet Deployment

---

## 📐 Systems Engineering & Scaling Mechanics

The infrastructure implements an automated provisioning pipeline structured across the following operational layers:

1. **Ingress Traffic Routing:** Public HTTP traffic (Port 80) is intercepted by an **Application Load Balancer**. The ALB evaluates real-time target health checks and symmetrically balances incoming connection pools.
2. **High Availability & Fault Tolerance:** To eliminate localized data center outages, the compute instances are distributed across isolated, redundant subnets within the AWS Stockholm region (`eu-north-1a`, `eu-north-1b`, and `eu-north-1c`).
3. **Infrastructure Blueprinting:** Worker nodes are spawned dynamically using an **AWS Launch Template** instead of manual configuration. This template defines hardware baseline constraints (`t3.micro`), encapsulates system runtime dependencies via a custom AMI, and enforces strict firewall isolation rules via `Security Groups` to only allow ingress traffic on internal port `8080`.
4. **Target Tracking Elasticity Policy:** Automated scaling decisions are driven by CloudWatch metric aggregations. A reactive target tracking policy triggers a horizontal scale-out event the moment aggregate **Average CPU Utilization exceeds 60%**. Conversely, when traffic subsides, resources are gracefully decommissioned down to the baseline threshold to optimize cost.

---

## 🚀 Production Configuration Constraints

The orchestration layers are bound to the following production runtime constraints:

| Parameter | Configuration Value | Operational Description |
| :--- | :--- | :--- |
| **Desired Capacity** | `1` | Default operational baseline instance count |
| **Minimum Capacity**| `1` | Minimum instance threshold to ensure continuous service availability |
| **Maximum Capacity**| `2` | Upper boundary limit for horizontal scale-out during heavy load spikes |
| **Scaling Metric** | `Average CPU Utilization > 60%` | CloudWatch metric telemetry trigger for auto-scaling routines |
| **Instance Class** | `t3.micro` | Computed hardware instance profile matching application overhead |
| **Warm-up Period** | `300 seconds` | Cooldown buffer allowing Spring Boot application boot and runtime registration |

---

## 📊 Verification & Deployment Status (Outputs)

The entire deployment layer has been fully verified and validated via the live AWS console interface. Upon triggering the `ecommerce-asg` orchestration block, the state successfully transitioned to `At desired capacity`, dynamically provisioning the computed EC2 worker nodes without any manual intervention.

The complete production source code (`src.zip`) and build configuration (`pom.xml`) are tracked within this repository to verify system integration. Detailed execution evidence, live telemetry outputs, and infrastructure state logs are thoroughly mapped in the engineering report.
