# MAESTRO
## Application with Built-in AI Functionality
### Layer-Specific Threats
| Layer | Components and Functions | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- | ---- |
| 1. Foundation Models | In-house model that performs image classification | | | |
| 2. Data Operations | Learning from test data | 1. Data poisoning | T1 | An attacker slips malicious data into the test data, inducing misrecognition |
| 3. Agent Framework | In-house agent that performs image classification | 2. Injection | T2 | An attacker makes malicious input, causing the system to malfunction |
| 4. Deployment and Infrastructure | On-premises server | 3. DoS | T4 | A large volume of access may exhaust resources and stop the service |
| 5. Evaluation and Observability | No logging function | 4. Untraceability | T8 | Because there is no logging function, it is difficult to follow the attacker's trail |
| 6. Security & Compliance | Access policy | | | |
| 7. Agent Ecosystem | - | | | |

### Threats That Span Layers
| Layer Combination | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- |
| L2+L4 | 5. Data poisoning | T1 | An attacker exploits a vulnerability in the infrastructure to alter the test data and induce misrecognition |

### Risk Assessment and Mitigations
| Threat Name | Threat | Likelihood (P) | Impact (I) | Risk (R) | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1. Data poisoning | T1 | 1 | 3 | 3 | Obtain test data from a trusted source |
| 2. Injection | T2 | 2 | 3 | 6 | Perform input validation |
| 3. DoS | T4 | 3 | 3 | 9 | Implement measures such as rate limiting |
| 4. Untraceability | T8 | 3 | 1 | 3 | Implement audit logs |
| 5. Data poisoning | T1 | 1 | 3 | 3 | Take measures such as regularly updating the infrastructure |

## Application Using an External LLM
### Layer-Specific Threats
| Layer | Components and Functions | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- | ---- |
| 1. Foundation Models | External model that summarizes the transcription | | | |
| 2. Data Operations | Summarization from the transcription data | | | |
| 3. Agent Framework | Agent that transcribes the recording data and summarizes it | 1. Prompt injection | T2,T5 | A string that performs prompt injection may be inserted into the string to be summarized, resulting in unintended instructions being carried out |
| 4. Deployment and Infrastructure | Server in the cloud, and third-party transcription and LLM | 2. DoS | T4 | A large volume of access may exhaust resources and stop the service |
| 5. Evaluation and Observability | No logging function | 3. Untraceability | T8 | Because there is no logging function, it is difficult to follow the attacker's trail |
| 6. Security & Compliance | Access policy and the guardrails of the third-party LLM | 4. Guardrail bypass | T7 | The guardrails may be bypassed, resulting in unintended instructions being carried out |
| 7. Agent Ecosystem | - | | | |

### Threats That Span Layers
| Layer Combination | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- |
| L1+L2+L3+L6 | 5. Prompt injection | T2,T5,T7 | An attacker inputs audio that causes prompt injection at the time of recording, inducing a malfunction when it is summarized |
| L3+L4 | 6. Prompt injection | T2,T5,T7 | By exploiting a vulnerability in the infrastructure to alter the recording data, a string containing prompt injection is generated, inducing a malfunction when it is summarized |

### Risk Assessment and Mitigations
| Threat Name | Threat | Likelihood (P) | Impact (I) | Risk (R) | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1. Prompt injection | T2,T5 | 1 | 3 | 3 | Perform input validation within the system as well |
| 2. DoS | T4 | 3 | 3 | 9 | Implement measures such as rate limiting |
| 3. Untraceability | T8 | 3 | 1 | 3 | Implement audit logs |
| 4. Guardrail bypass | T7 | 1 | 3 | 3 | Perform input validation within the system as well |
| 5. Prompt injection | T2,T5,T7 | 1 | 3 | 3 | Perform input validation within the system as well |
| 6. Prompt injection | T2,T5,T7 | 1 | 3 | 3 | Take measures such as regularly updating the infrastructure, perform input validation within the system as well |

## Application Using Agentic AI
### Layer-Specific Threats
| Layer | Components and Functions | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- | ---- |
| 1. Foundation Models | Search, recommendation, booking, and payment for hotels and air tickets using a third-party LLM | | | |
| 2. Data Operations | Retrieval of reservation data from external APIs | 1. Data poisoning | T1 | False information is input from an external API, resulting in an itinerary being recommended that cannot be booked |
| 3. Agent Framework | Agent that searches for and recommends hotels and air tickets according to the user's attributes, and further executes the flow from booking through payment | 2. Prompt injection | T2,T5 | A string that performs prompt injection may be inserted into the user's attributes, resulting in unintended instructions being carried out |
| 4. Deployment and Infrastructure | Server in the cloud, and third-party booking system, payment system, and LLM | 3. DoS | T4 | A large volume of access may exhaust resources and stop the service |
| 5. Evaluation and Observability | No logging function | 4. Untraceability | T8 | Because there is no logging function, it is difficult to follow the attacker's trail |
| 6. Security & Compliance | Access policy and the guardrails of the third-party LLM | 5. Guardrail bypass | T7 | The guardrails may be bypassed, resulting in unintended instructions being carried out |
| 7. Agent Ecosystem | Hotel reservation agent, air ticket reservation agent, and payment agent that work with the planning agent that operates everything | | | |

### Threats That Span Layers
| Layer Combination | Threat Name | Threat | Attack Scenario |
| ---- | ---- | ---- | ---- |
| L1+L2+L3+L6 | 6. Prompt injection | T2,T5,T7 | A string containing prompt injection is input into the reservation data, inducing a malfunction |
| L3+L4 | 7. Prompt injection | T2,T5,T7 | Using a vulnerability in the infrastructure, the reservation data is altered and a string containing prompt injection is inserted, inducing a malfunction in the planning agent |

### Risk Assessment and Mitigations
| Threat Name | Threat | Likelihood (P) | Impact (I) | Risk (R) | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1. Data poisoning | T1 | 1 | 3 | 3 | Use trusted information resources |
| 2. Prompt injection | T2,T5 | 1 | 3 | 3 | Perform input validation within the system as well |
| 3. DoS | T4 | 3 | 3 | 9 | Implement measures such as rate limiting |
| 4. Untraceability | T8 | 3 | 1 | 3 | Implement audit logs |
| 5. Guardrail bypass | T7 | 1 | 3 | 3 | Perform input validation within the system as well |
| 6. Prompt injection | T2,T5,T7 | 1 | 3 | 3 | Perform input validation within the system as well |
| 7. Prompt injection | T2,T5,T7 | 1 | 3 | 3 | Take measures such as regularly updating the infrastructure, perform input validation within the system as well |
