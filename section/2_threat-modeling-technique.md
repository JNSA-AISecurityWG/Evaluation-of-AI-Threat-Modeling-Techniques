# 脅威モデリング手法について
本ドキュメントでは、STRIDE、STRIDE+AI、MAESTROの3つの脅威モデリング手法を対象とし、脅威モデリングを実施しました。本章では、各手法について説明します。


## STRIDE
### 1. STRIDEとは

STRIDEはMicrosoftが提唱した代表的な脅威モデリングの枠組みであり、Spoofing / Tampering / Repudiation / Information Disclosure / Denial of Service / Elevation of Privilege の6分類で脅威を体系化します。  
本手法は、設計段階で DFD（Data Flow Diagram） を用いてシステムを分解し、信頼境界（Trust Boundary）をまたぐデータの流れやコンポーネント間の相互作用に着目して脅威を洗い出します。

### 2. STRIDEの基本概念

STRIDEは「何を守るか（セキュリティ特性）」と「どう脅かされるか（脅威）」を対応させて考える枠組みです。ポイントは、STRIDEが「脅威カテゴリのチェックリスト」ではなく、DFD・信頼境界・資産を起点にした分析手順という点です。

#### 2.1 STRIDEの脅威モデル

| 脅威カテゴリ                       | 対応するセキュリティ特性                |
| :--- | :--- |
| S: Spoofing（なりすまし）               | 認証の破壊（Authentication）       |
| T: Tampering（改ざん）                | 完全性の破壊（Integrity）           |
| R: Repudiation（否認）               | 証跡／責任追跡の破壊（Non-repudiation） |
| I: Information Disclosure（情報漏えい） | 機密性の破壊（Confidentiality）     |
| D: Denial of Service（サービス拒否）     | 可用性の破壊（Availability）        |
| E: Elevation of Privilege（権限昇格）  | 認可の破壊（Authorization）        |

#### 2.2 DFD要素

| DFD要素    | 論点になりやすい脅威カテゴリ          |
| -------- | --------------------------- |
| 外部エンティティ | S（なりすまし）起点になりやすい            |
| プロセス     | T/R/E（改ざん・否認・権限昇格）が論点になりやすい |
| データストア   | T/I（改ざん・漏えい）が論点になりやすい       |
| データフロー   | T/I/D（改ざん・漏えい・DoS）を受けやすい    |

#### 2.3 信頼境界

信頼境界は「同一の信頼前提（運用責任・権限・到達可能性・検証手段）が成り立つ範囲の境界」であり、信頼境界をまたぐ箇所は、信頼前提（認証・認可・検証・暗号化・監査）が切り替わるため、攻撃が成立しやすい論点が集中します。
信頼境界を明示することで、脅威列挙が「コンポーネント単体」から「境界をまたいで発生する脅威の洗い出し」の視点にシフトします。

### 3. 方法：STRIDEによる脅威モデリング手順

#### 3.1 手順0：スコープと前提の合意

- 対象システムの範囲（境界条件、除外範囲）
- 守るべき資産（例：機密データ、重要業務、ログ、コスト）
- 想定攻撃者（外部／内部、到達可能性）
- リスク受容基準（重大度×起こりやすさ、または社内基準）

#### 3.2 手順1：アーキテクチャ図／DFD作成

- Context Diagram（外部システム・利用者・信頼境界）
- Level 0/1 DFD（主要プロセス、データストア、データフロー）
- 信頼境界の明示（ネットワーク境界、テナント境界、権限境界）

#### 3.3 手順2：脅威列挙

- 信頼境界をまたぐデータフロー／相互作用に対して、脅威を箇条書きで洗い出す
- 必要に応じて、信頼境界内の要素（プロセス／データストア等）に対しても補足的に適用する
- 洗い出した脅威を、評価と対策検討に使える攻撃事例として整理する

#### 3.4 手順3：リスク評価と優先順位付け

OWASP Risk Rating（簡易版）を用いて、各脆弱性に対して以下の3つの可能性要因を評価し、技術的影響を含めリスクを判定する。

