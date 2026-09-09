# consulting_template

データ分析を伴うコンサルティング案件のためのワークスペース雛形。
**「誰の、どの意思決定を動かすために、何をどの順番でやるか」** をフォルダ構成として固定してある。

> **これは GitHub のテンプレートリポジトリである。**
> 案件を始めるときは `Use this template` → `Create a new repository` で新しいリポジトリを作る
> （**clone / fork ではない**）。手順は [SETUP.md](SETUP.md)。
>
> **このリポジトリ自体にクライアント情報・受領データ・案件の分析結果を置かないこと。**
> ここに入れてよいのは、案件をまたいで使える雛形とルールだけ。

- **はじめて使う人はここから: [SETUP.md](SETUP.md)** — 案件開始から最初の30分でやること
- **作業ルール（Claude が読む）: [CLAUDE.md](CLAUDE.md)** — 案件ごとに冒頭を書き換える
- **提案までの7ステップ: [docs/WORKFLOW.md](docs/WORKFLOW.md)** — 迷ったらこれ
- **成果物の合格ライン: [docs/QUALITY_BAR.md](docs/QUALITY_BAR.md)** — 提出前に必ず通す

---

## この雛形が前提にしている考え方

> **成果物の価値は、精度でも網羅性でもなく「相手が判断できるようになったか」で決まる。**

この1文から、以下のルールが導かれている。フォルダ構成はその実装である。

| ルール | どこで強制されるか |
|---|---|
| 判定基準は**結果を見る前に**書き、後から変えない | [docs/analysis/](docs/analysis/) の計画→結果の2段構成 |
| 事実・数値・解釈・提案を混ぜない | 置き場所の分離（下表） |
| 数値は出所まで辿れる | `analysis` ↔ `scripts` の1対1、`presentations/sources.md` |
| 「言えないこと」を明記する | 各テンプレートの必須項目 |
| 前提が変わったら上書きせず新ファイル | 各フォルダの運用ルール |
| 業務理解 → データ定義 → 分析 の順を崩さない | [docs/WORKFLOW.md](docs/WORKFLOW.md) の Step 1→2→4 |

---

## ディレクトリ

| パス | 役割 | 何を置くか |
|---|---|---|
| [docs/business/](docs/business/) | **事実** | 事業構造・業務フロー・規約・用語定義。情報源の階層を明記する |
| [docs/cases/](docs/cases/) | **外部事実** | 他社事例・外部ソリューション調査（出典必須） |
| [docs/data-dictionary/](docs/data-dictionary/) | **データの事実** | 変数定義・データ形状・結合キー・指標の定義台帳 |
| [docs/analysis/](docs/analysis/) | **数値と導出過程** | 分析の計画と結果（計画を先に書き、結果を後から埋める） |
| [docs/strategy/](docs/strategy/) | **構造化** | 問題の構造化・論点整理・戦略レベルの示唆 |
| [docs/ideas/](docs/ideas/) | **提案** | 施策候補（採否の決定はしない。判断材料を揃える） |
| [docs/presentations/](docs/presentations/) | **提示** | 意思決定者への提出資料。対外的に出るため検証のハードルが一段高い |
| [docs/client-qa/](docs/client-qa/) | **確認事項** | データでは決着しない論点を、答えやすい形で照会する |
| [scripts/](scripts/) | 再現スクリプト（分析IDと1対1。担当者別に分ける） | |
| [src/](src/) | 複数の分析で使う共通処理 | |
| [notebooks/](notebooks/) | 探索・試行錯誤の作業場。**Git管理外**（[運用](notebooks/README.md)） | |
| [data/](data/) | 入力データ。**Git管理外**（[構成](data/README.md)） | |
| [.claude/](.claude/) | Claude 用のスキルとコマンド（[一覧](.claude/README.md)） | |

各フォルダの運用ルールは、それぞれの `README.md` に記載。**フォルダを選ぶ前に、その README を読む。**

---

## 提案までの7ステップ（詳細は [docs/WORKFLOW.md](docs/WORKFLOW.md)）

```
依頼
 │
 ├─ Step 0  依頼を問いに変える          → CLAUDE.md（KGI・役割・禁止事項）
 ├─ Step 1  業務と市場を理解する        → docs/business/  docs/cases/
 ├─ Step 2  用語とデータを定義する      → docs/data-dictionary/
 ├─ Step 3  問題を構造化する            → docs/strategy/
 ├─ Step 4  問いを分析に落として検証    → docs/analysis/ + scripts/ + src/
 ├─ Step 5  施策を起案する              → docs/ideas/
 ├─ Step 6  意思決定者に提示する        → docs/presentations/ + sources.md
 └─ Step 7  決着しないことを確認事項に  → docs/client-qa/
                                            │
            回答が来たら Step1 / Step2 を更新して Step3 へ戻る ←┘
```

**Step 1・2 を飛ばして Step 4 に行かないこと。** これが最も多い失敗である。
データはすぐ触れるが業務は聞かないと分からないため、触れるものから始めると業務の理解が永久に後回しになる。
**最初の1〜2週間が Step 1・2 で終わるのは正常。**

---

## 分析の進め方

計画を先に書き、結果を後から埋める。**1ファイル＝1つの問い。**

```
docs/analysis/_TEMPLATE.md をコピーして採番
        ↓
① 計画（問い・データソース・前提・手法・判定基準）      → 計画中 / 計画確定
        ↓
② 集計         scripts/<担当者名>/ANL-NNN_<テーマ>.py  → 検証中
        ↓
③ 結果・解釈   図は figures/、集計値は tables/ へ出力    → 完了
```

判定基準は結果を見る前に決め、見た後に書き換えない。詳細は [docs/analysis/README.md](docs/analysis/README.md)。

---

## 開発環境

パッケージ管理は `uv`。

```bash
uv sync                      # 依存関係の同期
uv run ruff check .          # リンター
uv run ruff format .         # フォーマッター
uv run ty check              # 型チェック
uv run pytest                # テスト（動作確認の最小限）
```

分析は pandas 中心（重い集計・整形のみ polars / numpy）、可視化は plotly、中間データは parquet。
詳細と方針は [CLAUDE.md](CLAUDE.md)。

---

## このテンプレート自体を更新するとき

案件で「この運用は不便だった」「この欄が要らなかった」と分かったら、テンプレートに戻す。
**運用の改善がテンプレートに反映されないと、次の案件で同じ手戻りが起きる。**

1. `consulting_template` を直接 clone する（案件リポジトリとは別に）
2. ブランチを切って変更し、Pull Request を出す
3. **クライアント固有の内容が混ざっていないか**を確認してからマージする
   （固有名詞・実データの数値・受領ファイル名）

```bash
git clone https://github.com/mplantsconsulting/consulting_template.git
cd consulting_template
git switch -c improve/<変更の要点>
```

**既に作成済みの案件リポジトリには、テンプレートの変更は自動では反映されない。**
テンプレートリポジトリは作成時点のコピーを配る仕組みであり、追従の機能はない。
必要な差分は各案件で手で取り込む（多くの場合、`docs/` の README とテンプレートだけで足りる）。

### 変更してよいもの / 慎重に扱うもの

| | 例 |
|---|---|
| **気軽に直してよい** | 記入テンプレートの欄、README の説明、コマンドの点検項目 |
| **慎重に扱う** | 各フォルダの責務分担、ステータスの定義、`docs/analysis/` の計画→結果の運用 |

後者は**成果物の品質を担保している仕組み**なので、変える場合は理由を PR に書く。
