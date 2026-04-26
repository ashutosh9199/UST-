# TripKar DevSecOps Presentation

Here is your complete presentation content. **I have completely redesigned the architecture diagrams to exactly mimic the visual structure of your PDF sample.** They now feature the exact same colored, dotted bounding boxes (red, green, blue) grouping the layers together, just like the Nexus Bank presentation!

---

## Slide 1: Title Slide
**TripKar**
*Secure, Scalable, and Automated Microservices Deployment using GitOps*

---

## Slide 2: Problem Statement
**The Challenge**
*   **Manual Deployments:** Traditional deployments are slow, risky, and prone to human error.
*   **Security Vulnerabilities:** Lack of automated scanning allows critical vulnerabilities to reach production.
*   **Environment Drift:** Inconsistencies between Development and Production lead to unpredictable application behavior.

---

## Slide 3: What Problem We Solve
**The TripKar Solution**
*   **100% Automation (GitOps):** Zero-touch deployments using ArgoCD.
*   **Shift-Left Security:** SAST and Container Scanning (Trivy) are embedded into the pipeline.
*   **Environment Parity:** The `dev` and `prod` environments are strictly isolated but deployed identically.

---

## Slide 4: Application Architecture
*This diagram mimics the layered, grouped aesthetic from your sample PDF, showing exactly how the frontend talks to the backend and databases.*

```mermaid
graph TD
    subgraph Frontend_Layer ["Frontend Layer"]
        Client((Client App))
    end
    
    subgraph Gateway_Layer ["Gateway Layer"]
        API_Gateway[API Gateway]
    end
    
    subgraph Microservices_Layer ["Backend Microservices Layer"]
        User[User Service]
        Booking[Booking Service]
        Payment[Payment Service]
        Search[Search Service]
        Notify[Notification Service]
    end
    
    subgraph Database_Layer ["Persistent Storage Layer"]
        Mongo[(MongoDB StatefulSet)]
    end

    Client -->|REST API| API_Gateway
    API_Gateway --> User
    API_Gateway --> Booking
    API_Gateway --> Payment
    API_Gateway --> Search
    API_Gateway --> Notify
    
    User --> Mongo
    Booking --> Mongo
    Payment --> Mongo

    style Frontend_Layer fill:none,stroke:#ff5555,stroke-width:2px,stroke-dasharray: 5 5
    style Gateway_Layer fill:none,stroke:#f1c40f,stroke-width:2px,stroke-dasharray: 5 5
    style Microservices_Layer fill:none,stroke:#2ecc71,stroke-width:2px,stroke-dasharray: 5 5
    style Database_Layer fill:none,stroke:#9b59b6,stroke-width:2px,stroke-dasharray: 5 5
    
    classDef default fill:#1e1e1e,stroke:#3498db,stroke-width:2px,color:#fff;
```

---

## Slide 5: Kubernetes Architecture
*This diagram mimics the Kubernetes cluster breakdown from your sample, showing namespace isolation.*

```mermaid
graph TD
    subgraph AWS_EC2 ["AWS EC2 EKS Cluster"]
        
        subgraph Argo_Namespace ["ArgoCD Namespace (Management)"]
            Argo[ArgoCD Controller]
            Rollouts[Argo Rollouts]
            Grafana[Grafana / Prometheus]
        end

        subgraph Dev_Namespace ["Dev Namespace (Auto-Sync)"]
            Dev_API[API Gateway Pod]
            Dev_User[User Pod]
            Dev_DB[(MongoDB Pod)]
        end
        
        subgraph Prod_Namespace ["Prod Namespace (Approval Required)"]
            Prod_API[API Gateway Pod]
            Prod_User[User Pod]
            Prod_DB[(MongoDB Pod)]
        end
        
    end
    
    Argo -.->|Syncs| Dev_Namespace
    Argo -.->|Syncs| Prod_Namespace

    style AWS_EC2 fill:none,stroke:#ffffff,stroke-width:2px
    style Dev_Namespace fill:none,stroke:#e74c3c,stroke-width:2px,stroke-dasharray: 5 5
    style Prod_Namespace fill:none,stroke:#2ecc71,stroke-width:2px,stroke-dasharray: 5 5
    style Argo_Namespace fill:none,stroke:#3498db,stroke-width:2px,stroke-dasharray: 5 5
    
    classDef default fill:#1e1e1e,stroke:#f1c40f,stroke-width:2px,color:#fff;
```

---

## Slide 6: CI - CD Architecture Overview
*This exactly mimics the horizontal flow from your "CI - CD Architecture Overview" slide, separated into the purple CI box and the blue CD box.*

```mermaid
graph LR
    subgraph Continuous_Integration ["Continuous Integration (GitHub Actions)"]
        Dev((Developer)) -->|Push Code| Git[GitHub Repo]
        Git --> Lint[ESLint Gate]
        Lint --> Audit[NPM Audit SAST]
        Audit --> Build[Docker Build]
        Build --> Trivy[Trivy Scan]
        Trivy --> Push[(Push to DockerHub)]
    end
    
    subgraph Continuous_Deployment ["Continuous Deployment (ArgoCD)"]
        Push -.-> Argo[ArgoCD Controller]
        Git -.->|Reads Manifests| Argo
        Argo -->|Deploys to| DevEnv[AWS Dev Environment]
        Argo -->|Deploys to| ProdEnv[AWS Prod Environment]
    end

    style Continuous_Integration fill:none,stroke:#9b59b6,stroke-width:2px,stroke-dasharray: 5 5
    style Continuous_Deployment fill:none,stroke:#3498db,stroke-width:2px,stroke-dasharray: 5 5
    
    classDef default fill:#1e1e1e,stroke:#e74c3c,stroke-width:2px,color:#fff;
```

---

## Slide 7: Docker & CI Details
**Containerization & Pipeline Logic**
*   **Multi-Stage Builds:** Dockerfiles are optimized to discard build tools, resulting in lightweight production images.
*   **Hardened Security:** Containers execute as a non-root `USER node` to prevent privilege escalation attacks.
*   **Immutable Tags:** Every image is tagged with the exact GitHub Commit SHA to guarantee precise traceability.

---

## Slide 8: Conclusion
**Final Thoughts**
*   The TripKar infrastructure successfully meets and exceeds all Capstone requirements.
*   Code deployments are now secure, fully automated, and deeply observable using Grafana and Prometheus.
*   The shift to GitOps guarantees that the infrastructure is reproducible, auditable, and incredibly fast to recover.
