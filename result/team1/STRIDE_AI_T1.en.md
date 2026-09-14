# STRIDE+AI
## Application with Built-in AI Functionality
### STRIDE Table
#### TB1
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | ID/password authentication | No multi-factor authentication |
| T | TLS, input validation | |
| R | | No logging function |
| I | | |
| D | | No rate limiting |
| E | Privilege control | |

#### TB2
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | Token authentication | |
| T | TLS, input validation | |
| R | | No logging function |
| I | | Data is stored without encryption |
| D | | No rate limiting |
| E | Privilege control | |

#### TB3
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | | Data poisoning |
| T | | No resistance to adversarial attacks |
| R | | |
| I | | |
| D | | |
| E | | |

### Risk Assessment and Threats
| ID | Vulnerability | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| V1 | No multi-factor authentication for the customer | 2 | 3 | 3 | 2 | 5.3 | MID | Add multi-factor authentication for the customer |
| V2 | No audit log in the app | 1 | 2 | 2 | 1 | 1.7 | LOW | Add an audit log to the app |
| V3 | No rate limiting in the app | 2 | 3 | 3 | 2 | 5.3 | MID | Add rate limiting to the app |
| V4 | No log of the images sent from the camera | 1 | 2 | 2 | 1 | 1.7 | LOW | Add a transmission log to the camera |
| V5 | Images are stored on the camera without encryption | 2 | 2 | 2 | 3 | 6 | MID | Store images encrypted, define a retention period for images |
| V6 | There is a risk that the training data is poisoned | 2 | 2 | 2 | 3 | 6 | MID | Use only trusted training data |
| V7 | No resistance to adversarial attacks | 2 | 2 | 2 | 3 | 6 | MID | Add processing such as filtering to the images |

LOW: <3, MID: 4<=6, HIGH: 7<=9

## Application Using an External LLM
### STRIDE Table
#### TB1
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | ID/password authentication | No multi-factor authentication |
| T | TLS, input validation | |
| R | | No logging function |
| I | | |
| D | | No rate limiting |
| E | Privilege control | |

#### TB2
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | Token authentication | |
| T | TLS, input validation | Bias in the summarization results |
| R | Logging function | |
| I | Output validation | Leakage of the system prompt |
| D | Rate limiting | |
| E | Privilege control | Unintended behavior such as sending requests to external sites |

#### TB3
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | | Risk that the invitation code is guessed |
| T | TLS | |
| R | | No logging function |
| I | | Risk that a third party who has not been invited can view the summary content |
| D | | No rate limiting |
| E | | |

### Risk Assessment and Threats
| ID | Vulnerability | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| V1 | No multi-factor authentication for the customer | 2 | 3 | 3 | 2 | 5.3 | MID | Add multi-factor authentication for the customer |
| V2 | No audit log in the app | 1 | 2 | 2 | 1 | 1.7 | LOW | Add an audit log to the app |
| V3 | No rate limiting in the app | 2 | 3 | 3 | 2 | 5.3 | MID | Add rate limiting to the app |
| V4 | No validation for confidential information in the recording data | 1 | 2 | 2 | 1 | 1.7 | LOW | Obtain consent regarding the handling of confidential information, confirm that there is no problem with the processing of data by the external API |
| V5 | No output validation in the summarization API | 2 | 2 | 2 | 2 | 4 | MID | Add output validation to the summarization API |
| V6 | There is a risk that the invitation code is guessed | 3 | 3 | 3 | 3 | 9 | HIGH | Discontinue the invitation code scheme and modify it so that only logged-in users can view the content |
| V7 | Bias in the summarized data | 2 | 2 | 2 | 2 | 4 | MID | Add filtering as a countermeasure against prompt injection |
| V8 | Leakage of the system prompt | 2 | 2 | 2 | 3 | 6 | MID | Add filtering as a countermeasure against prompt injection |
| V9 | Unintended behavior such as sending requests to external sites | 2 | 2 | 2 | 3 | 6 | MID | Restrict unnecessary functionality on the LLM API side |

LOW: <3, MID: 4<=6, HIGH: 7<=9

## Application Using Agentic AI
### STRIDE Table
#### TB1
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | ID/password authentication | No multi-factor authentication |
| T | TLS, input validation | |
| R | | No logging function |
| I | | |
| D | | No rate limiting |
| E | Privilege control | |

#### TB2
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | Token authentication | |
| T | TLS, input validation | Bias in the results |
| R | | No logging function |
| I | Output validation | Leakage of the system prompt |
| D | | No rate limiting |
| E | Privilege control | Unintended behavior such as sending requests to external sites |

#### TB3
| | Mitigation | Vulnerability |
| ---- | ---- | ---- |
| S | Token authentication | |
| T | TLS, input validation | Bias in the results |
| R | Logging function | |
| I | Output validation | Leakage of the system prompt |
| D | Rate limiting | |
| E | Privilege control | Unintended behavior such as sending requests to external sites |

### Risk Assessment and Threats
| ID | Vulnerability | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| V1 | No multi-factor authentication for the customer | 2 | 3 | 3 | 2 | 5.3 | MID | Add multi-factor authentication for the customer |
| V2 | No audit log in the app | 1 | 2 | 2 | 1 | 1.7 | LOW | Add an audit log to the app |
| V3 | No rate limiting in the app | 2 | 3 | 3 | 2 | 5.3 | MID | Add rate limiting to the app |
| V4 | No log of the interactions with the AI Agent | 1 | 2 | 2 | 1 | 1.7 | LOW | Add a log of the AI Agent's interactions |
| V5 | No rate limiting for the AI Agent | 2 | 3 | 3 | 2 | 5.3 | MID | Add rate limiting to the AI Agent |
| V6 | Bias in the AI Agent's results | 2 | 2 | 2 | 2 | 4 | MID | Add filtering as a countermeasure against prompt injection |
| V7 | Leakage of the AI Agent's system prompt | 2 | 2 | 2 | 3 | 6 | MID | Add filtering as a countermeasure against prompt injection |
| V8 | Unintended behavior in the AI Agent, such as sending requests to external sites | 2 | 2 | 2 | 3 | 6 | MID | Restrict unnecessary functionality on the AI Agent side |
| V9 | No output validation for the LLM API | 2 | 2 | 2 | 3 | 6 | MID | Add output validation to the LLM API |
| V10 | Bias in the LLM API's results | 2 | 2 | 2 | 2 | 4 | MID | Add filtering as a countermeasure against prompt injection |
| V11 | Leakage of the LLM API's system prompt | 2 | 2 | 2 | 3 | 6 | MID | Add filtering as a countermeasure against prompt injection |
| V12 | Unintended behavior in the LLM API, such as sending requests to external sites | 2 | 2 | 2 | 3 | 6 | MID | Restrict unnecessary functionality on the LLM API side |



LOW: <3, MID: 4<=6, HIGH: 7<=9
