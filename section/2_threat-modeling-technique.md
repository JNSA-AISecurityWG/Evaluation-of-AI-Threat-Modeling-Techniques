# 脅威モデリング手法について
# 対象とする脅威モデル
# 脅威モデリング手法について

## 1. STRIDEとは

STRIDEはMicrosoftが提唱した代表的な脅威モデリングの枠組みであり、Spoofing / Tampering / Repudiation / Information Disclosure / Denial of Service / Elevation of Privilege の6分類で脅威を体系化します。  
本手法は、設計段階で DFD（Data Flow Diagram） を用いてシステムを分解し、信頼境界（Trust Boundary）をまたぐデータの流れやコンポーネント間の相互作用に着目して脅威を洗い出します。

## 2. STRIDEの基本概念

STRIDEは「何を守るか（セキュリティ特性）」と「どう脅かされるか（脅威）」を対応させて考える枠組みです。ポイントは、STRIDEが「脅威カテゴリのチェックリスト」ではなく、DFD・信頼境界・資産を起点にした分析手順という点です。

### 2.1 STRIDEの脅威モデル

| 脅威カテゴリ                       | 対応するセキュリティ特性                |
| :--- | :--- |
| S: Spoofing（なりすまし）               | 認証の破壊（Authentication）       |
| T: Tampering（改ざん）                | 完全性の破壊（Integrity）           |
| R: Repudiation（否認）               | 証跡／責任追跡の破壊（Non-repudiation） |
| I: Information Disclosure（情報漏えい） | 機密性の破壊（Confidentiality）     |
| D: Denial of Service（サービス拒否）     | 可用性の破壊（Availability）        |
| E: Elevation of Privilege（権限昇格）  | 認可の破壊（Authorization）        |

### 2.2 DFD要素

| DFD要素    | 論点になりやすい脅威カテゴリ          |
| -------- | --------------------------- |
| 外部エンティティ | S（なりすまし）起点になりやすい            |
| プロセス     | T/R/E（改ざん・否認・権限昇格）が論点になりやすい |
| データストア   | T/I（改ざん・漏えい）が論点になりやすい       |
| データフロー   | T/I/D（改ざん・漏えい・DoS）を受けやすい    |

### 2.3 TB

TB（Trust Boundary）は「同一の信頼前提（運用責任・権限・到達可能性・検証手段）が成り立つ範囲の境界」であり、TBをまたぐ箇所は、信頼前提（認証・認可・検証・暗号化・監査）が切り替わるため、攻撃が成立しやすい論点が集中します。
TBを明示することで、脅威列挙が「コンポーネント単体」から「境界をまたいで発生する脅威の洗い出し」の視点にシフトします。

## 3. 方法：STRIDEによる脅威モデリング手順

### 3.1 手順0：スコープと前提の合意

- 対象システムの範囲（境界条件、除外範囲）
- 守るべき資産（例：機密データ、重要業務、ログ、コスト）
- 想定攻撃者（外部／内部、到達可能性）
- リスク受容基準（重大度×起こりやすさ、または社内基準）

### 3.2 手順1：アーキテクチャ図／DFD作成

- Context Diagram（外部システム・利用者・信頼境界）
- Level 0/1 DFD（主要プロセス、データストア、データフロー）
- Trust Boundaryの明示（ネットワーク境界、テナント境界、権限境界）

### 3.3 手順2：脅威列挙（STRIDE）

- Trust Boundary（TB）をまたぐデータフロー／相互作用に対して、脅威を箇条書きで洗い出す
- 必要に応じて、TB内の要素（プロセス／データストア等）に対しても補足的に適用する
- 洗い出した脅威を、評価と対策検討に使える攻撃事例として整理する

### 3.4 手順3：リスク評価と優先順位付け

OWASP Risk Rating（簡易版）を用いて、各脆弱性に対して以下の3つの可能性要因を評価し、技術的影響を含めリスクを判定する。

- 攻撃ベクトルの悪用容易性（Ease of Exploit）
- 脆弱性の普及度（Awareness / Ease of Discovery）
- 検知可能性（Intrusion Detection）
- 技術的影響（Technical Impact）

OWASP Risk Rating（簡易版）による技術面の優先順位付けに加え、ビジネス観点（例：経済的損害、風評、コンプライアンス、プライバシー）で影響を評価し、対応優先度の最終調整に用いる。
Impact/Likelihood など評価方法は組織により異なるが、この研究では OWASP Risk Rating（簡易版）を用いる。

### 3.5 手順4：対策設計（Mitigation）

リスク評価で抽出されたすべての脆弱性に対して「既に実施されている対策」と「追加で必要な対策」を切り分けて整理する。
目的は、すべての脅威をカバーすることであり、完全にカバーされていない脅威を優先的に扱う。

対策方針としては
- 設計の見直しにより脅威を排除する（攻撃面そのものを消す／境界を再設計する 等）
- 標準的な緩和策を適用する（既知のベストプラクティスを当てはめる）
- 新しい緩和策を作成する（システム固有の事情に合わせて追加設計する）
- 設計上の脆弱性を容認する（リスク受容）（受容理由・前提・残存リスクを明記する）
- 既存対策（Mitigations）を特定し、未対策・不十分な箇所を脆弱性（Vulnerabilities）として整理する


## 参考文献

1.  Microsoft Learn Archive MSDN Magazine, Nov 2006. “Threat Modeling: Uncover Security Design Flaws Using The STRIDE Approach”
   https://learn.microsoft.com/en-us/archive/msdn-magazine/2006/november/uncover-security-design-flaws-using-the-stride-approach

2. Security Compass / Software Secured “What is STRIDE in Threat Modeling?”
   https://www.securitycompass.com/blog/stride-in-threat-modeling

3. OWASP “Threat Modeling Process”  
   https://owasp.org/www-community/Threat_Modeling_Process

4. OWASP Risk Rating Methodology  
   https://owasp.org/www-community/OWASP_Risk_Rating_Methodology


