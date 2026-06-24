# MSc Thesis: Experimental Study of Kubernetes and GitOps CI/CD Architectures

**Author:** Alexios Stamelos  
**Institution:** University of Macedonia (PAMAK)  
**Date:** June 2026  

[![Thesis PDF](https://img.shields.io/badge/Read-Full_Thesis_(PDF)-blue.svg)](https://github.com/Stamalexx/MSc-Thesis-GitOps-Microservices/blob/main/Alexios_Stamelos_19_6_2026.pdf)

## Abstract
<div align="justify">
The transition from monolithic software architectures to distributed microservices has introduced significant deployment complexities, necessitating orchestration platforms such as Kubernetes and robust automation through DevOps methodologies. While traditional "push-based" Continuous Integration and Continuous Delivery (CI/CD) pipelines address some of these challenges, they frequently suffer from security vulnerabilities, delayed defect discovery, and pipeline brittleness. This thesis presents the design, implementation, and empirical evaluation of a declarative, "pull-based" GitOps CI/CD architecture specifically tailored for Kubernetes-based microservices. By decoupling the integration and delivery phases utilizing Jenkins and Argo CD across a strict multi-repository topology, the proposed architecture successfully abstracts administrative cluster access away from the CI server, thereby enhancing the overall security posture. A critical contribution of this study is the implementation of a "Shift-Left" testing paradigm. By programmatically synthesizing ephemeral, localized container orchestration environments and executing End-to-End (E2E) automation via Playwright, the pipeline establishes a definitive quality gate. This mechanism proactively intercepts both user interface regressions and catastrophic inter-service dependency failures, physically insulating the production cluster from defective logic. Furthermore, the architecture resolves inherent CI infrastructure vulnerabilities by incorporating defensive programming algorithms to handle structural codebase mutations during massive upstream synchronizations, alongside declarative "janitor" routines that automate aggressive resource hygiene. The system was empirically validated through five distinct operational scenarios. The results demonstrate successful continuous delivery, zero-touch disaster recovery via autonomous reconciliation of state drift, and high-fidelity runtime validation. Ultimately, this research proves that combining GitOps principles with ephemeral testing environments provides a highly resilient, scalable, and auditable operational baseline capable of supporting continuous software evolution.
</div>

**Keywords:** GitOps, Git, Jenkins, ArgoCD, Playwright, Oracle VirtualBox, Kubernetes, Helm, Continuous Integration, Continuous Delivery.

## Architecture & Methodology
The evaluation was conducted using the **Google Online Boutique** microservices demo, deployed across a localized Ubuntu Server 24 Kubernetes cluster. 

The automated pipeline executes in two decoupled phases:
1. **Continuous Integration (Jenkins):** Utilizes a dynamic `git diff` mechanism to build only modified services. It provisions an ephemeral, localized replica of the application via `docker-compose` to execute headless Playwright End-to-End (E2E) smoke tests. This acts as the ultimate quality gate, ensuring defective logic never reaches production.
2. **Continuous Delivery (Argo CD):** Once images are pushed to Docker Hub and the declarative Helm configurations are updated, Argo CD detects the state drift and autonomously synchronizes the live cluster with zero downtime.


<div align="center">
  <img width="354" height="736" alt="image" src="https://github.com/user-attachments/assets/deed7d05-d3b9-44c4-adfa-52969ac97bd2" />
</div>

<div align="justify">
<i>
Architectural flow of the GitOps-driven CI/CD pipeline. The diagram illustrates the decoupling of continuous integration (Jenkins) from continuous delivery (Argo CD), highlighting the multi-repository strategy and the "pull-based" deployment mechanism within the Kubernetes cluster.
</i>
</div>





## Experimental Scenarios Evaluated
The resilience of the architecture was empirically tested against five distinct scenarios:
* **Safe Feature Enhancement:** Zero-downtime synchronization of UI modifications.
* **UI Regression Detection:** Successful interception of simulated checkout errors before deployment.
* **Inter-Service Dependency Failure:** Detection of cascading backend crashes hidden during standard container compilation.
* **Unauthorized State Drift:** Demonstration of zero-touch disaster recovery where Argo CD autonomously healed manually deleted infrastructure.
* **Bulk Integration:** Resolution of pipeline brittleness and CI resource exhaustion (out of disk space) via dynamic path-validation and aggressive "Janitor" resource pruning.

## Technology Stack
* **Container Orchestration:** Kubernetes, Docker
* **GitOps & Delivery:** Argo CD, Helm
* **Continuous Integration:** Jenkins (Groovy)
* **Automated Testing:** Microsoft Playwright (Node.js/TypeScript)
* **Infrastructure:** Oracle VirtualBox, Ubuntu Server 24

## Related Repositories
The codebase utilized for this research is strictly segregated to enforce a secure separation of concerns:
* [**GitOps Repository**](https://github.com/Stamalexx/microservices-gitops-config.git) 
* [**Source Code Repository**](https://github.com/Stamalexx/microservices-demo-DevOps-apply.git)
* [**Docker Repositories**](https://hub.docker.com/repositories/stamalexx)
* [**E2E Testing Repository**](https://github.com/Stamalexx/microservices-playwright-testing.git)
