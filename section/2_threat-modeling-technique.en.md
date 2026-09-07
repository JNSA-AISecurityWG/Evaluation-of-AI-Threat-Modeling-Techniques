# Threat Modeling Techniques
In this document, we conducted threat modeling using three threat modeling techniques: STRIDE, STRIDE+AI, and MAESTRO. This chapter explains each of these techniques.

## STRIDE
### What Is STRIDE
STRIDE is a representative threat modeling framework proposed by Microsoft. It organizes threats into six categories: Spoofing / Tampering / Repudiation / Information Disclosure / Denial of Service / Elevation of Privilege [1][2].  
This technique decomposes the system at the design stage using a DFD (Data Flow Diagram), and enumerates threats by focusing on data flows that cross trust boundaries and on interactions between components.

### Basic Concepts of STRIDE

STRIDE is a framework for reasoning about "what to protect (security properties)" in correspondence with "how it can be threatened (threats)." The key point is that STRIDE is not a "checklist of threat categories" but an analytical procedure that starts from DFDs, trust boundaries, and assets.

#### The STRIDE Threat Model

| Threat Category                       | Corresponding Security Property                |
| :--- | :--- |
| S: Spoofing               | Loss of Authentication       |
| T: Tampering                | Loss of Integrity           |
| R: Repudiation               | Loss of Non-repudiation |
| I: Information Disclosure | Loss of Confidentiality     |
| D: Denial of Service     | Loss of Availability        |
| E: Elevation of Privilege  | Loss of Authorization        |

#### DFD Elements

| DFD Element    | Threat Categories That Tend to Be at Issue          |
| -------- | --------------------------- |
| External entity | Tends to be the starting point for S (spoofing)            |
| Process     | T/R/E (tampering, repudiation, elevation of privilege) tend to be at issue |
| Data store   | T/I (tampering, information disclosure) tend to be at issue       |
| Data flow   | Susceptible to T/I/D (tampering, information disclosure, DoS)    |

#### Trust Boundaries

A trust boundary is "the boundary of the scope within which the same trust assumptions (operational responsibility, privileges, reachability, and means of verification) hold." Because the trust assumptions (authentication, authorization, verification, encryption, and auditing) switch where a trust boundary is crossed, points at which attacks can succeed are concentrated there.
Making trust boundaries explicit shifts threat enumeration from the perspective of "individual components" to that of "identifying threats that arise across boundaries."

### Method: The STRIDE Threat Modeling Procedure
Here we describe the procedure based on the threat modeling process published by OWASP [3].

#### Step 0: Agree on Scope and Assumptions

- The scope of the target system (boundary conditions and exclusions)
- The assets to be protected (e.g., confidential data, critical business operations, logs, cost)
- The assumed attackers (external / internal, reachability)
- Risk acceptance criteria (severity × likelihood, or internal organizational criteria)

#### Step 1: Create the Architecture Diagram / DFD

- Context Diagram (external systems, users, trust boundaries)
- Level 0/1 DFD (main processes, data stores, data flows)
- Explicit trust boundaries (network boundaries, tenant boundaries, privilege boundaries)

#### Step 2: Threat Enumeration

- For data flows and interactions that cross trust boundaries, enumerate threats as bullet points
- As necessary, apply the same approach supplementarily to elements inside trust boundaries (processes, data stores, etc.)
- Organize the enumerated threats into attack scenarios that can be used for evaluation and for considering mitigations

#### Step 3: Risk Assessment and Prioritization

Using the OWASP Risk Rating (simplified version) [4], evaluate the following three likelihood factors for each vulnerability and determine the risk, including the technical impact.

- Ease of Exploit of the attack vector
- Awareness / Ease of Discovery of the vulnerability
- Intrusion Detection
- Technical Impact

In addition to the technical prioritization from the OWASP Risk Rating (simplified version), evaluate the impact from a business perspective (e.g., financial damage, reputation, compliance, privacy) and use it to make the final adjustment to the response priority.
Evaluation methods such as Impact/Likelihood differ from organization to organization, but in this study we use the OWASP Risk Rating (simplified version).