- 攻撃ベクトルの悪用容易性（Ease of Exploit）
- 脆弱性の普及度（Awareness / Ease of Discovery）
- 検知可能性（Intrusion Detection）
- 技術的影響（Technical Impact）

OWASP Risk Rating（簡易版）による技術面の優先順位付けに加え、ビジネス観点（例：経済的損害、風評、コンプライアンス、プライバシー）で影響を評価し、対応優先度の最終調整に用いる。
Impact/Likelihood など評価方法は組織により異なるが、この研究では OWASP Risk Rating（簡易版）を用いる。

#### 3.5 手順4：対策設計

リスク評価で抽出されたすべての脆弱性に対して「既に実施されている対策」と「追加で必要な対策」を切り分けて整理する。
目的は、すべての脅威をカバーすることであり、完全にカバーされていない脅威を優先的に扱う。

対策方針としては
- 設計の見直しにより脅威を排除する（攻撃面そのものを消す／境界を再設計する 等）
- 標準的な緩和策を適用する（既知のベストプラクティスを当てはめる）
- 新しい緩和策を作成する（システム固有の事情に合わせて追加設計する）
- 設計上の脆弱性を容認する（リスク受容）（受容理由・前提・残存リスクを明記する）
- 既存対策（Mitigations）を特定し、未対策・不十分な箇所を脆弱性（Vulnerabilities）として整理する


### 参考文献

1. Microsoft Corporation, Uncover Security Design Flaws Using The STRIDE Approach | Microsoft Learn, https://learn.microsoft.com/en-us/archive/msdn-magazine/2006/november/uncover-security-design-flaws-using-the-stride-approach (2026年3月28日 取得)
2. Security Compass., What is STRIDE in Threat Modeling? - Security Compass, https://www.securitycompass.com/blog/stride-in-threat-modeling (2026年3月28日 取得)
3. OWASP Foundation, Inc., Threat Modeling Process | OWASP Foundation, https://owasp.org/www-community/Threat_Modeling_Process (2026年3月28日 取得)
4. OWASP Foundation, Inc., OWASP Risk Rating Methodology | OWASP Foundation, https://owasp.org/www-community/OWASP_Risk_Rating_Methodology (2026年3月28日 取得)
---

## STRIDE+AI

### STRIDEをAIシステムに適用するとは

STRIDEは、Microsoftによって提案された脅威モデリングの枠組みですが、近年、業務システムにAIや機械学習（ML）が組み込まれるようになったことで、従来のSTRIDEでは捉えにくい脅威が顕在化しています。

具体的には、攻撃対象がコードだけでなく、学習データや学習済みモデル、推論時の入力データなどに広がっています。

こうした背景から、既存のSTRIDEを前提としつつ、AI/MLシステムの特性を考慮して解釈・適用する考え方が、 学術研究[1][2][3]や、実務向けの整理・トレーニング[4][5]の両面で議論されるようになっています。

本稿では、便宜的にこの考え方を **STRIDE+AI** と呼び、STRIDEをAI/MLシステムに適用するために拡張する考え方と代表的な対策について説明します。

### STRIDE+AIにおける脅威カテゴリとAI向け適用例

