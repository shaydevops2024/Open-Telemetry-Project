# **OpenTelemetry Demo – Architecture & Overview**

## **High-Level Illustration**

![Image](https://opentelemetry.io/blog/2022/demo-announcement/current-demo-architecture.png?utm_source=chatgpt.com)

![Image](https://cdn.sanity.io/images/rdn92ihu/production/b1c172e7f1a8895bf3b9a2a6d4ab10f9f93161b5-2902x1398.png?auto=format\&fit=max\&utm_source=chatgpt.com)

![Image](https://timescale.ghost.io/blog/content/images/2022/03/architecture-diagram-demo-env.png?utm_source=chatgpt.com)

---

## **1. Project Overview**

The **OpenTelemetry Demo** is a complete, production-like microservices application that demonstrates how modern observability works using:

* **Distributed Tracing**
* **Metrics Collection**
* **Logs Correlation**
* **OpenTelemetry Collector Pipelines**
* **Visualization in Grafana + Jaeger**

It is intentionally polyglot (many languages) and multi-service to simulate real-world cloud architectures.

### **High-Level Concept Diagram**

```
 User → Frontend → Envoy → Microservices → OTel Collector → Observability Backends
```

---

## **2. Requirements to Start**

### **Hardware**

* **4–8 GB RAM**
* Any OS (Linux / macOS / Windows)

### **Software**

* Docker + Docker Compose
  **or**
* Kubernetes + Helm
* Kubectl
* Internet access to pull images

### **Optional**

* Grafana
* Jaeger
* Prometheus
* OpenSearch

---

## **3. Architecture Overview**

Here is a **visual understanding** of the system structure:

![Image](https://www.cncf.io/wp-content/uploads/2022/07/architecture-diagram-demo-env.png?utm_source=chatgpt.com)

![Image](https://blog.axway.com/wp-content/uploads/2019/08/How-to-Photograph-a-Black-Hole-Observing-Microservices-with-Open-Telemetry-3.png?utm_source=chatgpt.com)

![Image](https://opentelemetry.io/docs/demo/collector-data-flow-dashboard/otelcol-data-flow-overview.png?utm_source=chatgpt.com)

### **3.1 System Layers**

```
+---------------------------+
|   Web & Mobile Clients    |
+---------------------------+
              |
              v
+-----------------------------------+
|        Envoy Frontend Proxy       |
+-----------------------------------+
              |
              v
+---------------- Backend ----------------+
| Product Catalog  | Cart | Checkout     |
| Payment          | Shipping            |
| Recommendation   | Ads                 |
| Currency         | Fraud Detection     |
| Accounting       | Email               |
| Image Provider   | ...                 |
+----------------------------------------+
              |
    +---------------------+
    | Redis / Kafka       |
    +---------------------+
              |
              v
+----------------------------------------+
|     OpenTelemetry Collector            |
|   (Receives → Processes → Exports)     |
+----------------------------------------+
              |
              v
+----------------------------------------+
| Prometheus | Jaeger | Logs Backend     |
+----------------------------------------+
              |
              v
+---------------+
|    Grafana    |
+---------------+
```

---

## **4. Backend Components**

Below are **visuals + explanations** for each backend service type.

---

### **4.1 Cart Service**

![Image](https://doc.akka.io/libraries/guide/microservices-tutorial/_images/example-overview.drawio.svg?utm_source=chatgpt.com)

![Image](https://miro.medium.com/1%2Aaf7npKtH5Kmf9tVKL5Cdzg.png?utm_source=chatgpt.com)

**Purpose**
Handles the user’s cart state:

* Add items
* Update quantities
* Remove items
* Calculate totals

**Data Storage**

* Uses **Redis/Valkey** for fast, real-time cart access.

**Observability (OTel)**

* Spans for every cart action
* Metrics for request count, latency, and errors
* Logs correlated with trace IDs

**Example Trace (high-level)**

```
Add Item → Redis Command → Response → Export Trace
```

---

### **4.2 Payment Service**

![Image](https://storage.googleapis.com/prd-hatenaa-engineering-asset/20190607111923.png?utm_source=chatgpt.com)

![Image](https://cdn-proxy.slickplan.com/wp-content/uploads/2021/07/ecommerce-checkout-user-flow.png?utm_source=chatgpt.com)

**Purpose**

* Validate and authorize payments
* Interact with mock gateway
* Publish events to **Kafka**
* Participate in fraud-detection workflow

**Observability**

* Span example: `"payment.authorize"`
* Attributes tracked:

  * Amount
  * Currency
  * Payment result
* Errors captured and linked to same trace
* Metrics: success rate, latency distribution

**Simplified Flow**

```
Checkout → Payment → Kafka Event → Fraud Detection → Checkout Response
```

---

### **4.3 Prometheus (Metrics)**

![Image](https://prometheus.io/assets/docs/architecture.svg?utm_source=chatgpt.com)

![Image](https://signoz.io/img/guides/2024/07/how-does-prometheus-work-Untitled.webp?utm_source=chatgpt.com)

**Purpose**

* Scrapes metrics from the **OpenTelemetry Collector**
* Stores time-series data for:

  * Latency
  * Request Volume
  * Error Rates
  * CPU / Memory Usage

**Why It Matters**

Prometheus allows the system to answer questions like:

* “What is the P95 latency of Checkout?”
* “Is the Cart error rate increasing?”
* “Did the new deployment slow down the Payment Service?”

---

### **4.4 Grafana (Visualization Layer)**

![Image](https://tyk-community-assets.s3.dualstack.us-east-1.amazonaws.com/original/2X/3/33ee9cc3250c665f6e8c963d08d59b4b009f7dab.png?utm_source=chatgpt.com)

![Image](https://dz2cdn1.dzone.com/storage/temp/15790286-instana-dashboard-screenshot.png?utm_source=chatgpt.com)

**Purpose**

Grafana is the **central UI** for:

* Metrics (Prometheus)
* Traces (Jaeger)
* Logs (OpenSearch, Loki, etc.)

**Capabilities**

* Build performance dashboards
* Drill down from a spike → specific trace → related logs
* Combine business and technical metrics in one view

---

# **5. Project Goals**

The OpenTelemetry Demo aims to:

###  **1. Teach Real Observability**

Provide an end-to-end, realistic example of distributed tracing, metrics, and logging.

###  **2. Serve as a Reference Architecture**

Demonstrate how modern cloud-native systems should integrate OpenTelemetry.

###  **3. Provide a Sandbox for DevOps & SRE**

Allow teams to:

* Test new exporters
* Experiment with collector pipelines
* Validate service instrumentation patterns

###  **4. Help Vendors Integrate With OTel**

A vendor-neutral environment for backend compatibility testing.