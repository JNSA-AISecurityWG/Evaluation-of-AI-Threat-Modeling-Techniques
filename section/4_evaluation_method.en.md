# Evaluation Method
The data used for the evaluation are the results of threat modeling performed with the STRIDE, STRIDE+AI, and MAESTRO techniques on the three types of models presented in Chapter 3. For data collection, three teams of three members each evaluated the three types of models using the STRIDE, STRIDE+AI, and MAESTRO techniques, and their results were collected. The way each technique was carried out during data collection is described below.

## STRIDE
STRIDE was carried out on the DFD provided, following the flow below.

1. Setting trust boundaries (three)
Three trust boundaries are set on the DFD.

2. Creating a STRIDE table for each trust boundary
In the STRIDE table, mitigations and vulnerabilities are examined for each item of STRIDE.

| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | | |
| T | | |
| R | | |
| I | | |
| D | | |
| E | | |

3. Risk assessment and presentation of mitigations for the threats
The simplified version of the OWASP Risk Rating Methodology is used for risk assessment. As for the risk score, 3 or below is LOW, 4 to 6 is MID, and 7 to 9 is HIGH.
The rating formula is as follows.
((Ease of Exploit + Awareness + Intrusion Detection) / 3) * Technical Impact = Score
The format of the summary table is as follows.

| ID | Vulnerability | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| V1 | | | | | | 1.6 | LOW | |
| V2 | | | | | | 5.3 | MID | |
| V3 | | | | | | 1.6 | LOW | |
| V4 | | | | | | 1.6 | LOW | |

## STRIDE+AI
STRIDE+AI was carried out on the DFD provided, following the flow below.

1. Setting trust boundaries (three)
Three trust boundaries are set on the DFD.

2. Creating a STRIDE table for each trust boundary
In the STRIDE table, mitigations and vulnerabilities are examined for each item of STRIDE. As for the threats, based on the OWASP AI Exchange [1], specifically the Periodic table of AI security [2] published there, AI-specific threats are also examined in addition to ordinary threats.

| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | | |
| T | | |
| R | | |
| I | | |
| D | | |
| E | | |

3. Risk assessment and presentation of mitigations for the threats
The simplified version of the OWASP Risk Rating Methodology is used for risk assessment. As for the risk score, 3 or below is LOW, 4 to 6 is MID, and 7 to 9 is HIGH.
The rating formula is as follows.
((Ease of Exploit + Awareness + Intrusion Detection) / 3) * Technical Impact = Score
The format of the summary table is as follows.

| ID | Vulnerability | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| V1 | | | | | | 1.6 | LOW | |
| V2 | | | | | | 5.3 | MID | |
| V3 | | | | | | 1.6 | LOW | |
| V4 | | | | | | 1.6 | LOW | |


## MAESTRO
MAESTRO was carried out on the DFD provided, following the flow below.

1. Threats are identified by applying to the DFD the Detailed Threat Model (T1–T17) from the OWASP Agentic Security Initiative (ASI) [3] document "Agentic AI – Threats and Mitigations (Version 1.1)" [4].

2. Decomposition of the system
The system is decomposed into its constituent elements based on MAESTRO's "seven-layer architecture." Here, the constituent elements and functions of the agent are defined.

| Layer | Components and Functions |
| ---- | ---- |
| 1. Foundation Models| |
| 2. Data Operations| |
| 3. Agent Framework | |
| 4. Deployment and Infrastructure | |
| 5. Evaluation and Observability | |
| 6. Security & Compliance | |
| 7. Agent Ecosystem | |


3. Layer-Specific Threat Modeling  
Using the threat landscape specific to each layer, the threats latent in that layer are identified.


| Layer | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- |
| 1. Foundation Models | 1. | | |
| 2. Data Operations | 2. | | |
| 3. Agent Framework |  | | |
| 4. Deployment and Infrastructure |  | | |
| 5. Evaluation and Observability |  | | |
| 6. Security & Compliance |  | | |
| 7. Agent Ecosystem |  | | |

4. Identifying threats that span layers  
The interactions between layers are analyzed to examine how a vulnerability in one layer affects other layers (cascading risk).

| Layer Combination | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- |
|  | 3. | | |
|  |  | | |
|  |  | | |
|  |  | | |
|  |  | | |

5. Risk assessment and planning of mitigations  
Referring to the risk assessment method in [5], the "likelihood" and "impact" of each threat are evaluated using risk measurement criteria and a matrix. Based on the results, the priority of the threats to be addressed is determined. Risk is calculated based on the following formula and values.

R = P × I

- P (Likelihood): An estimate of the probability that the threat occurs under operational or adversarial conditions.
- I (Impact): The severity level of the consequences that may arise if the threat is achieved.

| Rating | Ordinal Value |
| ---- | ---- |
| Low | 1 |
| Medium | 2 |
| High | 3 |


| Threat Name | Threat | Likelihood (P) | Impact (I) | Risk (R) | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1. |  | | | | |
| 2. |  | | | | |
| 3. | | | | | |

## References
1. OWASP Foundation, Inc., AI Exchange, https://owaspai.org (retrieved March 28, 2026)
1. OWASP Foundation, Inc., 0\. AI Security Overview | AI Exchange, https://owaspai.org/docs/ai_security_overview/#periodic-table-of-ai-security (retrieved March 28, 2026)
1. OWASP Foundation, Inc., Agentic Security Initiative - OWASP Gen AI Security Project, https://genai.owasp.org/initiatives/agentic-security-initiative/ (retrieved March 28, 2026)
1. OWASP Foundation, Inc., Agentic AI - OWASP Lists Threats and Mitigations, https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/ (retrieved March 28, 2026)
1. arXiv, [2508.10043] Securing Agentic AI: Threat Modeling and Risk Analysis for Network Monitoring Agentic AI System, https://arxiv.org/abs/2508.10043 (retrieved March 28, 2026)
