# STRIDE
## Application with Built-in AI Functionality
### STRIDE Table
#### TB1

| TB1 | Mitigation | Vulnerability |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| S | Strong authentication (MFA), tightening of session management | Log in by impersonating another customer and view another company's product images and evaluation results |
| T | Encryption of communications with TLS, data integrity checks | Tamper with the "Evaluation results" between the browser and the front end to make a defective product look like a good one |
| R | A customer claims that they "did not receive the evaluation results," which becomes the cause of later trouble | Recording of access logs and the data viewing history |
| I | Interception of communications steals images containing customers' personal information and manufacturing know-how | HTTPS communication, control of the browser cache |
| D | Send a large volume of access to bring down the front end and stop the factory's shipping judgment | Setting of rate limits |
| E |                                                              |                                                              |

#### TB2

| TB2 | Mitigation | Vulnerability |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| S | Implement mutual authentication between internal components | An unauthorized process impersonates the evaluation process (Evaluate Product Image) and sends fake evaluation results |
| T | Implement encryption of communications | Rewrite the "Evaluation result" judged by the AI before it reaches the management component |
| R | Centrally manage the execution logs of the evaluation process | No trace remains that the AI's evaluation process was executed, so responsibility for a misjudgment cannot be traced |
| I | Unencrypted "Customer data" flowing over the internal network is intercepted | Encryption of internal communications, suppression of unnecessary internal data transfers |
| D | Send excessive requests to the evaluation process to exhaust resources | Throttling by the API Gateway, introduction of queue processing |
| E |                                                              |                                                              |

#### TB3

| TB3 | Mitigation | Vulnerability |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| S | Access control for the storage | An unauthorized script impersonates "Product Image I/O" and deletes or overwrites images in the storage |
| T | Hash value management for files, consideration of immutable storage (WORM) | Rewrite the "Product Images" in the storage to cause defects in the AI's retraining and re-evaluation |
| R | Enabling of audit logs for storage operations | There is no record of who uploaded or changed images and when, so unauthorized operations cannot be identified |
| I | Settings that block public access, encryption (server-side encryption) | A misconfiguration exposes the "Product Images" folder externally, leaking the state of the production line |
| D | Setting of storage quotas, monitoring and alert notifications | Deliberately exhaust the storage capacity to obstruct the saving of images from the line camera |
| E | Exploit a vulnerability in the I/O process to access other database files on the same server | Minimization of privileges |

### Risk Assessment and Threats

| ID | Threat (STRIDE) | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ------ | -------------------------------------- | ---------- | ------ | ---------- | ---------- | ---------- | ------ | -------------------------- |
| V1 | [S] Unauthorized login to the customer portal | 2 | 2 | 2 | 2 | 4 | MID | Strong password policy |
| V2 | [T] Rewriting of the judgment result displayed on screen | 1 | 1 | 2 | 3 | 4 | MID | Attaching digital signatures to the data |
| V3 | [R] Claiming not to have received the judgment results | 1 | 1 | 1 | 1 | 1 | LOW | Retention of receipt confirmation (ACK) logs |
| V4 | [I] Leakage of images from the browser cache | 1 | 2 | 2 | 2 | 3.33333333 | MID | Cache-control header settings |
| V5 | [D] DoS attack on judgment result queries | 2 | 3 | 3 | 3 | 8 | HIGH | Access restriction by a WAF |
| V6 | [E] Access to the administration screen by a general customer | 1 | 1 | 2 | 3 | 4 | MID | Implementation of strict RBAC |
| V7 | [S] Launch of a fake evaluation process | 1 | 1 | 2 | 3 | 4 | MID | Hash verification of the executable |
| V8 | [T] Tampering with the AI model file and parameters | 2 | 1 | 2 | 3 | 5 | MID | FIM (file integrity monitoring) |
| V9 | [R] Failure to record the history of changes to the judgment threshold | 1 | 1 | 1 | 2 | 2 | LOW | Enabling audit logs for configuration changes |
| V10 | [I] Leakage of model data in memory | 1 | 1 | 2 | 3 | 4 | MID | Consideration of memory encryption technology |
| V11 | [D] Resource exhaustion of the image processing process | 2 | 2 | 2 | 3 | 6 | MID | Resource limits |
| V12 | [S] Image injection by a fake line camera | 1 | 1 | 2 | 3 | 4 | MID | Authentication using device certificates |
| V13 | [T] Poisoning of the training images | 1 | 1 | 1 | 3 | 3 | LOW | Ensuring the immutability of the dataset |
| V14 | [R] Ignoring failures to save images | 1 | 1 | 2 | 2 | 2.66666667 | LOW | Liveness monitoring of the I/O status |
| V15 | [I] External exposure due to storage misconfiguration | 2 | 2 | 2 | 3 | 6 | MID | Automation of configuration scanning |
| V16 | [D] Write latency due to exhaustion of I/O bandwidth | 2 | 2 | 3 | 3 | 7 | MID | Reservation of storage IOPS |