| 脅威カテゴリ | AI/MLシステムでの解釈 | 具体的な事例 | 対策例|
| :--- | :--- | :--- | :--- |
| **S: Spoofing**<br>なりすまし | 攻撃者がディープフェイクなどにより正規のユーザを装いシステムの認証を回避したり、細工をほどこしたデータをシステムに入力する。| 犯罪者がAIを用いてCEOの声を模倣し、電話で数十万ドル規模の偽の送金指示を出した詐欺の事例や、研究・実証実験としてTeslaの画像認識AIに速度制限標識「35」を「85」と誤認させた事例。 | 入力元・API利用者の認証強化（多要素認証等）、データソースの真正性検証、異常検知によるなりすましの検出など |
| **T: Tampering**<br>改ざん | 学習データやモデルの不正な変更により、推論結果や振る舞いが意図的に歪められる。| MicrosoftのTwitterボット「Tay」がユーザによる悪意ある入力で差別的発言を学習し、サービス停止に追い込まれた事例。 | 信頼できるデータソースの確保、学習データの管理・検証、モデルの完全性保護（署名・検証）、性能の継続的監視など |
| **R: Repudiation**<br>否認 | AIの判断や操作について、後から責任の所在を否認できてしまう状態。| 英国での親権を争う裁判で、夫を陥れる目的でディープフェイク音声が使用され、夫が必死に否認せざるを得なかった事例。（この事例では、録音データのメタデータを分析することで偽の音声データであることが判明した。） | 改ざん耐性のある監査ログ、モデル出力への署名や透かし付与、操作主体との紐付けによる説明責任の確保など |
| **I: Information Disclosure**<br>情報漏えい | 学習済みモデルや推論結果を通じて、訓練データや機密情報が推測・漏洩するリスク。| 生成AIに対して特定のプロンプトを与えることで、学習データから個人情報（氏名、電話番号など）の学習データ由来と考えられる情報が出力されたと報告された事例。 | 学習時の機密性確保（差分プライバシー等）、出力制御とフィルタリング、モデルやデータへのアクセス制御など |
| **D: Denial of Service**<br>サービス拒否 | 大量のリクエストや負荷の高いリクエストにより、推論が継続できなくなる状態。| 生成AIに対して極端に大きな出力を要求するプロンプトを送信し、計算資源を浪費させる攻撃。実際に巧妙につくられた入力により、単一リクエストで想定外の高コストが発生した事例。 | レート制限、入力制約、リソース使用量の監視、負荷時の機能制限などの可用性確保策など |
| **E: Elevation of Privilege**<br>権限昇格 | AIを介して、本来許可されていない操作や権限を取得される。| 生成AIへの「DAN（Do Anything Now）」プロンプトにより、モデルの安全制約をバイパスして不適切な出力が生成された事例。 | 最小権限設計、実行環境の分離（サンドボックス化）、入力・指示内容の検証と制御など |

### 特徴的な分析視点：ライフサイクル全体への着目

AIシステムでは、脅威が運用時だけでなく、開発・学習・更新といった各段階に分散します。

- **学習前**：データ収集・前処理時の偏りや混入  
- **学習中**：学習データやハイパーパラメータの操作  
- **運用中**：推論APIへの悪用入力、情報漏えい  
- **運用後**：性能劣化や判断ミスに対する責任所在  

STRIDEをAIに適用する際は、 **「いつ・どの段階で・どの資産が脅かされるか」** を意識して分析することが、 従来のアプリケーション脅威モデリングとは異なる重要な観点となります。

### 参考資料
1. IEEE, STRIDE-AI: An Approach to Identifying Vulnerabilities of Machine Learning Assets | IEEE Conference Publication | IEEE Xplore, https://ieeexplore.ieee.org/document/9527917 (2026年3月28日 取得)

2. MDPI, Modeling Threats to AI-ML Systems Using STRIDE, https://www.mdpi.com/1424-8220/22/17/6662 (2026年3月28日 取得)

3. Well Testing, Extending STRIDE and MITRE ATLAS for AI-Specific Threat Landscapes | Well Testing Journal, https://welltestingjournal.com/index.php/WT/article/view/213 (2026年3月28日 取得)

4. Toreon BV, Mind the Gap: STRIDE-AI - Your Clear Path to Understanding AI Vulnerabilities, https://www.toreon.com/stride-ai-your-clear-path-to-understanding-ai-vulnerabilities/ (2026年3月28日 取得)

5. IEEE, Tutorial #1: Modeling Threats to ML Models with STRIDE-AI &#8211; 2025 IEEE 9th Forum on Research and Technologies for Society and Industry Innovation (RTSI), https://2025.ieee-rtsi.org/modeling-threats-to-ml-models-with-stride-ai/ (2026年3月28日 取得)

---

## MAESTRO
### MAESTROとは
MAESTRO（Multi-Agent Environment, Security, Threat, Risk, and Outcome）は、CSA(Cloud Security Alliance)のKen Huang[1]によって提唱されたマルチエージェントシステム（MAS）に特化した脅威モデリング・フレームワークです。

