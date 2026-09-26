# Evaluation Results
Three teams of three members each evaluated the three types of models using the STRIDE, STRIDE+AI, and MAESTRO techniques, and their results were collected and analyzed.

## Evaluation Data Details
The details of the evaluation data are as follows.

### Team 1
* [STRIDE Detailed Results](/section/result/STRIDE_T1.en.md)
* [STRIDE+AI Detailed Results](/section/result/STRIDE_AI_T1.en.md)
* [MAESTRO Detailed Results](/section/result/MAESTRO_T1.en.md)

### Team 2
* [STRIDE Detailed Results](/section/result/STRIDE_T2.en.md)
* [STRIDE+AI Detailed Results](/section/result/STRIDE_AI_T2.en.md)
* [MAESTRO Detailed Results](/section/result/MAESTRO_T2.en.md)

### Team 3

* [STRIDE Detailed Results](/section/result/STRIDE_T3.en.md)
* [STRIDE+AI Detailed Results](/section/result/STRIDE_AI_T3.en.md)
* [MAESTRO Detailed Results](/section/result/MAESTRO_T3.en.md)


## Practitioners' Feedback
The feedback from each team regarding the threat modeling is as follows.

### Team 1
* STRIDE and STRIDE+AI could be carried out in a relatively short time, but MAESTRO took longer than the other techniques.
* There is no single correct answer in analyzing threats specific to AI, and I found it difficult that the results differ between STRIDE and MAESTRO. I came to appreciate the difference in perspective between the techniques, and learned the depth and importance of analysis.
* STRIDE+AI is general-purpose and easy to understand because it is based on conventional security criteria, but I felt that threats specific to AI are easy to overlook. MAESTRO, on the other hand, makes it easy to identify AI-specific threats, but I feel that deep background knowledge of AI is essential. As AI threats grow more complex, I came to appreciate the importance of understanding the strengths and weaknesses of each framework and using the right one in the right place.

### Team 2
* MAESTRO is highly abstract and the learning cost of organizing its premises is large, so I felt that interpretations vary and differences in judgment arise easily.
* AI systems can take various forms, and they are likely to become even more complex in the future. The threat analysis techniques we applied each have strengths and weaknesses, and I felt that the flow of the process needs to be adapted to the form and context of the system. For that reason, I came to appreciate the importance of not treating each technique as fixed, but of using and updating them flexibly according to the context and purpose.
* The drawback of MAESTRO is that it takes time to build a shared understanding among those doing the modeling, but I felt it is better suited to enumerating the threats to an AI system. With STRIDE+AI, evaluating threats was easy, but I felt that it does not fully cover the threats specific to AI agents. I would suggest using MAESTRO for AI agent systems, and STRIDE+AI for a simple AI chat or a non-language deep learning model.

### Team 3
* Because MAESTRO is a threat analysis technique intended for multi-agent systems, applying it to anything other than a multi-agent system naturally puts many of its threats out of scope. Also, my impression was that the threats and mitigations that emerge as a result of applying it are those of the conventional information security framework.
* While I felt that STRIDE and MAESTRO are very dependable techniques for organizing threats, I came to appreciate that the final quality depends far less on the capability of the tool than on the comprehension needed to draw a DFD all the way down to its trust boundaries, and on the literacy of the operational side in deciding how far the organization will go in looking for risk.
* As for MAESTRO, while it can detect threats specific to multi-agent systems, it has difficulty detecting the typical threats specific to the system, so I felt that carrying out MAESTRO after STRIDE is effective

## Analysis
Based on the evaluation data above and interviews with the participants, we compiled the following analysis.
### Existing STRIDE alone cannot sufficiently cover threats specific to AI.
For every team, existing STRIDE alone could evaluate threats to a general system, but had difficulty evaluating threats specific to AI, such as adversarial attacks and data poisoning.

### MAESTRO can evaluate AI-related threats in detail, but has difficulty evaluating the system portion.
MAESTRO can evaluate threats specific to AI in more detail than STRIDE+AI, but has difficulty evaluating the threats to the system on which the AI operates.

### Compared with the other techniques, STRIDE+AI provides evaluation on both fronts: AI threats and threats to the system.
Although STRIDE+AI cannot evaluate threats specific to AI in as much detail as MAESTRO, it can also cover threats to the system, making evaluation from both sides possible.

### MAESTRO allows detailed judgment about AI threats, but requires considerable understanding of AI on the part of those carrying it out.
For MAESTRO, every team undertook the threat modeling after receiving classroom training on AI threats; more knowledge of AI and AI security is required than for the other techniques.

### In each technique, the risks detected are evaluated quantitatively.
In each technique, the detected risks are evaluated quantitatively by a prescribed calculation method.

## Comparison of the Techniques
The comparison of the techniques is summarized in the table below.
| | STRIDE | STRIDE+AI | MAESTRO | 
| ---- | ---- | ---- | ---- |
| Evaluation of threats to AI | x | 〇 | 〇 |
| Evaluation of threats to the system | 〇 | 〇 | x | 
| Quantitative risk evaluation | 〇 | 〇 | 〇 | 
| Ease of carrying out | 〇 | 〇 | x | 
| Time to carry out | 〇 | 〇 | x | 
