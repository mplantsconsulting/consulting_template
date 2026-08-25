# .claude — Claude Code の設定

このリポジトリで使えるスキルとコマンド。
**すべて日本語で出力し、このリポジトリのフォルダ構成・運用ルールに沿った形で成果物を作る**ように書いてある。

## 前提

作業ルールは [CLAUDE.md](../CLAUDE.md) に書いてある。特に以下は全スキル共通の制約。

- **判断させない。** 採否・優先順位は人が決める
- **出典・数値を生成させない。** 確認できないものは「確認できなかった」と書かせる
- **前提の穴を一般論で埋めさせない**

## コマンド（`/` で呼ぶ）

| コマンド | 何をするか | いつ使うか |
|---|---|---|
| `/anl-plan` | 分析の計画を `docs/analysis/` に起案する（**判定基準まで**書く） | 分析を始めるとき |
| `/anl-review` | 分析ファイルを運用ルールに照らして点検する | `計画確定` にする前 / `完了` にする前 |
| `/challenge` | 直前の結論・成果物に反証を当てる | 結論が出たとき（**必ず1回通す**） |
| `/qa-draft` | 各所の「未確認事項」を集約し、`docs/client-qa/` の照会文を作る | 確認事項が溜まったとき |
| `/index-sync` | `docs/INDEX.md` を実ファイルと突き合わせて更新する | コミット前 |
| `/submit-check` | [QUALITY_BAR.md](../docs/QUALITY_BAR.md) のチェックを機械的に通す | 資料を出す前 |

## スキル（必要に応じて自動で呼ばれる）

`strategy-skills-for-claude` の21スキルを、我々のユースケース
（**業種固定・DX中心・実装を含む下流も担当・日本語のみ**）に合わせて7本に絞り込んだもの。

| スキル | 何をするか | 出力先 |
|---|---|---|
| [issue-framing](skills/issue-framing/SKILL.md) | 依頼を問いに変換し、判定基準まで落とす | `docs/analysis/` `docs/strategy/` |
| [issue-structuring](skills/issue-structuring/SKILL.md) | 問題をMECEに割り、**空欄を可視化する** | `docs/strategy/` |
| [business-understanding](skills/business-understanding/SKILL.md) | 一次資料から業務フローを起こし、未確認点を洗い出す | `docs/business/` |
| [dx-initiative-design](skills/dx-initiative-design/SKILL.md) | DX施策を起案する（**実装工数と検証方法まで**） | `docs/ideas/` |
| [external-research](skills/external-research/SKILL.md) | 他社事例を出典付きで調査する（**捏造の禁止**） | `docs/cases/` |
| [deliverable-narrative](skills/deliverable-narrative/SKILL.md) | 提示資料の構成とメッセージラインを作る | `docs/presentations/` |
| [assumption-audit](skills/assumption-audit/SKILL.md) | 前提を洗い出し、**重要度×根拠の強さ**で格付けして検証タスクに変える | 監査対象のファイル |

### 元の21スキルとの関係

`strategy-skills-for-claude` はあくまでベースであり、以下の理由で絞り込んでいる。

| 絞り込みの理由 | 該当する元スキル |
|---|---|
| **業種が固定**のため、市場・競合の網羅的分析の比重が低い | market-mapping, competitive-intel, profit-pool-analysis, customer-segmentation |
| **DX中心**のため、価格・ポートフォリオ・M&A系は範囲外 | pricing-strategy, portfolio-review, strategic-options |
| **実装を含む下流も担当**するため、施策は工数見積とセットにした | initiative-prioritizer, transformation-roadmap → `dx-initiative-design` に統合 |
| 案件規模に対して重い | war-gaming, value-realization, operating-model-design, stakeholder-alignment |

**そのまま採り入れたもの**

| 採用した理由 | 元スキル |
|---|---|
| 「AIに頼ると前提の穴が埋められる」という我々の課題に直接効く | assumption-audit → `assumption-audit`（根拠の強さの判定を情報源の階層に合わせて改変） |

元の21スキルは**自習用の参考資料**として有用なので、必要になったら
`strategy-skills-for-claude`（社内で配布）側を直接参照する。

## 使い分けの目安

```
依頼が来た                → /anl-plan の前に issue-framing で問いに変換する
業務が分からない          → business-understanding（データを触る前に）
問題の全体像が見えない    → issue-structuring
分析を始める              → /anl-plan → （レビュー）→ 集計 → /anl-review
結論が出た                → /challenge（必ず通す）
施策を書く                → dx-initiative-design
提出前に足元を固める      → assumption-audit（重要度が高く根拠が弱い前提を潰す）
他社はどうしているか      → external-research
資料を作る                → deliverable-narrative → /submit-check
コミット前                → /index-sync
```