自律的なエージェントが相互に連携するシステムは、単一のAIモデルよりも複雑な攻撃対象を持ちます。
MAESTROは、この複雑さを構造化し、エージェント特有の脆弱性を特定するために設計されました。

### 6つの主要原則

MAESTROは、エージェント型AI特有の課題に対応するため、以下の原則に基づいて構築されています。

1. 拡張されたセキュリティカテゴリ:
   STRIDE、PASTA、LINDDUNといった伝統的なセキュリティカテゴリに、AI固有の考慮事項を組み込んで拡張します。

2. マルチエージェントと環境への焦点:
   単体のエージェントだけでなく、エージェント間の相互作用や、エージェントとそれを取り巻く環境との関わりを考慮します。

3. 階層化されたセキュリティ:
   セキュリティを単一の層としてではなく、エージェント・アーキテクチャの各層すべてに組み込まれるべき属性として扱います。

4. AI固有の脅威への対応:
   特に敵対的機械学習（Adversarial ML）や、エージェントの自律性に起因するリスクなど、AI特有の脅威に対処します。

5. リスクベースのアプローチ:
   エージェントが置かれた文脈（コンテキスト）における発生可能性と影響度に基づき、脅威の優先順位付けを行います。

6. 継続的な監視と適応:
   絶えず進化するAI技術と脅威に対応するため、継続的なモニタリング、脅威インテリジェンスの活用、およびモデルの更新をプロセスに組み込みます。

### Agentic AI Threats and Mitigationについて

MAESTRO を活用するにあたり、OWASP の Agentic Security Initiative（ASI）が公開した 「Agentic AI – Threats and Mitigations」が重要な参照資料となります。ASI の分類（脅威分類）を脅威モデリングにおいてより深く扱う手段として MAESTROフレームワークの利用を推奨しています。


#### Agentic AI Threats and Mitigation の脅威分類

「Agentic AI – Threats and Mitigations」では、脅威を5つのカテゴリに整理しています。

| カテゴリ | 説明 | 主な論点 |
| :--- | :--- | :--- |
| **Agent Design**（エージェント設計） | システムアーキテクチャや設計パターンに起因する脆弱性 | プロンプト設計、境界の定義、入力検証 |
| **Agent Memory**（エージェントメモリ） | データの保存・検索機構に伴うリスク | メモリの汚染、コンテキスト操作、永続化データの改ざん |
| **Planning & Autonomy**（計画と自律性） | 意思決定や自律的行動に対する脅威 | 意思決定ループの悪用、目標のすり替え、自律実行の制御喪失 |
| **Tool Use**（ツール利用） | 外部APIやツールへのアクセスに伴うリスク | ツールの不正利用、権限の逸脱、インジェクション経路 |
| **Deployment & Operations**（デプロイと運用） | 実行時・運用時のセキュリティ | 運用環境の設定ミス、監視の欠如、インシデント対応 |

#### 17の詳細脅威（Detailed Threat Model, T1–T17）

当該文書の Detailed Threat Model では、上記5カテゴリに基づきT1からT17までの17個の脅威が定義されています。
各脅威には、想定される対策、影響を受けるコンポーネント、攻撃パターン例、リスクの説明が含まれます。

4章では、MAESTRO の手順のうち「DFD に対する脅威の特定」のステップで、この T1–T17 の詳細脅威モデルを適用しています。
つまり、MAESTRO の7レイヤーによる分解とレイヤー別・クロスレイヤー脅威の分析に、ASIの17脅威を組み合わせることで、エージェント型AIシステムの脅威を具体的に洗い出しています。

以下に、Agentic AI – Threats and Mitigations（Version 1.1）の Detailed Threat Model で定義されるT1–T17の一覧を示します。

