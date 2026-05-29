# 🔄 Stack de GitOps Inmutable con ArgoCD y Helm Charts

Repositorio reproducible para un flujo end-to-end de Entrega Continua (CD) declarativa sobre Kubernetes. El objetivo es documentar la implementación, validación y automatización del ciclo de vida de una infraestructura inmutable utilizando el patrón arquitectónico Root Application, empaquetamiento modularizado con Helm v3 y reconciliación automática de estado con ArgoCD.

## 📝 Descripción General

Este proyecto documenta la construcción de una plataforma local de GitOps sobre un clúster Kubernetes (Minikube), aislando por completo el entorno de producción y gestionando de forma declarativa el despliegue automático de una aplicación web (Porfolio). 

El stack demuestra el uso de:
* **ArgoCD** como controlador GitOps centralizado.
* **Helm v3** para la abstracción, parametrización y versionado de manifiestos nativos.
* **Docker Hub** como registro público de imágenes inmutables.
* **Mecanismos de Autocuración (Self-Healing):** Corrección automática de derivaciones de configuración (*Drift Detection*).
* **Evidencias visuales** del proceso de despliegue, sincronización y reconciliación.

> Este repositorio funciona como evidencia técnica profesional para la validación de habilidades en infraestructura como código y flujos GitOps. Las configuraciones de la aplicación se gestionan de forma sanitizada mediante un archivo dinámico `values.yaml`.

---

## 🎯 Objetivo del Proyecto

El proyecto fue desarrollado como parte de mi portafolio profesional Cloud & DevOps, persiguiendo los siguientes hitos de ingeniería:
* **Implementar una arquitectura GitOps real:** Desacoplar el pipeline de integración (CI) del de despliegue (CD) para cumplir con el principio de separación de responsabilidades.
* **Garantizar la inmutabilidad:** Bloquear cualquier cambio manual arbitrario directo en el clúster (`kubectl`) mediante políticas de sincronización automatizadas.
* **Modularizar con Helm:** Evitar la duplicidad de código YAML parametrizando réplicas, puertos y tags de imágenes.
* **Documentar bajo estándares corporativos:** Crear un Runbook de despliegue y validación basado en evidencias reales verificables.

---

## 📐 Arquitectura de Infraestructura

La arquitectura implementa el patrón de reconciliación asíncrona, conectando el estado deseado en Git con el estado vivo del clúster de forma automatizada.

```text
[ Docker Hub ] ──( Descarga de Imagen v12 )──┐
                                             ▼
┌───────────────────────── Clúster Kubernetes (Minikube) ──────────────┐
│                                                                      │
│  ┌──────────────┐       Compara Estado Deseado         ┌────────────┐ │
│  │   ArgoCD     │ <──────────────────────────────────> │   GitHub   │ │
│  │ (Controller) │    (Polling automático / Webhook)    │ (Repo IaC) │ │
│  └──────┬───────┘                                     └────────────┘ │
│         │                                                            │
│         ▼ Aplica cambios declarativos                                │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │ porfolio-prod (Namespace) ──> [ Pods v12 ] ──> [ Service K8s ]  │ │
│  └─────────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────────┘