LOW: <3, MID: 4<=6, HIGH: 7<=9

## Application Using an External LLM
### STRIDE Table
#### TB1

| TB1 | Mitigation | Vulnerability |
| ---- | ------------------------------------- | ------------------------------------------------------------ |
| S | Authentication (ID/password, etc.), session management | V1: Session hijacking/theft of credentials makes it possible to impersonate the Customer |
| T | Server-side input validation, TLS | V2: Insufficient validation against tampering with request parameters (summary ID/sharing recipient/recording metadata, etc.) |
| R | Operation logs / audit logs | V3: Insufficient audit logs for upload/summarization/sharing operations means the actor can repudiate by claiming they "did not do it" |
| I | HTTPS, privilege checks | V4: Leakage/guessing of the invitation code URL may allow summary data to be viewed without logging in |
| D | Rate limiting | V5: Bulk upload of large recordings and the like overloads the front end/back end (reduced availability) |
| E | Authentication and authorization | V6: Rewriting the Invitation Code and the like makes unauthorized access to and manipulation of other users' summary data possible |

#### TB2

| TB2 | Mitigation | Vulnerability |
| ---- | ------------------------ | ------------------------------------------------------------ |
| S | API key authentication | V7: Leakage or misconfiguration of the API key allows a third party to impersonate the legitimate application and use the transcription API |
| T | TLS | V8: Tampering at the application level with the recording data sent or the transcription results received cannot be detected |
| R | API call logs | V9: Insufficient records of recording data transmission and transcription execution allow repudiation |
| I | HTTPS, privilege checks | V10: Recording data and transcription data may leak via the external API |
| D | Rate limiting, timeouts | V11: A failure or delay in the external transcription API stops the function |
| E | Minimization of API key privileges | V12: An over-privileged key resulting from misconfiguration and the like enables unexpected operations and data reach |

#### TB3

| TB3 | Mitigation | Vulnerability |
| ---- | ------------------------ | ------------------------------------------------------------ |
| S | API key authentication | V13: Leakage or misconfiguration of the API key allows a third party to impersonate the legitimate application and use the summarization API |
| T | TLS | V14: Tampering at the application level with the transcription results sent or the summary data received cannot be detected |
| R | API call logs | V15: Repudiation of the execution of summary generation through tampering with the logs |
| I | HTTPS, privilege checks | V16: Transcription/summary data may leak via the external API |
| D | Rate limiting, timeouts | V17: A failure of the external LLM stops the summarization function |
| E | Minimization of API key privileges | V18: An over-privileged key resulting from misconfiguration and the like enables unexpected operations and data reach |

### Risk Assessment and Threats

| ID | Vulnerability | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ------ | ------------------------------------------------------------ | ---------- | ------ | ---------- | ---------- | ------ | ------ | ------------------------------------------------------------ |
| V1 | Session hijacking/theft of credentials makes it possible to impersonate the Customer | 3 | 3 | 2 | 3 | 8.0 | HIGH | MFA, session protection (SameSite/HttpOnly), short-lived tokens |
| V2 | Insufficient validation against tampering with request parameters (summary ID/sharing recipient/recording metadata, etc.) | 3 | 3 | 2 | 3 | 8.0 | HIGH | Server-side validation, signed requests, recomputation of critical values on the server |
| V3 | Insufficient audit logs for upload/summarization/sharing operations means the actor can repudiate by claiming they "did not do it" | 2 | 3 | 2 | 2 | 4.7 | MID | Audit logs (who/what/when/from where), tamper-resistant storage |
| V4 | Leakage/guessing of the invitation code URL may allow summary data to be viewed without logging in | 3 | 3 | 2 | 3 | 8.0 | HIGH | Strengthening the invitation code and shortening its lifetime, limits on the number of views and on the expiry, additional authentication at viewing time (optional) |
| V5 | Bulk upload of large recordings and the like overloads the front end/back end (reduced availability) | 2 | 3 | 3 | 2 | 5.3 | MID | Upload limits, rate limiting, queueing/asynchronous processing |
| V6 | Rewriting the Invitation Code and the like makes unauthorized access to and manipulation of other users' summary data possible | 3 | 2 | 2 | 3 | 7.0 | HIGH | Per-object authorization, privilege testing, access auditing |
| V7 | Leakage or misconfiguration of the API key allows a third party to impersonate the legitimate application and use the transcription API | 3 | 2 | 2 | 3 | 7.0 | HIGH | Key management, rotation, scope separation |
| V8 | Tampering with the recording data sent or the transcription results received cannot be detected | 2 | 2 | 2 | 3 | 6.0 | MID | Integrity verification by hashes/signatures |
| V9 | Insufficient records of recording data transmission and transcription execution allow repudiation | 2 | 2 | 2 | 2 | 4.0 | MID | Audit logs of API calls and results |
| V10 | Recording data and transcription data may leak via the external API | 2 | 3 | 2 | 3 | 7.0 | HIGH | Data minimization, masking, encryption |
| V11 | A failure or delay in the external transcription API stops the function | 2 | 3 | 3 | 2 | 5.3 | MID | Fallback, asynchronous processing |
| V12 | An over-privileged key resulting from misconfiguration and the like enables unexpected operations and data reach | 2 | 2 | 2 | 3 | 6.0 | MID | Least-privilege key design |
| V13 | Leakage or misconfiguration of the API key allows a third party to impersonate the legitimate application and use the summarization API | 3 | 2 | 2 | 3 | 7.0 | HIGH | Key management, rotation |
| V14 | Tampering at the application level with the transcription results sent or the summary data received cannot be detected | 2 | 2 | 2 | 3 | 6.0 | MID | Integrity verification |
| V15 | Repudiation of the execution of summary generation through tampering with the logs | 2 | 2 | 2 | 2 | 4.0 | MID | Audit logs |
| V16 | Transcription/summary data may leak via the external API | 2 | 3 | 2 | 3 | 7.0 | HIGH | Minimization, encryption |
| V17 | A failure of the external LLM stops the summarization function | 2 | 3 | 3 | 2 | 5.3 | MID | Fallback |
| V18 | An over-privileged key resulting from misconfiguration and the like enables unexpected operations and data reach | 2 | 2 | 2 | 3 | 6.0 | MID | Least privilege |