| 識別子 | 脅威名（英語） | 概要 |
| :--- | :--- | :--- |
| **T1** | Memory Poisoning（メモリポイズニング） | AIの短期・長期メモリを悪用し、悪意あるまたは虚偽のデータを注入してエージェントのコンテキストを操作し、意思決定の改変や不正な操作を招く |
| **T2** | Tool Misuse（ツールの悪用） | 攻撃者がプロンプトや指示でエージェントを操作し、認可された範囲内で統合ツールを悪用する。Agent Hijacking（敵対的に操作されたデータを取り込み、意図しない動作や悪意あるツール呼び出しを引き起こす）を含む |
| **T3** | Privilege Compromise（権限の侵害） | 権限管理の弱点（動的なロール継承や設定ミスなど）を突き、不正な操作を行う |
| **T4** | Resource Overload（リソース過負荷） | AIシステムの計算・メモリ・サービス能力を意図的に枯渇させ、性能劣化や障害を引き起こす |
| **T5** | Cascading Hallucination Attacks（カスケード型ハルシネーション攻撃） | 文脈上もっともらしいが虚偽の情報を生成する傾向を悪用し、その情報がシステム内で伝播・増幅して意思決定を撹乱する。破壊的推論やツール呼び出しへの影響も含む |
| **T6** | Intent Breaking & Goal Manipulation（意図破壊・目標操作） | エージェントの計画・目標設定の脆弱性を突き、目標や推論を操作・転用する。Agent Hijacking などの手法を含む |
| **T7** | Misaligned & Deceptive Behaviors（不整合・欺瞞的行動） | エージェントが欺瞞的推論や目標の誤解により、有害・禁止された行動を実行する。悪意ある入力がなくても自律的に不整合な戦略が生じ、意図しない結果を招く場合がある |
| **T8** | Repudiation & Untraceability（否認・追跡不能） | ログや意思決定の透明性不足により、エージェントの行動を追跡・説明できず、責任の所在が不明になる |
| **T9** | Identity Spoofing & Impersonation / Agent Identity Compromise（なりすまし・エージェントアイデンティティ侵害） | 認証機構を悪用し、エージェントやユーザになりすまして不正な行動を行う。永続的なエージェントIDの窃取・悪用により、ガードレールを迂回した特権的APIアクセスが可能になる場合を含む |
| **T10** | Overwhelming Human in the Loop（ヒューマンインザループの圧倒） | 人間の監督・意思確認を備えたシステムに対し、人間の認知的限界やインタラクション枠組みの弱点を突いて攻撃する |
| **T11** | Unexpected RCE and Code Attacks（予期しないRCE・コード攻撃） | AIが生成した実行環境を悪用し、悪意あるコードを注入、意図しない動作を引き起こす、または不正なスクリプトを実行する |
| **T12** | Agent Communication Poisoning（エージェント間通信の汚染） | エージェント間の通信経路を操作し、虚偽情報の拡散、ワークフローの妨害、意思決定への不当な影響を行う |
| **T13** | Rogue Agents in Multi-Agent Systems（マルチエージェントシステムにおける不正エージェント） | 悪意あるまたは侵害されたエージェントが通常の監視外で動作し、不正な操作やデータ流出を行う。「感染性バックドア」のように、1つの侵害エージェントが他に悪意あるロジックを広める概念を含む |
| **T14** | Human Attacks on Multi-Agent Systems（マルチエージェントへの人間による攻撃） | 攻撃者がエージェント間の委譲・信頼関係・ワークフロー依存を悪用し、権限の昇格やAI駆動オペレーションの操作を行う |
| **T15** | Human Manipulation（人間の操作） | エージェントがユーザと直接対話する場面で、信頼関係によりユーザの懐疑が弱まり、攻撃者がエージェントを通じてユーザを操作・誤情報を拡散・密かな行動を取るリスク |
| **T16** | Insecure Inter-Agent Protocol Abuse（安全でないエージェント間プロトコルの悪用） | MCPやA2Aなどのプロトコルの欠陥（同意バイパスやコンテキストハイジャックなど）を突き、不正なエージェント動作を引き起こす |
| **T17** | Supply Chain Compromise（サプライチェーン侵害） | 侵害されたサプライチェーンにより、脆弱・悪意・旧版その他の有害なコンポーネントがエージェントに組み込まれ、攻撃者がエージェントの動作を操作・データを取得・任意コード実行を行う。モデル、ライブラリ、ツール、ビルド環境などの侵害経路を含む |

