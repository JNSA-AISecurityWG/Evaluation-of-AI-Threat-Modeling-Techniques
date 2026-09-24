# Evaluation of Threat Modeling Techniques for Systems That Use AI
[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

This document is published under the [Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg

## Overview
In recent years, with the proliferation of systems that use AI, it has become increasingly important to understand both the threats specific to AI and the threats to the systems on which AI operates. In this document, we address three AI usage patterns: applications with built-in AI functionality, applications using an external LLM, and applications using agentic AI. For these, we apply three threat modeling techniques—STRIDE, STRIDE+AI, and MAESTRO—and discuss the advantages and disadvantages of each.

The threat modeling was carried out by three teams, each consisting of three members, and a total of nine modeling sessions were performed. This document also publishes information on the threat models addressed and their results. We hope that this information will serve as a reference when conducting threat modeling.


## Table of Contents
1. [Introduction](/section/1_introduction.en.md)
2. [Threat Modeling Techniques](/section/2_threat-modeling-technique.en.md)
* STRIDE
* STRIDE+AI
* MAESTRO
3. [Target Threat Models](/section/3_threat-model.en.md)
* Application with Built-in AI Functionality
* Application Using an External LLM
* Application Using Agentic AI
4. [Evaluation Method](/section/4_evaluation_method.en.md)
5. [Evaluation Results](/section/5_result.en.md)
6. [Discussion](/section/6_discussion.en.md)
7. [Conclusion](/section/7_conclusion.en.md)

## Project Members
This document has been created by the members of the NPO Japan Network Security Association (JNSA) – Research Division – AI Security Working Group. The project members are as follows.

### Project Member List (in no particular order)
#### Working Group Leader
* 服部 祐一 (株式会社セキュアサイクル, Leader, JNSA – Research Division – AI Security Working Group)

#### Working Group Members (in Japanese alphabetical order)
* 安達 康平(株式会社セキュアサイクル)
* 五十嵐 裕(株式会社ギブリー)
* 砂金 善弘(株式会社セキュアサイクル)
* 伊東 道明(株式会社ChillStack)
* 榎本 祐樹(フューチャーセキュアウェイブ株式会社)
* 倉地 伸明(富士ソフト株式会社)
* 庄司 勝哉(株式会社ラック)
* 野田 俊夫(アドソル日進株式会社)
* 濵村 遼成(株式会社セキュアサイクル)
* 松永 昌浩(セコム株式会社)
* 松山 保(株式会社ヌーラボ)

## Related Organizations

### NPO Japan Network Security Association (JNSA)
The NPO Japan Network Security Association is an organization whose purpose is to contribute to the information society by working to maintain and improve the level of information security in the networked society and to raise awareness of information security in Japan, and by providing information on the latest information security technologies and on threats to information security.

[NPO Japan Network Security Association Official Website](https://www.jnsa.org/) 

## License
This document is published under the [Creative Commons Attribution-Share Alike v4.0](https://creativecommons.org/licenses/by-sa/4.0/) license.

## Contact
For questions, improvements, requests, and the like regarding this document, please contact us through the Issues of this GitHub repository.

## 言語 / Languages
- [日本語版 (Japanese)](/README.md)
- [English version](/README.en.md)
