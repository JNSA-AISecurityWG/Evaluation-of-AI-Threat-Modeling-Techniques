# 評価手法
評価に用いたデータは、3章で挙げた3種類のモデルについて、STRIDE、STRIDE+AI及びMAESTROの手法を用いて脅威モデリングを行った結果を用いました。データの収集においては、
3名1チームの構成で計3チームにおいて、各チームで3種類のモデルに対してSTRIDE、STRIDE+AI及びMAESTROの手法を用いて評価を行いそれらの結果を収集しました。また、データ収集の際の各手法の実施方法については、下記の通りです。

## STRIDE
STRIDEについては、用意されたDFD図に対して下記の流れで実施しました。

1. 信頼境界の設定(3つ)
DFD図に対して信頼境界を3つ設定します。

2. 信頼境界ごとのSTRIDEテーブルの作成
STRIDEテーブルは、STRIDEの各項目ごとに緩和策と脆弱性を検討します。

| | 緩和策 | 脆弱性 |
| ---- | ---- | ---- |
| S | | |
| T | | |
| R | | |
| I | | |
| D | | |
| E | | |

3. リスクの評価と脅威に対する対策の提示
リスクの評価にはOWASP Risk Rating Methodologyの簡易版を用います。リスクのスコアについては、3以下がLOW、6以下がMID、9以下がHIGHになります。
レーティングの式については、下記の通りです。
((悪用容易性 + 脆弱性の普及度+検知可能性) / 3) * 技術的影響 = スコア
取りまとめる表の形式は、下記の通りです。

| ID | 脆弱性 | 悪用可能性 | 普及度 | 検知可能性 | 技術的影響 | スコア | リスク | 対策 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| V1 | | | | | | 1.6 | LOW | |
| V2 | | | | | | 5.3 | MID | |
| V3 | | | | | | 1.6 | LOW | |
| V4 | | | | | | 1.6 | LOW | |

## STRIDE+AI
STRIDE+AIについては、用意されたDFD図に対して下記の流れで実施しました。

1. 信頼境界の設定(3つ)
DFD図に対して信頼境界を3つ設定します。

2. 信頼境界ごとのSTRIDEテーブルの作成
STRIDEテーブルは、STRIDEの各項目ごとに緩和策と脆弱性を検討します。なお、脅威については、OWASP AI EXchange[1]で公開されているPeriodic table of AI security[2]を元に通常の脅威に加え、AI独自の脅威についても検討します。

| | 緩和策 | 脆弱性 |
| ---- | ---- | ---- |
| S | | |
| T | | |
| R | | |
| I | | |
| D | | |
| E | | |

3. リスクの評価と脅威に対する対策の提示
リスクの評価にはOWASP Risk Rating Methodologyの簡易版を用います。リスクのスコアについては、3以下がLOW、6以下がMID、9以下がHIGHになります。
レーティングの式については、下記の通りです。
((悪用容易性 + 脆弱性の普及度+検知可能性) / 3) * 技術的影響 = スコア
取りまとめる表の形式は、下記の通りです。

| ID | 脆弱性 | 悪用可能性 | 普及度 | 検知可能性 | 技術的影響 | スコア | リスク | 対策 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| V1 | | | | | | 1.6 | LOW | |
| V2 | | | | | | 5.3 | MID | |
| V3 | | | | | | 1.6 | LOW | |
| V4 | | | | | | 1.6 | LOW | |


## MAESTRO
MAESTROについては、用意されたDFD図に対して下記の流れで実施しました。

1. DFD図に対して、OWASP Agentic Security Initiative(ASI)[3]による 「Agentic AI – Threats and Mitigations(Version 1.1)」[4]の Detailed Threat Model(T1-T17)を適用して脅威を特定します。

2. システムの分解
システムをMAESTROの「7レイヤーアーキテクチャ」に基づいて構成要素に分解します。ここで、エージェントの構成要素と機能を定義します。

| レイヤー | 構成要素と機能 |
| ---- | ---- |
| 1.基盤モデル(Foundation Models)| |
| 2.データ操作(Data Operations)| |
| 3.エージェントフレームワーク(Agent Framework) | |
| 4.デプロイとインフラ(Deployment and Infrastructure) | |
| 5.評価と可観測性(Evaluation and Observability) | |
| 6.セキュリティとコンプライアンス(Security & Compliance) | |
| 7.エージェントエコシステム(Agent Ecosystem) | |


3. レイヤー別の脅威モデリング(Layer-Specific Threat Modeling)  
各層固有の脅威ランドスケープを用いて、該当する層に潜む脅威を特定します。


| レイヤー | 脅威名 | 脅威 | 攻撃シナリオ |
| ---- | ---- | ---- | ---- |
| 1.基盤モデル(Foundation Models) | 1. | | |
| 2.データ操作(Data Operations) | 2. | | |
| 3.エージェントフレームワーク(Agent Framework) |  | | |
| 4.デプロイとインフラ(Deployment and Infrastructure) |  | | |
| 5.評価と可観測性(Evaluation and Observability) |  | | |
| 6.セキュリティとコンプライアンス(Security & Compliance) |  | | |
| 7.エージェントエコシステム(Agent Ecosystem) |  | | |

4. レイヤーを越えた脅威の特定  
レイヤーの間の相互作用を分析し、あるレイヤーの脆弱性が他のレイヤーにどのような影響を及ぼすか(連鎖的なリスク)を検討します。

| レイヤーの組み合わせ | 脅威名 | 脅威 | 攻撃シナリオ |
| ---- | ---- | ---- | ---- |
|  | 3. | | |
|  |  | | |
|  |  | | |
|  |  | | |
|  |  | | |

5. リスク評価と緩和策の計画  
リスク測定基準とマトリックスを用いて、各脅威の「発生可能性」と「影響度」を評価します。その結果に基づき、対処すべき脅威の優先順位を決定します。リスクは以下の式および値に基づき計算します。

R = P × I

- P (Likelihood / 可能性): 運用条件または敵対的な条件下で、その脅威が発生する確率の推定値。
- I (Impact / 影響): 脅威が達成された場合に発生する可能性のある結果の深刻度レベル。

| 評価 | 数値(Ordinal Value) |
| ---- | ---- |
| Low (低) | 1 |
| Medium (中) | 2 |
| High (高) | 3 |


| 脅威名 | 脅威 | 発生可能性(P) | 影響度(I) | リスク(R) | 対策 |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1. |  | | | | |
| 2. |  | | | | |
| 3. | | | | | |

## 参考文献
1. AI Exchange, https://owaspai.org
1. 0\. AI Security Overview | AI Exchange, https://owaspai.org/docs/ai_security_overview/#periodic-table-of-ai-security
1. Agentic Security Initiative - OWASP Gen AI Security Project, https://genai.owasp.org/initiatives/agentic-security-initiative/
1. Agentic AI - OWASP Lists Threats and Mitigations, https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/ 
