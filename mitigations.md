# Mitigations and Secure Design Recommendations

## Purpose

This document maps threats to practical security controls suitable for a connected vehicle / ADAS architecture. The focus is on controls that improve security posture without ignoring system realities such as safety sensitivity, constrained trust boundaries, and fleet-scale backend exposure.

## Core Security Design Principles

- minimize trust in externally reachable components
- isolate safety-relevant processing from remote interfaces
- authenticate devices, users, and services explicitly
- protect integrity of sensor and operational data
- apply defense in depth across vehicle, backend, and mobile layers
- log security-relevant actions in a forensically useful way

## Communication Security

### TLS For External Channels

Apply mutually authenticated TLS or strongly authenticated TLS with device identity for:

- telematics-to-cloud traffic
- mobile-to-cloud API traffic
- software-management and provisioning channels

### Internal Network Protection

Use authenticated messaging or gateway-enforced policy controls for traffic crossing sensitive vehicle domains. Ethernet should not be treated as trusted simply because it is inside the vehicle.

## Cryptographic Controls

### AES-Based Data Protection

Use AES-based encryption for sensitive data at rest and for selected protected payloads where object-level confidentiality is required beyond transport security.

### PKI and Device Identity

Use certificate-based device identity for telematics units and backend trust establishment.

### Integrity Protection

Use cryptographic integrity checks or message authentication mechanisms for software artifacts, sensitive configuration payloads, and high-value internal messages crossing trust boundaries.

## Component-Level Mitigations

## 1. Camera and Radar Sensors

- secure boot and signed firmware updates
- authenticated maintenance and calibration workflows
- sanity checks and sensor-fusion plausibility validation
- diagnostics rate limiting and access restrictions
- tamper-aware event logging

## 2. ADAS Processing ECU

- secure boot, measured boot, and signed software deployment
- hardware-backed key storage where possible
- debug interface lockdown in production mode
- memory protection and least-privilege service design
- authenticated input validation from trusted gateways only
- runtime monitoring for anomalous sensor or message behavior

## 3. Vehicle Ethernet Network

- segmentation between safety-relevant, diagnostics, infotainment, and telematics domains
- gateway allowlists for message types and source identities
- replay detection and freshness checks where operationally viable
- bandwidth protection and QoS for safety-relevant traffic
- internal network monitoring for anomalous node behavior

## 4. Telematics Control Unit

- hardened network-facing services
- minimal exposed ports and protocols
- certificate-based device authentication
- signed updates with rollback protection
- firewalling between telematics and higher-trust vehicle networks
- strong separation between remote services and safety-relevant functions
- rate limiting and malformed-request handling

## 5. Cloud Backend

- strong service-to-service authentication
- role-based and attribute-based access control
- server-side authorization for all user and device actions
- encryption at rest for telemetry, identity, and audit stores
- secure secrets management
- immutable or append-protected audit logging
- anomaly detection for API misuse and fleet-wide abuse patterns

## 6. Mobile Application

- short-lived tokens and secure refresh handling
- secure platform storage for secrets
- MFA support for high-risk account operations
- server-side enforcement of authorization decisions
- certificate pinning where operationally justified
- jailbreak or root detection as a defense-in-depth signal, not a sole control

## Logging and Repudiation Controls

Security-relevant events should be logged with:

- authenticated user or device identity
- source context
- action performed
- target resource
- timestamp
- outcome
- correlation identifier

## Access Control Recommendations

- enforce least privilege for cloud services, mobile actions, and maintenance workflows
- separate user capabilities from service or engineering capabilities
- restrict remote actions that can influence safety-relevant functions
- require additional authorization checks for high-impact operations

## Availability Protections

- rate limiting on telematics and cloud APIs
- circuit breakers and isolation for backend dependencies
- resource controls on parsing and session handling
- degraded-mode behavior for loss of cloud connectivity
- network QoS and prioritization for critical in-vehicle traffic

## Residual Risk Considerations

Even with strong controls, the following residual risks remain relevant:

- physical attacks on sensors and service interfaces
- complex supply-chain or firmware trust compromise
- high-capability radio or sensor interference attacks
- zero-day compromise of exposed telematics or backend services

## Threat-To-Control Summary

| Threat Area | Primary Controls |
|---|---|
| Sensor spoofing or tampering | sensor-fusion plausibility checks, signed firmware, authenticated maintenance, anomaly monitoring |
| Rogue internal node activity | segmentation, gateway allowlists, authenticated messaging, internal monitoring |
| Telematics compromise | hardened services, minimal exposure, signed updates, firewalling, device certificates |
| Cloud identity abuse | strong authentication, certificate-based device identity, RBAC or ABAC, secure secrets management |
| Mobile account misuse | MFA, short-lived tokens, secure storage, server-side authorization enforcement |
| Repudiation gaps | structured audit logging, identity-bound events, immutable or append-protected logs |
| Telemetry disclosure | TLS in transit, AES-based encryption at rest, least-privilege storage access |
| Availability attacks | rate limiting, parser hardening, QoS, service isolation, degraded-mode behavior |