### MAESTROの7レイヤーについて
MAESTROは、AIエージェントのシステムを7つのレイヤーに切り分けて分析することができます。
以下に、主な7レイヤーの名称と焦点と脅威例をまとめます。

| レイヤー | 名称 | 焦点と主な脅威例 | ASI脅威（T1–T17） |
| :--- | :--- | :--- | :--- |
| **Layer 1** | **基盤モデル**(Foundation Models) | 基盤モデルの完全性、モデルの盗用、学習データの汚染 | T1（メモリが学習に用いられる場合）, T7 |
| **Layer 2** | **データ操作**(Data Operations) | RAG（検索拡張生成）の汚染、ベクターストアの整合性、データ漏洩 | T1, T12 |
| **Layer 3** | **エージェントフレームワーク**(Agent Framework) | 自律的な実行ロジック、ワークフローのハイジャック、プロンプトインジェクション | T2, T5, T6 |
| **Layer 4** | **デプロイとインフラ**(Deployment and Infrastructure) | デプロイ環境の脆弱性、エージェントによるリソース枯渇（DoS） | T3, T4, T13, T14 |
| **Layer 5** | **評価と可観測性**(Evaluation and Observability) | モニタリングの回避、ログの改ざん、評価指標の操作 | T8, T10 |
| **Layer 6** | **セキュリティとコンプライアンス**(Security & Compliance) | 権限昇格、ガードレールのバイパス、ガバナンス違反 | T3, T7 |
| **Layer 7** | **エージェントエコシステム**(Agent Ecosystem) | 外部ツール・人間・他エージェントとの相互作用における信頼の欠如 | T9, T13, T14, T15 |



### クロスレイヤー脅威
MAESTROの特徴として、単一のレイヤーだけでなく、レイヤー間をまたぐ脅威を可視化できます。
具体的なクロスレイヤーの脅威名と具体的な例をまとめます。

| 脅威名 | 説明 | 具体的な例 |
| :--- | :--- | :--- |
| **サプライチェーン攻撃**(Supply Chain Attacks) | ある層のコンポーネントを侵害し、他の層に影響を及ぼす攻撃する | L3（フレームワーク）のライブラリを侵害し、最終的にL7（エージェントエコシステム）に影響を与える |
| **ラテラルムーブメント**(Lateral Movement) | 攻撃者が1つの層へのアクセス権を得た後、そのアクセスを利用して他の層を侵害する | L4（インフラ）へのアクセスを得た攻撃者が、L2（データオペレーション）を侵害するために利用する|
| **権限昇格**(Privilege Escalation) | エージェントや攻撃者が特定の層で不正な権限を取得し、それを使って他の層にアクセス・操作する。 | 1つの層で取得した未認可の特権を悪用し、他の層のデータを操作する |
| **データ漏洩**(Data Leakage) | ある層の機密データが、別の層のプロセス等を通じて外部に露出することである。 | 特定の層で保持されている機密情報が、他の層を経由して漏洩する |
| **目標不整合の連鎖**(Goal Misalignment Cascades) | 1つのエージェントで発生した目標の不整合が、相互作用を通じて他のエージェントへ伝播する | L2でのデータポイズニングにより目標が歪んだエージェントが、エコシステム内の他エージェントにその影響を広める |


### 参考資料
1. Cloud Security Alliance., Agentic AI Threat Modeling Framework: MAESTRO | CSA, https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro (2026年3月28日 取得)

2. arXiv, [2508.10043] Securing Agentic AI: Threat Modeling and Risk Analysis for Network Monitoring Agentic AI System, https://arxiv.org/abs/2508.10043 (2026年3月28日 取得)

3. OWASP Foundation, Inc., Agentic AI - OWASP Lists Threats and Mitigations, https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/ (2026年3月28日 取得)
