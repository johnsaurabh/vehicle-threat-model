# Risk Assessment

## Method

This assessment uses qualitative scoring:

- Likelihood: High, Medium, Low
- Impact: High, Medium, Low
- Risk Level: High, Medium, Low

Likelihood reflects attacker opportunity, complexity, exposure, and realism. Impact reflects safety influence, operational disruption, confidentiality loss, fleet-scale effects, and recovery cost.

## Risk Table

| Threat | Component | Likelihood | Impact | Risk Level |
|---|---|---|---|---|
| Spoofed camera input influences ADAS perception | Camera Sensor | Medium | High | High |
| Radar jamming or falsified returns degrade object detection | Radar Sensor | Medium | High | High |
| Rogue internal node injects unauthenticated data into the ECU | ADAS Processing ECU | Medium | High | High |
| Malicious or replayed frames alter data on the vehicle Ethernet network | Vehicle Ethernet Network | High | High | High |
| Network flooding disrupts sensor and ECU communications | Vehicle Ethernet Network | Medium | High | High |
| External exploit of telematics service yields remote foothold | Telematics Control Unit | Medium | High | High |
| TCU compromise pivots into higher-trust in-vehicle systems | Telematics Control Unit | Medium | High | High |
| Broken device authentication allows rogue vehicle identity in cloud APIs | Cloud Backend | Medium | High | High |
| Broken authorization enables unauthorized remote operations or data access | Cloud Backend | Medium | High | High |
| Telemetry or identity data leakage from backend storage | Cloud Backend | Medium | Medium | Medium |
| Mobile token theft allows user impersonation | Mobile Application | High | Medium | High |
| Mobile client tampering bypasses client-side restrictions | Mobile Application | High | Medium | Medium |
| Insufficient logging prevents attribution of remote actions | Cloud Backend / Mobile Application | Medium | Medium | Medium |
| ECU service exploit yields privileged execution on safety-relevant compute | ADAS Processing ECU | Low | High | High |
| Sensor firmware compromise persists malicious behavior | Camera Sensor / Radar Sensor | Low | High | Medium |

## Rationale By Threat Area

## 1. Sensor Spoofing and Interference

Sensor deception has meaningful operational realism and can affect perception quality directly. While successful execution may require specialized conditions or equipment, impact is high because ADAS functions depend on sensor integrity.

## 2. Internal Network Tampering

The in-vehicle Ethernet network is a high-risk zone because multiple internal nodes can produce traffic and compromised nodes may already reside behind perimeter defenses. Tampering and replay are plausible if message authentication and segmentation are weak.

## 3. Telematics Compromise

The TCU is internet-adjacent and frequently one of the most exposed components in a connected vehicle. A successful exploit can provide persistence, data access, or a pivot path into more trusted in-vehicle domains, driving both likelihood and impact upward.

## 4. Backend Identity and Authorization Failures

Cloud platforms aggregate large volumes of vehicle and user data, and they often mediate remote service actions. Broken authentication or authorization can therefore affect many vehicles or accounts simultaneously, making the impact high even when exploitation complexity is moderate.

## 5. Mobile Token and Session Abuse

Mobile applications operate in user-controlled environments where token theft, device compromise, and request tampering are common threat patterns. The impact is often bounded by server-side authorization, but account abuse remains significant.

## 6. ECU Privilege Escalation

Direct privileged compromise of the ADAS ECU may be harder than attacking user-facing or network-facing components, so likelihood is lower. The impact remains high because the ECU is safety-relevant and trusted by other components.

## Risk Priorities

Immediate design attention should focus on:

1. telematics exposure and pivot prevention
2. internal network integrity and segmentation
3. backend identity and authorization controls
4. sensor and ECU data authenticity
5. auditability of remote and high-trust actions
