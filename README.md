# 印象評価と味覚評価の相関分析

## 概要

本プロジェクトでは、印象評価と味覚評価の相関関係を分析し、各色ごとの相関係数を算出するPythonスクリプトを提供します。データはエクセルファイルから読み込み、線形回帰分析を行い、相関係数を求めてグラフ化します。

## 使用技術

- Python 3
- Pandas
- SciPy
- Matplotlib

## 使用方法

### 1. 必要なライブラリのインストール
```bash
pip install numpy pandas scipy matplotlib
```

### 2. スクリプトの実行
```bash
python correlation_analysis.py
```

### 3. 出力データ
- `相関係数_苦味_印象と味覚.csv`
  - 各色のピアソン相関係数をまとめたCSVファイル。
- `<色>_苦味_印象と味覚.png`
  - 各色の印象評価と味覚評価の散布図および回帰直線を含むグラフ。

## データフォーマット

### **入力データ（Excelファイル）**
- `印象評価（相関関係）.xlsx`
- `味覚評価（相関関係）.xlsx`

各ファイルには、行に評価項目、列に被験者ごとのデータが含まれます。

### **Python コード**
```python
import pandas as pd
from scipy.stats import linregress
import matplotlib.pyplot as plt

# エクセルファイル名とシート名を指定
stress_file = "印象評価（相関関係）.xlsx"
impression_file = "味覚評価（相関関係）.xlsx"
stress_sheet = "苦味"
impression_sheet = "苦味"

# データの読み込み
df1 = pd.read_excel(stress_file, sheet_name=stress_sheet, index_col=0)
df2 = pd.read_excel(impression_file, sheet_name=impression_sheet, index_col=0)

# 相関分析
correlation_results = []
for color in df1.columns:
    stress_values = df1[color].dropna()
    impression_values = df2[color].dropna()
    if len(stress_values) < 2 or len(impression_values) < 2:
        continue
    slope, intercept, r_value, _, _ = linregress(impression_values, stress_values)
    correlation_results.append({"Color": color, "Pearson Correlation": round(r_value, 2)})

# CSV に保存
pd.DataFrame(correlation_results).to_csv("相関係数_苦味_印象と味覚.csv", index=False)
```

## ライセンス
本プロジェクトは MIT ライセンスのもとで提供されます。

## 著者
近畿大学 理工学部 情報学科 システムデザイン論研究室　田中真矢

