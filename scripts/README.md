# scripts — 分析の再現スクリプト

## 目的

[docs/analysis/](../docs/analysis/) の各分析を、**同じ入力から同じ結果が出る形で**再実行できるようにする。

> **再実行できない集計は結果として扱わない。**

## 置くもの

- 分析IDと1対1のスクリプト（1分析＝1ファイル）

```
ANL-NNN_<分析テーマ>.py    例: ANL-003_aging_by_contract_type.py
prepare_processed_data.py  生データ → data/processed/*.parquet の変換
```

## 置かないもの

- 複数の分析で使い回す処理 → [src/](../src/)（スクリプトからimportする）
- 使い捨ての試行錯誤 → 手元で完結させ、コミットしない

## ルール

1. **入出力のパスをスクリプト冒頭にまとめる。** どのデータを読み、どこへ書くかを、コードを追わずに読めるようにする。
2. **出力先は `docs/analysis/ANL-NNN_<テーマ>/` 配下に固定する。** 図は `figures/`、集計値は `tables/`。
3. **出力ファイル名をスクリプト内で決め打ちする。** 実行日などを名前に含めない（差分が追えなくなる）。
4. **前処理と集計を分けて書く。** どこまでが加工で、どこからが集計かが読み取れる状態にする。
5. **除外・補完はコード上でも明示する。** `docs/analysis/` の「前提・制約」と一致させる。**片方だけ直さない。**
6. **生データ（`data/raw/`）を書き換えない。** 読み取り専用として扱う。
7. **個人・個体を特定しうる列を出力に含めない。** `docs/` 配下に書き出す集計値は、集計途中であっても匿名の粒度にする。

## 雛形

```python
"""ANL-NNN <分析テーマ>

問い: <docs/analysis/ANL-NNN_<テーマ>.md の「問い」と同じものを書く>
計画: docs/analysis/ANL-NNN_<テーマ>.md
"""

from pathlib import Path

import pandas as pd

# ---- 入出力 -------------------------------------------------------------
RAW = Path("data/raw/<ファイル名>")
OUT = Path("docs/analysis/ANL-NNN_<テーマ>")
FIG = OUT / "figures"
TBL = OUT / "tables"

# ---- 前提・除外条件 -----------------------------------------------------
# docs/analysis/ANL-NNN_<テーマ>.md の「前提・制約」と一致させる
PERIOD = ("YYYY-MM-DD", "YYYY-MM-DD")
EXCLUDE_REASON = "<何をなぜ除外するか>"


def load() -> pd.DataFrame:
    """生データの読み込み（共通処理は src/ に切り出す）"""
    ...


def preprocess(df: pd.DataFrame) -> pd.DataFrame:
    """前処理。除外・補完はここに閉じる"""
    ...


def aggregate(df: pd.DataFrame) -> pd.DataFrame:
    """集計。ここから先は加工しない"""
    ...


def main() -> None:
    FIG.mkdir(parents=True, exist_ok=True)
    TBL.mkdir(parents=True, exist_ok=True)

    df = preprocess(load())
    result = aggregate(df)

    # 集計値は CSV（UTF-8 BOM付き）で保存する
    result.to_csv(TBL / "tbl01_<内容>.csv", index=False, encoding="utf-8-sig")


if __name__ == "__main__":
    main()
```
