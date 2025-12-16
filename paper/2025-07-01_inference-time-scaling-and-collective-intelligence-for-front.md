# Inference-Time Scaling and Collective Intelligence for Frontier AI

- Source: https://sakana.ai/ab-mcts/
- Published: 2025-07-01

## 重要ポイント

- AB-MCTSは推論時に深さ・幅方向の探索を適応的に切り替え、効率的な試行錯誤を実現するアルゴリズム。

- Multi-LLM AB-MCTSは複数のLLM（例：o4-mini、Gemini-2.5-Pro、DeepSeek-R1-0528）を動的に使い分け、AIの集団知能を形成。

- ARC-AGI-2ベンチマーク実験で、従来のリピートサンプリングや単一LLMを大きく上回るPass@250（30%以上）を記録。

- 探索過程で問題に最適なLLMの選択頻度が自動で増加し、単体では解けない問題も組み合わせで解決。

- アルゴリズムの実装はTreeQuest（Apache 2.0ライセンス）として公開され、応用やカスタマイズが容易。

- 推論時スケーリングとAI集団知能は今後のAI発展において重要な新潮流と位置づけられる。

- 今後の課題として、最終解の選択法や報酬設計、他のマルチエージェント手法との比較検証が挙げられる。


## 要約