LOW: <3, MID: 4<=6, HIGH: 7<=9

## Application Using Agentic AI
### STRIDE Table
#### TB1

| TB1 | Mitigation | Vulnerability |
| ---------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| S: Spoofing | Multi-factor authentication (MFA) and session management by Customer Management | Account takeover (spoofing) due to an inadequate password policy or session management |
| T: Tampering | Encryption of communications between the front end and the back end with TLS | Tampering with request data (travel preferences, etc.) due to inadequate encryption on the communication path |
| R: Repudiation | Collection of access logs and operation logs at the API Gateway and elsewhere | Missing logs or a lack of tamper resistance makes it impossible to trace the user's operations |
| I: Information Disclosure | Securing a secure communication path, masking of confidential information on screen | Leakage of internal system information through eavesdropping on communications or through inappropriate error messages |
| D: Denial of Service | Rate limiting at the API Gateway, introduction of a WAF | Exhaustion of system resources by a large volume of requests (denial of service on the front end) |
| E: Elevation of Privilege | Role-based access control (RBAC) in Customer Management | Unauthorized access to other users' reservation and payment information due to inadequate authorization control (BOLA/IDOR) |

#### TB2

| TB2 | Mitigation | Vulnerability |
| ---------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------ |
| S: Spoofing | Introduction of mutual authentication between agents (mTLS, etc.) | Authentication between agents is insufficient, and a malicious process impersonates a legitimate agent |
| T: Tampering | Data signing and integrity checks for inter-agent communication | Messages between agents (reservation data and instructions) are tampered with |
| R: Repudiation | Recording of audit logs of requests and responses between agents | Communication logs between agents are insufficient, so the origin of an unauthorized instruction cannot be identified |
| I: Information Disclosure | Protection of confidential information in memory, encryption of inter-agent communication | Leakage of Payment data from shared memory or unencrypted internal communication |
| D: Denial of Service | Task queueing for agents and timeout settings | Exhaustion of internal resources due to excessive task submission from the Travel Planning Agent to other Agents |
| E: Elevation of Privilege | Thorough application of least privilege to specialized agents (Payment, etc.) | The Payment Agent can execute unauthorized payment processing on the instruction of the Travel Planning Agent alone |

#### TB3

| TB3 | Mitigation | Vulnerability |
| ---------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| S: Spoofing | Secure management of API keys using a secret manager | An API key hard-coded in the source code or elsewhere leaks, and the external API is used illicitly |
| T: Tampering | Sanitization and validation of responses from the external API | A fraudulent response from the external API is accepted without validation, and data inside the system is poisoned |
| R: Repudiation | Retention of logs of the external API call history and response content | Failures and errors in external API calls are not recorded, so where responsibility lies at the time of an incident becomes unclear |
| I: Information Disclosure | Masking of PII (personal information) in the data sent to the external API | Confidential information such as Payment data remains in the request that is sent and is transmitted externally |
| D: Denial of Service | Limits on external API calls (introduction of a circuit breaker) | A delay in or halt of the external API's response causes the AITravelApp process to hang |
| E: Elevation of Privilege | Use a separate API key with least privilege for each external service | If a single API key leaks, all the integrated external API services become operable |