#### Step 4: Mitigation Design

For every vulnerability identified in the risk assessment, separate and organize "mitigations already in place" and "additional mitigations required."
The goal is to cover all threats, and threats that are not fully covered are addressed with priority.

The mitigation policies are as follows:
- Eliminate the threat by revising the design (removing the attack surface itself, redesigning boundaries, etc.)
- Apply standard mitigations (applying known best practices)
- Create new mitigations (additional design tailored to system-specific circumstances)
- Accept the design vulnerability (risk acceptance) (documenting the reason for acceptance, the assumptions, and the residual risk)
- Identify existing mitigations and organize unaddressed or insufficient areas as vulnerabilities

---

## STRIDE+AI

### What It Means to Apply STRIDE to AI Systems

STRIDE is a threat modeling framework proposed by Microsoft. In recent years, however, as AI and machine learning (ML) have come to be incorporated into business systems, threats that are difficult to capture with conventional STRIDE have come to the surface.

Specifically, the attack surface has expanded beyond code to include training data, trained models, and input data at inference time.

Against this background, the idea of interpreting and applying STRIDE while taking the characteristics of AI/ML systems into account—on the premise of the existing STRIDE—has come to be discussed both in academic research [5][6][7] and in practice-oriented frameworks and training [8][9].

In this document, we refer to this idea as **STRIDE+AI** for convenience, and describe the concepts for extending STRIDE so that it can be applied to AI/ML systems, along with representative mitigations.

### Threat Categories in STRIDE+AI and Examples of Their Application to AI

| Threat Category | Interpretation in AI/ML Systems | Concrete Examples | Example Mitigations |
| :--- | :--- | :--- | :--- |
| **S: Spoofing** | An attacker impersonates a legitimate user through deepfakes or similar means to bypass the system's authentication, or feeds crafted data into the system. | A fraud case in which criminals used AI to imitate a CEO's voice and issued a fake transfer instruction worth hundreds of thousands of dollars over the phone; and, as research and a proof of concept, a case in which Tesla's image recognition AI was made to misread a speed limit sign reading "35" as "85." | Strengthening authentication of input sources and API users (multi-factor authentication, etc.), verifying the authenticity of data sources, and detecting impersonation through anomaly detection |
| **T: Tampering** | Inference results or behavior are intentionally distorted through unauthorized modification of training data or the model. | The case in which Microsoft's Twitter bot "Tay" learned discriminatory statements from malicious user input and had to be taken out of service. | Securing trustworthy data sources, managing and validating training data, protecting model integrity (signing and verification), and continuous performance monitoring |
| **R: Repudiation** | A state in which responsibility for an AI's decisions or operations can later be denied. | A case in a child custody trial in the UK in which deepfake audio was used with the aim of incriminating the husband, forcing him to desperately deny it. (In this case, analyzing the metadata of the recording revealed that the audio data was fake.) | Tamper-resistant audit logs, signing or watermarking model outputs, and ensuring accountability by linking actions to the acting entity |
| **I: Information Disclosure** | The risk that training data or confidential information is inferred or leaked through a trained model or its inference results. | A reported case in which giving a generative AI a specific prompt caused it to output information—such as names and telephone numbers—that was believed to originate from its training data. | Ensuring confidentiality during training (differential privacy, etc.), output control and filtering, and access control for models and data |
| **D: Denial of Service** | A state in which inference can no longer be sustained due to a large volume of requests or requests with a high computational load. | An attack that sends a generative AI a prompt demanding an extremely large output, thereby wasting computational resources. There have been actual cases in which a carefully crafted input caused unexpectedly high cost in a single request. | Availability measures such as rate limiting, input constraints, monitoring of resource usage, and functional restrictions under load |
| **E: Elevation of Privilege** | Operations or privileges that are not originally permitted are obtained by way of the AI. | A case in which a "DAN (Do Anything Now)" prompt to a generative AI bypassed the model's safety constraints and produced inappropriate output. | Least-privilege design, isolation of the execution environment (sandboxing), and validation and control of inputs and instructions |

### A Distinctive Analytical Perspective: Focusing on the Entire Lifecycle

