# Connected Vehicle Threat Modeling Project

## Overview

This repository contains a complete threat modeling and risk assessment package for a simulated connected vehicle / ADAS platform. The project models a realistic vehicle-side and cloud-connected architecture spanning sensors, in-vehicle compute, Ethernet-based communication, telematics, mobile access, and backend services. It is structured as a security engineering deliverable rather than a generic academic exercise.

The objective is to document system-level security analysis across architecture definition, data flow analysis, STRIDE-based threat identification, risk assessment, and mitigation planning for a modern connected vehicle environment.

## Scope

The modeled system includes:

- front camera
- front radar
- ADAS processing ECU
- in-vehicle Ethernet backbone and gateway behavior
- telematics control unit
- cloud backend services
- mobile application used by vehicle owners or fleet operators

The project assumes:

- Ethernet is used for high-bandwidth in-vehicle data movement
- internal vehicle communication crosses different trust zones
- telematics-to-cloud and mobile-to-cloud communication use TLS
- cryptographic controls, logging, and identity management are part of the security design

## Deliverables

- [architecture.md](architecture.md)
- [data-flow.md](data-flow.md)
- [threat-model.md](threat-model.md)
- [risk-assessment.md](risk-assessment.md)
- [mitigations.md](mitigations.md)
- [diagrams.md](diagrams.md)
- [diagrams/architecture.mmd](diagrams/architecture.mmd)
- [diagrams/dataflow.mmd](diagrams/dataflow.mmd)
- [diagrams/threat-overlay.mmd](diagrams/threat-overlay.mmd)

## System Summary

The modeled platform processes local sensor data for ADAS functions, exposes selected telemetry through the telematics unit, synchronizes operational data to cloud services, and enables user access through a mobile application. This creates multiple trust boundaries:

- sensor to ECU boundary
- in-vehicle network boundary
- vehicle to external network boundary
- cloud control plane boundary
- mobile user boundary

Each of these boundaries introduces different security concerns such as spoofed sensor inputs, ECU compromise, tampering on the vehicle network, telematics abuse, insecure cloud APIs, and unauthorized mobile commands.

## Security Objectives

The threat model uses the following security goals:

- preserve integrity of ADAS-relevant sensor and decision data
- restrict unauthorized control or configuration changes
- protect confidentiality of telemetry, credentials, and user data
- maintain traceability through secure audit logging
- preserve service availability for safety-relevant and operationally critical functions
- enforce trust boundaries across vehicle, backend, and user-facing interfaces

## Methods Used

This project applies:

- structured architecture decomposition
- data flow analysis
- trust boundary identification
- STRIDE threat modeling
- qualitative likelihood and impact scoring
- mitigation mapping to threats and components

## Tech Stack

- Markdown for security documentation
- Mermaid for architecture, data-flow, and threat-overlay diagrams
- STRIDE for threat categorization
- Qualitative likelihood and impact scoring for risk assessment

## How To Read This Project

1. Start with [architecture.md](architecture.md) for the component model.
2. Review [data-flow.md](data-flow.md) to understand trust boundaries, data movement, and attack surfaces.
3. Read [threat-model.md](threat-model.md) for STRIDE coverage by component.
4. Use [risk-assessment.md](risk-assessment.md) for prioritized threats and rationale.
5. Use [mitigations.md](mitigations.md) for security controls and design recommendations.
6. Use [diagrams.md](diagrams.md) to view all Mermaid diagrams rendered directly on GitHub.

## Diagram Index

- Architecture diagram: [diagrams/architecture.mmd](diagrams/architecture.mmd)
- Data flow diagram: [diagrams/dataflow.mmd](diagrams/dataflow.mmd)
- Threat overlay diagram: [diagrams/threat-overlay.mmd](diagrams/threat-overlay.mmd)
- GitHub-rendered diagram page: [diagrams.md](diagrams.md)

These diagrams are written in Mermaid so they render directly on GitHub and remain versionable as text.

## Key Security Themes

- authenticated in-vehicle and external communication
- secure segmentation between safety-relevant and externally reachable domains
- integrity protection for ADAS data pipelines
- defense in depth across vehicle, cloud, and mobile surfaces
- disciplined logging and traceability for forensic and operational review
