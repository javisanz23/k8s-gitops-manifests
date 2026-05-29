# 🔄 GitOps Continuous Delivery Pipeline with ArgoCD & Helm

Este repositorio gestiona de forma completamente declarativa e inmutable la infraestructura y el ciclo de vida de despliegue de aplicaciones sobre Kubernetes, aplicando metodologías **GitOps** mediante **ArgoCD** y empaquetamiento modular con **Helm**.

## 📐 Arquitectura del Sistema & Flujo GitOps

```text
[ Código Fuente ] ──> [ GitHub Actions (CI) ] ──> [ Docker Hub Image ]
                                                         │
┌───────────────────────── Clúster Kubernetes ───────────▼─────────────┐
│                                                                      │
│  ┌──────────────┐      Monitorea Estado Deseado       ┌────────────┐ │
│  │   ArgoCD     │ <──────────────────────────────────> │   GitHub   │ │
│  │ (Controller) │    (Sincronización Automática)       │ (Repo IaC) │ │
│  └──────┬───────┘                                     └────────────┘ │
│         │                                                            │
│         ▼ Aplica cambios en vivo                                     │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │ porfolio-prod (Namespace) ──> [ Pods v13 ] ──> [ Ingress Nginx ]│ │
│  └─────────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────────┘
