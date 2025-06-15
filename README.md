# 量子プログラミング・チュートリアル — 事前準備README

## 本資料の目的

この README は、量子プログラミング・チュートリアルに参加される方向けに、**演習開始前に整えていただきたい実行環境**をまとめたものです。演習当日に量子アルゴリズムの実装と解析に集中できるよう、必ず事前にご準備ください。

## 対象者

- Python の基礎文法を理解している方
- Jupyter Notebook／Google Colab の基本操作を経験済みの方  
- 量子コンピューティングの入門者〜中級者

## 推奨実行環境

実行環境は **Google Colab** を前提に作成しています。

## パッケージとバージョン

演習資料は下表のバージョンで動作検証済みです。可能な限り**バージョンを固定すると安定に動作します。**

| パッケージ | バージョン | 用途 |
|-----------|------------|------|
| **Python** | 3.11.13| Colabのデフォルト設定（2025.6.16）|
| **NumPy** | 2.1.3 | 数値計算基盤 |
| **SciPy** | 1.13.1 | 科学計算ライブラリ |
| **Scikit-learn** | 1.5.2 | 機械学習ライブラリ |
| **QURI Parts** | 0.22.0 | 量子回路シミュレータ |

## Google Colab でのクイックスタート

### Step 1: Google Colab にアクセス
[https://colab.google/](https://colab.google/) にアクセスし、「新しいノートブック」を作成

### Step 2: 環境セットアップ

以下のセルをノートブック冒頭に貼り付け、実行してください。**セッションが切れるたびに再実行が必要です。**

```python
!pip install numpy==2.1.3 scipy==1.14.1 scikit-learn==1.5.2 quri-parts==0.22.0
```

### Step 3: インストール確認
実行後に次のコードで各ライブラリのバージョンが表示されれば準備完了です。

```python
# === パッケージ確認 ===
import sys
import platform
import numpy as np
import scipy
import sklearn

print(f"Python: {sys.version}")
print(f"Platform: {platform.platform()}")

print("\nインストール確認:")
print(f"├─ NumPy: {np.__version__}")
print(f"├─ SciPy: {scipy.__version__}")
print(f"├─ Scikit-learn: {sklearn.__version__}")

# QURI Partsバージョン確認（複数の方法で試行）
quri_version = "不明"
try:
    import quri_parts
    if hasattr(quri_parts, '__version__'):
        quri_version = quri_parts.__version__
    else:
        # pipから直接バージョン情報を取得
        import subprocess
        result = subprocess.run(['pip', 'show', 'quri-parts'], 
                              capture_output=True, text=True, check=True)
        for line in result.stdout.split('\n'):
            if line.startswith('Version:'):
                quri_version = line.split(':', 1)[1].strip()
                break
except Exception as e:
    print(f"QURI Partsバージョン取得エラー: {e}")

print(f"└─ QURI Parts: {quri_version}")

# 依存関係チェック
quri_components = []
components = {
    'quri_parts.circuit': '回路構築',
    'quri_parts.core': 'コア機能', 
    'quri_parts.qulacs': 'Qulacsシミュレータ',
    'quri_parts.algo': 'アルゴリズム'
}

print("\nQURI Partsコンポーネント確認例:")
for module, description in components.items():
    try:
        __import__(module)
        print(f"{module} ({description})")
        quri_components.append(module)
    except ImportError as e:
        print(f"{module} ({description}): {e}")

# 詳細情報
print(f"\n検出コンポーネント: {len(quri_components)}/{len(components)}")

if len(quri_components) >= 3:
    print("QURI Parts正常にインストール済み")
else:
    print("QURI Partsの一部コンポーネントが不足")
```

```
Python: 3.11.13 (main, Jun  4 2025, 08:57:29) [GCC 11.4.0]
Platform: Linux-6.1.123+-x86_64-with-glibc2.35

インストール確認:
├─ NumPy: 2.1.3
├─ SciPy: 1.14.1
├─ Scikit-learn: 1.5.2
└─ QURI Parts: 0.22.0

QURI Partsコンポーネント確認例:
quri_parts.circuit (回路構築)
quri_parts.core (コア機能)
quri_parts.qulacs (Qulacsシミュレータ)
quri_parts.algo (アルゴリズム)

検出コンポーネント: 4/4
QURI Parts正常にインストール済み
```

### Step 4: 動作テスト（推奨）
以下のコードで基本的な量子回路が正常に動作することを確認：

```python
# === 基本動作テスト ===
from quri_parts.circuit import QuantumCircuit
from quri_parts.circuit import H, CNOT

# Bell状態回路の作成
circuit = QuantumCircuit(2)
circuit.add_gate(H(0))        # Hadamardゲート
circuit.add_gate(CNOT(0, 1))  # CNOTゲート

print("動作テスト:")
print(f"├─ 回路作成: 成功")
print(f"├─ ゲート数: {len(circuit.gates)}")
print(f"└─ 量子ビット数: {circuit.qubit_count}")

print("\n環境構築完了！チュートリアル準備OK")
```

#### 全体的な環境リセット
```python
# 最終手段: 完全リセット
# メニューバー → ランタイム → ランタイムを接続解除して削除
# その後、新しいランタイムで Step 2 から再実行
```
