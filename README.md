

# Omniverse: Multi-Robot Fleet Management Platform

Simulate real hardware robots through ROS 2. Control them via a web-based, real-time 3D dashboard. Add AI perception for intelligent autonomy.

**Architected & Maintained by**: [Puneet](https://puneetportfolio.vercel.app/) | [GitHub](https://github.com/puneetshukla041)

---

##  Overview

Omniverse is a full-stack, distributed robotics platform engineered to mirror enterprise-level robotics infrastructure. It bridges the gap between low-level hardware control and high-level web accessibility, providing a seamless operational interface for multi-robot fleets.

**Core Capabilities:**
- **ROS 2 Humble** for robust robot simulation, navigation, and hardware control.
- **Node.js (Fastify/TypeScript) Backend** acting as a high-performance ROS-to-Web bridge.
- **Next.js 16 Dashboard** featuring real-time, low-latency 3D visualization using React Three Fiber.
- **ML Microservice** (PyTorch + FastAPI) for computer vision, spatial awareness, and intelligent perception.

##  System Architecture & Data Flow

The system is decoupled into isolated, highly cohesive microservices communicating via WebSocket, REST, and DDS (Data Distribution Service).

```mermaid
graph TD
    subgraph Client [Client Tier: Next.js Frontend]
        UI[React 19 UI / shadcn]
        R3F[React Three Fiber 3D Vis]
    end

    subgraph Gateway [Gateway Tier: Node.js Backend]
        API[Fastify REST API]
        WS[Socket.IO Bridge]
    end

    subgraph Hardware [Robotics Tier: ROS 2 Workspace]
        RCL[rclnodejs Client]
        Sim[Gazebo Simulation]
        Nav[Nav2 / MoveIt2]
    end

    subgraph AI [Intelligence Tier: ML Service]
        CV[FastAPI + PyTorch]
        Models[YOLO / Custom CV Models]
    end

    %% Connections
    UI <-->|HTTP/REST| API
    R3F <-->|Low-Latency WS| WS
    API <--> WS
    WS <-->|Bidirectional Streams| RCL
    RCL <-->|ROS 2 DDS Topics| Sim
    RCL <-->|ROS 2 DDS Actions| Nav
    Sim -->|Raw Sensor/Camera Data| CV
    CV <--> Models
    CV -->|Processed Inference Data| RCL