### Risk Assessment and Threats

| ID | Vulnerability | Ease of Exploit | Awareness | Intrusion Detection | Technical Impact | Score | Risk | Mitigation |
| ---- | ------------------------------------------------------------ | ---------- | ------ | ---------- | ---------- | ------ | ------ | ------------------------------------------------------------ |
| V1 | (TB1-S) Account takeover (spoofing) due to an inadequate password policy or session management | 3 | 3 | 2 | 2 | 5.3 | MID | Introduction of multi-factor authentication (MFA) and implementation of strong session management. |
| V2 | (TB1-T) Tampering with request data due to inadequate encryption on the communication path | 2 | 2 | 2 | 2 | 4 | MID | Application of complete TLS encryption between the front end and the API Gateway. |
| V3 | (TB1-R) Missing logs or a lack of tamper resistance makes it impossible to trace the user's operations | 2 | 2 | 1 | 1 | 1.6 | LOW | Centralized collection of access logs at the API Gateway and forwarding to a tamper-proof log server. |
| V4 | (TB1-I) Leakage of internal system information through eavesdropping on communications or through inappropriate error messages | 2 | 2 | 1 | 2 | 3.3 | MID | Encryption of communications and generalization of the error messages displayed on the front end (hiding stack traces). |
| V5 | (TB1-D) Exhaustion of system resources by a large volume of requests (denial of service on the front end) | 3 | 3 | 3 | 2 | 6 | MID | Rate limiting at the API Gateway and blocking of anomalous traffic by a WAF (Web Application Firewall). |
| V6 | (TB1-E) Unauthorized access to other users' reservation and payment information due to inadequate authorization control | 2 | 3 | 2 | 2 | 4.6 | MID | Implementation of strict object-level authorization control for each API request (BOLA countermeasure). |
| V7 | (TB2-S) Authentication between agents is insufficient, and a malicious process impersonates a legitimate agent | 1 | 1 | 1 | 3 | 3 | LOW | Introduction of strict authentication such as mTLS (mutual TLS) within the internal network and between microservices. |
| V8 | (TB2-T) Messages between agents (reservation data and instructions) are tampered with | 1 | 1 | 1 | 3 | 3 | LOW | Message signing for inter-agent communication and establishment of an encrypted communication channel. |
| V9 | (TB2-R) Communication logs between agents are insufficient, so the origin of an unauthorized instruction cannot be identified | 2 | 2 | 1 | 1 | 1.6 | LOW | Recording of distributed tracing logs of inter-agent communication using trace IDs and the like. |
| V10 | (TB2-I) Leakage of Payment data from shared memory or unencrypted internal communication | 1 | 1 | 1 | 3 | 3 | LOW | Appropriate erasure of confidential information in memory and encryption in internal storage and communication. |
| V11 | (TB2-D) Exhaustion of internal resources due to excessive task submission from the Travel Planning Agent to other Agents | 2 | 2 | 2 | 2 | 4 | MID | Introduction of task queueing for agents and limits on the number of concurrent processes and on timeouts. |
| V12 | (TB2-E) The Payment Agent can execute unauthorized payment processing on the instruction of the Travel Planning Agent alone | 2 | 2 | 1 | 3 | 5 | MID | A design that requires the user's explicit approval (Human-in-the-Loop) for privileged processing such as payment. |
| V13 | (TB3-S) An API key hard-coded in the source code or elsewhere leaks, and the external API is used illicitly | 2 | 2 | 2 | 3 | 6 | MID | Dynamic retrieval of keys using a secret manager and regular rotation. |
| V14 | (TB3-T) A fraudulent response from the external API is accepted without validation, and data inside the system is poisoned | 2 | 2 | 1 | 2 | 3.3 | MID | Implementation of strict schema validation and sanitization of data obtained from the external API. |
| V15 | (TB3-R) Failures and errors in external API calls are not recorded, so where responsibility lies at the time of an incident becomes unclear | 2 | 2 | 1 | 1 | 1.6 | LOW | Collection of audit logs of external API call requests, responses, and status codes. |
| V16 | (TB3-I) Confidential information such as Payment data remains in the request that is sent and is transmitted externally | 2 | 2 | 1 | 3 | 5 | MID | Introduction of a data loss prevention (DLP) mechanism that automatically masks or removes PII and payment information before transmission. |
| V17 | (TB3-D) A delay in or halt of the external API's response causes the AITravelApp process to hang | 2 | 3 | 3 | 2 | 5.3 | MID | Implementation of the circuit breaker pattern for external API calls and appropriate timeout settings. |
| V18 | (TB3-E) If a single API key leaks, all the integrated external API services become operable | 2 | 2 | 1 | 3 | 5 | MID | Issue and use a separate API key with least privilege for each external API provider. |

LOW: <3, MID: 4<=6, HIGH: 7<=9