In AI systems, threats are distributed not only across operation but also across the stages of development, training, and updating.

- **Before training**: bias and contamination during data collection and preprocessing  
- **During training**: manipulation of training data and hyperparameters  
- **During operation**: malicious input to inference APIs, information disclosure  
- **After operation**: accountability for performance degradation and erroneous decisions  

When applying STRIDE to AI, analyzing with an awareness of **"when, at which stage, and which asset is threatened"** is an important perspective that differs from conventional application threat modeling.

---

## MAESTRO
### What Is MAESTRO
MAESTRO (Multi-Agent Environment, Security, Threat, Risk, and Outcome) is a threat modeling framework specialized for multi-agent systems (MAS), proposed by Ken Huang [10] of the CSA (Cloud Security Alliance).

Systems in which autonomous agents interact with one another have a more complex attack surface than a single AI model.
MAESTRO was designed to structure this complexity and to identify agent-specific vulnerabilities.

### The Six Core Principles

MAESTRO is built on the following principles in order to address the challenges specific to agentic AI.

1. Extended Security Categories:
   It extends traditional security categories such as STRIDE, PASTA, and LINDDUN by incorporating AI-specific considerations.

2. Multi-Agent and Environment Focus:
   It considers not only individual agents but also the interactions among agents and the relationship between agents and their surrounding environment.

3. Layered Security:
   It treats security not as a single layer but as an attribute that should be built into every layer of the agent architecture.

4. AI-Specific Threats:
   It addresses threats specific to AI, in particular adversarial machine learning (Adversarial ML) and risks arising from the autonomy of agents.

5. Risk-Based Approach:
   It prioritizes threats based on their likelihood and impact in the context in which the agent is placed.

6. Continuous Monitoring and Adaptation:
   To keep up with continuously evolving AI technologies and threats, it incorporates continuous monitoring, the use of threat intelligence, and model updates into the process.

### About Agentic AI – Threats and Mitigations

In making use of MAESTRO, "Agentic AI – Threats and Mitigations," published by OWASP's Agentic Security Initiative (ASI), is an important reference. ASI recommends the use of the MAESTRO framework as a means of dealing with its own classification (threat taxonomy) in greater depth within threat modeling.


#### The Threat Taxonomy of Agentic AI – Threats and Mitigations

"Agentic AI – Threats and Mitigations" organizes threats into five categories.

| Category | Description | Main Points of Concern |
| :--- | :--- | :--- |
| **Agent Design** | Vulnerabilities arising from the system architecture and design patterns | Prompt design, definition of boundaries, input validation |
| **Agent Memory** | Risks associated with mechanisms for storing and retrieving data | Memory poisoning, context manipulation, tampering with persisted data |
| **Planning & Autonomy** | Threats to decision-making and autonomous action | Abuse of the decision-making loop, goal substitution, loss of control over autonomous execution |
| **Tool Use** | Risks associated with access to external APIs and tools | Misuse of tools, exceeding privileges, injection paths |
| **Deployment & Operations** | Security at runtime and in operation | Misconfiguration of the operating environment, lack of monitoring, incident response |

#### The 17 Detailed Threats (Detailed Threat Model, T1–T17)

The Detailed Threat Model in that document defines 17 threats, from T1 to T17, based on the five categories above.
Each threat includes the mitigations envisaged, the affected components, example attack patterns, and a description of the risk.

In Chapter 4, we apply this T1–T17 detailed threat model in the "identifying threats against the DFD" step of the MAESTRO procedure.
That is, by combining ASI's 17 threats with MAESTRO's decomposition into seven layers and its analysis of per-layer and cross-layer threats, we concretely enumerate threats to agentic AI systems.

The following is the list of T1–T17 defined in the Detailed Threat Model of Agentic AI – Threats and Mitigations (Version 1.1) [11].

