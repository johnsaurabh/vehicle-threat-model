# Diagram Pack

This page embeds all Mermaid diagrams so they render directly on GitHub.

## 1. Architecture Diagram

Source: [diagrams/architecture.mmd](diagrams/architecture.mmd)

```mermaid
flowchart LR
    subgraph SD[Sensor Domain]
        Camera[Camera Sensor]
        Radar[Radar Sensor]
    end

    subgraph VD[Vehicle Domain]
        ECU[ADAS Processing ECU]
        Eth[Vehicle Ethernet Network]
        TCU[Telematics Control Unit]
    end

    subgraph CD[Cloud Domain]
        Cloud[Cloud Backend]
        Store[(Telemetry and Identity Stores)]
    end

    subgraph UD[User Domain]
        Mobile[Mobile Application]
    end

    Camera -->|Video frames| ECU
    Radar -->|Range and velocity| ECU
    ECU -->|Perception status and diagnostics| Eth
    Eth -->|Vehicle telemetry| TCU
    TCU <-->|TLS| Cloud
    Cloud -->|Persist telemetry, identity, audit data| Store
    Mobile <-->|TLS| Cloud
```

## 2. Data Flow Diagram

Source: [diagrams/dataflow.mmd](diagrams/dataflow.mmd)

```mermaid
flowchart LR
    subgraph TB1[Trust Boundary 1: Sensor Domain]
        Camera[External Process: Camera Sensor]
        Radar[External Process: Radar Sensor]
    end

    subgraph TB2[Trust Boundary 2: Vehicle Compute Domain]
        ECU[Process: ADAS ECU]
        Eth[Process: Vehicle Ethernet Network]
        TCU[Process: Telematics Control Unit]
    end

    subgraph TB3[Trust Boundary 3: Cloud Service Domain]
        Cloud[Process: Cloud Backend]
        Telemetry[(Data Store: Telemetry Store)]
        Identity[(Data Store: Identity and Audit Store)]
    end

    subgraph TB4[Trust Boundary 4: User Device Domain]
        Mobile[External Process: Mobile App]
    end

    Camera -->|Sensor frames| ECU
    Radar -->|Target detections| ECU
    ECU -->|ADAS status and diagnostics| Eth
    Eth -->|Selected telemetry| TCU
    TCU -->|Device-authenticated TLS session| Cloud
    Cloud -->|Write vehicle telemetry| Telemetry
    Cloud -->|Write audit and identity data| Identity
    Mobile -->|User-authenticated TLS API requests| Cloud
    Cloud -->|Vehicle state and account responses| Mobile
    Cloud -->|Authorized remote service requests| TCU
```

## 3. Threat Overlay Diagram

Source: [diagrams/threat-overlay.mmd](diagrams/threat-overlay.mmd)

```mermaid
flowchart LR
    Camera[Camera Sensor<br/>S: spoofed imagery<br/>T: calibration tampering<br/>D: sensor blinding]
    Radar[Radar Sensor<br/>S: false returns<br/>T: config tampering<br/>D: radar jamming]
    ECU[ADAS ECU<br/>S: rogue node inputs<br/>T: runtime/config tampering<br/>E: privileged code execution]
    Eth[Vehicle Ethernet<br/>S: fake node identity<br/>T: replay or message injection<br/>D: bandwidth flooding]
    TCU[Telematics Unit<br/>S: rogue backend/client identity<br/>T: remote payload tampering<br/>E: remote service exploit]
    Cloud[Cloud Backend<br/>S: fake user/device identity<br/>T: API or data manipulation<br/>E: broken authorization]
    Mobile[Mobile App<br/>S: token theft<br/>T: client request tampering<br/>E: privilege abuse through API]

    Camera --> ECU
    Radar --> ECU
    ECU --> Eth
    Eth --> TCU
    TCU <-->|TLS| Cloud
    Mobile <-->|TLS| Cloud
```
