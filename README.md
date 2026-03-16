# AI脅威モデリング手法の評価
[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

本ドキュメントは、[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa] ライセンスのもとに公開しています。

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg

## 概要
近年、AIを利用したシステムの普及に伴い、AI特有の脅威や、それらが動作するシステムに対する脅威を把握する重要性が高まっています。本ドキュメントでは、AIの利用パターンとして、内部にAI機能を有するアプリケーション、外部のLLMを用いたアプリケーション、エージェント型AIを用いたアプリケーションの3種類を対象としています。これらに対して、STRIDE、STRIDE+AI、MAESTROの3つの脅威モデリング手法を適用し、それぞれのメリットおよびデメリットについて議論します。

脅威モデリングは、3名で構成された3チームで実施し、合計9回のモデリングを行いました。また、本ドキュメントでは、対象とした脅威モデルの情報とその結果についても公開しています。これらの情報が、脅威モデリングを実施する際の参考となることを期待しています。


## 章立て
1. [はじめに](/section/1_introduction.md)
2. [脅威モデリング手法について](/section/2_threat-modeling-technique.md)
* STRIDE
* STRIDE+AI
* MAESTRO
3. [対象とする脅威モデル](/section/3_threat-model.md)
* 内部にAI機能を有するアプリケーション
* 外部のLLMを用いたアプリケーション
* エージェント型AIを用いたアプリケーション
4. [評価手法](/section/4_evaluation_method.md)
5. [評価結果](/section/5_result.md)
6. [議論](/section/6_discussion.md)
7. [まとめ](/section/7_conclusion.md)

## プロジェクトメンバー
本ドキュメントは、NPO日本ネットワークセキュリティ協会 調査研究部会 AIセキュリティWGのメンバーによって作成されています。プロジェクトメンバーは以下の通りです。

### プロジェクトメンバー一覧(順不同)
* ○○ ○○(△△)

## 関連組織について

### NPO日本ネットワークセキュリティ協会（JNSA）
NPO日本ネットワークセキュリティ協会は、ネットワーク社会の情報セキュリティレベルの維持・向上及び日本における情報セキュリティ意識の啓発に努めるとともに、最新の情報セキュリティ技術および情報セキュリティへの脅威に関する情報提供などを行うことで、情報化社会へ貢献することを目的とした組織です。

[NPO日本ネットワークセキュリティ協会 公式サイト](https://www.jnsa.org/) 

## ライセンス
本ドキュメントは [Creative Commons Attribution-Share Alike v4.0](https://creativecommons.org/licenses/by-sa/4.0/) ライセンスのもとに公開しています。

## お問い合わせ先
本ドキュメントに関する質問、改善や要望等は、本GitHubリポジトリのIssueにてご連絡ください。