| Identifier | Threat Name | Overview |
| :--- | :--- | :--- |
| **T1** | Memory Poisoning | Exploiting an AI's short-term and long-term memory to inject malicious or false data, manipulating the agent's context and leading to altered decisions and unauthorized operations |
| **T2** | Tool Misuse | An attacker manipulates the agent through prompts or instructions to misuse integrated tools within the authorized scope. Includes Agent Hijacking (ingesting adversarially manipulated data, causing unintended behavior or malicious tool calls) |
| **T3** | Privilege Compromise | Exploiting weaknesses in privilege management (such as dynamic role inheritance or misconfiguration) to perform unauthorized operations |
| **T4** | Resource Overload | Intentionally exhausting the computational, memory, or service capacity of an AI system, causing performance degradation or failure |
| **T5** | Cascading Hallucination Attacks | Exploiting the tendency to generate contextually plausible but false information, which then propagates and is amplified within the system, disrupting decision-making. Also includes effects on destructive inference and tool calls |
| **T6** | Intent Breaking & Goal Manipulation | Exploiting vulnerabilities in an agent's planning and goal setting to manipulate or repurpose its goals and reasoning. Includes techniques such as Agent Hijacking |
| **T7** | Misaligned & Deceptive Behaviors | An agent performs harmful or prohibited actions due to deceptive reasoning or misinterpretation of goals. Misaligned strategies may arise autonomously even without malicious input, leading to unintended outcomes |
| **T8** | Repudiation & Untraceability | Insufficient transparency of logs and decisions makes an agent's actions impossible to trace or explain, obscuring where responsibility lies |
| **T9** | Identity Spoofing & Impersonation / Agent Identity Compromise | Abusing authentication mechanisms to impersonate agents or users and perform unauthorized actions. Includes cases where theft and abuse of a persistent agent identity enables privileged API access that bypasses guardrails |
| **T10** | Overwhelming Human in the Loop | Attacking systems that include human oversight and confirmation by exploiting the cognitive limits of humans and weaknesses in the interaction framework |
| **T11** | Unexpected RCE and Code Attacks | Abusing an AI-generated execution environment to inject malicious code, cause unintended behavior, or execute unauthorized scripts |
| **T12** | Agent Communication Poisoning | Manipulating communication paths between agents to spread false information, disrupt workflows, and exert undue influence on decision-making |
| **T13** | Rogue Agents in Multi-Agent Systems | A malicious or compromised agent operates outside normal monitoring and performs unauthorized operations or data exfiltration. Includes the concept of an "infectious backdoor," in which one compromised agent spreads malicious logic to others |
| **T14** | Human Attacks on Multi-Agent Systems | An attacker exploits delegation, trust relationships, and workflow dependencies among agents to escalate privileges and manipulate AI-driven operations |
| **T15** | Human Manipulation | In situations where an agent interacts directly with a user, the trust relationship weakens the user's skepticism, creating the risk that an attacker manipulates the user through the agent, spreads misinformation, or takes covert actions |
| **T16** | Insecure Inter-Agent Protocol Abuse | Exploiting flaws in protocols such as MCP and A2A (consent bypass, context hijacking, etc.) to induce unauthorized agent behavior |
| **T17** | Supply Chain Compromise | A compromised supply chain causes vulnerable, malicious, outdated, or otherwise harmful components to be incorporated into an agent, allowing an attacker to manipulate the agent's behavior, obtain data, or execute arbitrary code. Includes compromise paths such as models, libraries, tools, and build environments |

### The Seven Layers of MAESTRO
MAESTRO makes it possible to analyze an AI agent system by dividing it into seven layers.
The names, focus, and example threats of the seven main layers are summarized below.

| Layer | Name | Focus and Main Example Threats | ASI Threats (T1–T17) |
| :--- | :--- | :--- | :--- |
| **Layer 1** | **Foundation Models** | Integrity of the foundation model, model theft, contamination of training data | T1 (when memory is used for training), T7 |
| **Layer 2** | **Data Operations** | Poisoning of RAG (Retrieval-Augmented Generation), integrity of the vector store, data leakage | T1, T12 |
| **Layer 3** | **Agent Framework** | Autonomous execution logic, workflow hijacking, prompt injection | T2, T5, T6 |
| **Layer 4** | **Deployment and Infrastructure** | Vulnerabilities in the deployment environment, resource exhaustion by agents (DoS) | T3, T4, T13, T14 |
| **Layer 5** | **Evaluation and Observability** | Evasion of monitoring, tampering with logs, manipulation of evaluation metrics | T8, T10 |
| **Layer 6** | **Security & Compliance** | Elevation of privilege, bypassing guardrails, governance violations | T3, T7 |
| **Layer 7** | **Agent Ecosystem** | Lack of trust in interactions with external tools, humans, and other agents | T9, T13, T14, T15 |



