# OperantKit

[English](README.md)

行動研究のためのオペラント条件づけツールキット。

---

## アーキテクチャ

OperantKit は計算能力と責務範囲によって5層に関心を分離する:

```
                         contingency-dsl
 ┌─────────────────────────────────────────────────┐
 │  Core                 非チューリング完全（CFG）    │
 │  FR 10, Conc(VI 30-s, VI 60-s)                  │
 ├──────────────────────────────────────────────────┤
 │  Core-Stateful        CFG構文、TC近傍評価         │
 │  Pctl(IRT, 50), Adj(start=FR 1, step=2)         │
 ├──────────────────────────────────────────────────┤
 │  Core-TrialBased      CFG構文、離散試行           │
 │  MTS(comparisons=3, consequence=CRF, ITI=5s)    │
 └──────────────────────────────────────────────────┘
 ┌──────────────────────────────────────────────────┐
 │  contingency-core     チューリング完全             │
 │  動的遷移: フェーズ変更、ヨーキング、               │
 │  適応的滴定                                       │
 ├──────────────────────────────────────────────────┤
 │  experiment-core      チューリング完全 + 安全性制約 │
 │  有限化: セッション終了、安定状態基準、              │
 │  ABAデザイン検証                                   │
 └──────────────────────────────────────────────────┘
```

**contingency-dsl** は随伴性が「何であるか」を宣言する。
**contingency-core** は随伴性が「どう変化するか」を定義する。
**experiment-core** は実験が「終了し測定可能なデータを生成すること」を保証する。

測定仕様（反応率、強化率、IRT分布、切り替え率）と手続き記述との接続は、実験層・分析層の責務であり、DSLの責務ではない。

## パッケージ

| カテゴリ | パッケージ | 役割 |
|---|---|---|
| **core** | contingency-dsl | 言語非依存のDSL仕様（文法、ASTスキーマ、適合テスト） |
| | contingency-dsl-py | Pythonリファレンスパーサー |
| | contingency-dsl-paper | DSL AST → JEAB/J-ABA Method section コンパイラ |
| | contingency-dsl-reader | 論文Method section → DSL AST 抽出器 |
| | contingency-py | Python強化スケジュールエンジン |
| | contingency-rs | Rust実装（HIL、PyO3/WASM/C FFIバインディング） |
| | models | コア行動モデル（手続き・結果） |
| **experiment** | experiment-core | セッションライフサイクル、実験文脈、復活パラダイム |
| | contingency-annotator | アノテーション拡張（刺激、時間、被験体、装置） |
| | session-recorder | セッションランナー + JSONLイベントロガー |
| **analysis** | session-analyzer | 累積記録、統計分析、モデルフィッティング |
| **simulators** | behavior-simulator | 動物モデルシミュレーション |
| | ai-operant-box | LLMを仮想被験体とする実験環境 |
| | social-contingency-sim | 社会的随伴性シミュレータ（7方策 x 5シナリオ） |
| **tools** | aba-advisor | ABA介入提案エンジン（TF-IDF + ルール + opt-in LLM） |
| | behavior-scope | 非侵襲的リアルタイム観測ダッシュボード |

## 起源

[YutoMizutani/OperantKit](https://github.com/YutoMizutani/OperantKit)（Swift, MIT, 2018-2020）を復興・進化させた行動分析ツールキット。
