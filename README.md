# Chronos Engine ⏳

**Distributed Task Scheduler**

Chronos Engine is a robust, horizontally scalable system designed to schedule, manage, and execute background tasks with high reliability and visibility. It is built to offload heavy processing from main user journeys in enterprise platforms.

## 🚀 Project Overview

*   **Goal:** Build a scalable system for background tasks (e.g., email notifications, report generation, data syncing).
*   **Target Audience:** Enterprise platforms (e.g., insurance portals) requiring reliable offloading of heavy processing.

## ✨ Key Features

### Core Scheduling
*   **One-time Tasks:** Schedule execution at a specific future timestamp.
*   **Recurring Tasks (CRON):** Support for standard cron expressions for periodic jobs.
*   **Dynamic Payload:** Pass custom JSON data with each task.

### Execution & Reliability
*   **Retries with Backoff:** Automatic retries for failed tasks using Exponential Backoff.
*   **Dead Letter Queue (DLQ):** Failed tasks after max retries are moved to DLQ for manual inspection.
*   **Task Prioritization:** Critical tasks (e.g., payments) take precedence over lower priority ones.

### Monitoring (The "Senior" Layer) *[Planned]*
*   **Real-time Dashboard:** Angular UI to view queue health (Active, Waiting, Completed, Failed).
*   **Execution Logs:** Searchable history with error stack traces.
*   **Manual Override:** Ability to "Retry Now" or "Cancel" pending tasks.

## 🛠️ Tech Stack

This project utilizes a modern "Power Stack" (2026 Industry Standard):

| Layer | Technology | Reason |
| :--- | :--- | :--- |
| **Frontend** | **Angular 18+** | Leverages Signals for real-time dashboard updates. |
| **Backend API** | **Node.js (NestJS)** | Enterprise Design Patterns (Dependency Injection). |
| **Message Broker** | **Redis** | Essential for high-speed queue management. |
| **Task Engine** | **BullMQ** | Industry standard for distributed queues; supports retries, priorities, parent-child jobs. |
| **Database** | **PostgreSQL** | Long-term auditing and RBAC data storage. |
| **Real-time** | **Socket.IO** | Pushing task status updates to the dashboard instantly. |
| **Infrastructure** | **Docker & AWS** | Containerized Workers for scaling on EC2 or ECS. |

## 🏗️ System Architecture

1.  **Producer (API):** Receives requests and pushes tasks to Redis via BullMQ.
2.  **Broker (Redis):** Acts as the high-performance buffer.
3.  **Consumers (Workers):** Independent processes that "watch" the queue. Horizontally scalable.
4.  **Observer (Socket.IO):** Listens for events (Completed/Failed) and alerts the Angular UI.

## ⚙️ Non-Functional Requirements

*   **Scalability:** effortless addition of "Workers" to handle increased load.
*   **Persistence:** Tasks survive server restarts (backed by PostgreSQL and Redis).
*   **At-Least-Once Delivery:** Guarantee that no task is lost, even on worker crashes.
*   **Observability:** Integrated logging and performance metrics.