### Cross-Layer Threats
A distinctive feature of MAESTRO is that it can make visible not only threats within a single layer but also threats that span multiple layers.
The specific cross-layer threat names and concrete examples are summarized below.

| Threat Name | Description | Concrete Example |
| :--- | :--- | :--- |
| **Supply Chain Attacks** | An attack that compromises a component in one layer and affects other layers | Compromising a library in L3 (Framework) and ultimately affecting L7 (Agent Ecosystem) |
| **Lateral Movement** | After gaining access to one layer, an attacker uses that access to compromise other layers | An attacker who has gained access to L4 (Infrastructure) uses it to compromise L2 (Data Operations)|
| **Privilege Escalation** | An agent or attacker obtains unauthorized privileges in a particular layer and uses them to access and operate on other layers. | Abusing unauthorized privileges obtained in one layer to manipulate data in another layer |
| **Data Leakage** | Confidential data in one layer is exposed externally by way of a process or the like in another layer. | Confidential information held in a particular layer leaks by way of another layer |
| **Goal Misalignment Cascades** | A goal misalignment that occurs in one agent propagates to other agents through their interactions | An agent whose goals have been distorted by data poisoning in L2 spreads that effect to other agents in the ecosystem |


### References
1. Microsoft Corporation, Uncover Security Design Flaws Using The STRIDE Approach | Microsoft Learn, https://learn.microsoft.com/en-us/archive/msdn-magazine/2006/november/uncover-security-design-flaws-using-the-stride-approach (retrieved March 28, 2026)
1. Security Compass., What is STRIDE in Threat Modeling? - Security Compass, https://www.securitycompass.com/blog/stride-in-threat-modeling (retrieved March 28, 2026)
1. OWASP Foundation, Inc., Threat Modeling Process | OWASP Foundation, https://owasp.org/www-community/Threat_Modeling_Process (retrieved March 28, 2026)
1. OWASP Foundation, Inc., OWASP Risk Rating Methodology | OWASP Foundation, https://owasp.org/www-community/OWASP_Risk_Rating_Methodology (retrieved March 28, 2026)
1. IEEE, STRIDE-AI: An Approach to Identifying Vulnerabilities of Machine Learning Assets | IEEE Conference Publication | IEEE Xplore, https://ieeexplore.ieee.org/document/9527917 (retrieved March 28, 2026)
1. MDPI, Modeling Threats to AI-ML Systems Using STRIDE, https://www.mdpi.com/1424-8220/22/17/6662 (retrieved March 28, 2026)
1. Well Testing, Extending STRIDE and MITRE ATLAS for AI-Specific Threat Landscapes | Well Testing Journal, https://welltestingjournal.com/index.php/WT/article/view/213 (retrieved March 28, 2026)
1. Toreon BV, Mind the Gap: STRIDE-AI - Your Clear Path to Understanding AI Vulnerabilities, https://www.toreon.com/stride-ai-your-clear-path-to-understanding-ai-vulnerabilities/ (retrieved March 28, 2026)
1. IEEE, Tutorial #1: Modeling Threats to ML Models with STRIDE-AI &#8211; 2025 IEEE 9th Forum on Research and Technologies for Society and Industry Innovation (RTSI), https://2025.ieee-rtsi.org/modeling-threats-to-ml-models-with-stride-ai/ (retrieved March 28, 2026)
1. Cloud Security Alliance., Agentic AI Threat Modeling Framework: MAESTRO | CSA, https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro (retrieved March 28, 2026)
1. OWASP Foundation, Inc., Agentic AI - OWASP Lists Threats and Mitigations, https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/ (retrieved March 28, 2026)
