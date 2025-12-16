# Inference-Time Scaling and Collective Intelligence for Frontier AI

- Source: https://sakana.ai/ab-mcts/
- Published: 2025-07-01

## 重要ポイント

- AB-MCTSは推論時の計算資源を活用し、LLMによる探索の深さ・幅を柔軟に調整できる新手法。

- Multi-LLM AB-MCTSは複数のフロンティアLLMを状況に応じて動的に選択し、集団知能を構築。

- ARC-AGI-2ベンチマークで、単一モデルや従来手法を大幅に上回るPass@k（最大30%以上）を実現。

- 複数LLMの協調により、個々のモデルでは解けない問題も解決可能であることを示した。

- TreeQuestとしてアルゴリズムの実装をApache 2.0ライセンスで公開。

- 推論時スケーリングと集団知能の組み合わせが今後のAI発展の新たな方向性を示唆。

- 最終候補解選択や評価指標の改良など、今後の研究課題も明示されている。


## 要約
本論文では、Sakana AIが提唱する新しい推論時スケーリング手法「AB-MCTS（Adaptive Branching Monte Carlo Tree Search）」と、複数のフロンティアAIモデルを協調的に活用する「Multi-LLM AB-MCTS」について紹介している。従来の進化的モデル融合に加え、推論時にも複数のLLMを活用し、個々のモデルの特性や偏りを集団知能として利用するアプローチである。AB-MCTSは、深さ方向（既存解の精緻化）と幅方向（新規解の生成）の両方を柔軟に探索し、アルファ碁等で用いられるMCTSを拡張したもの。さらに、Multi-LLM AB-MCTSではどのLLMを使うかという選択も含めた三次元的な探索を行い、個々の問題ごとに最適なモデルを動的に割り当てる。ARC-AGI-2ベンチマークでの実験では、既存手法を大きく上回る性能を示し、複数のLLM協調による問題解決や、個別LLMでは不可能な問題の解決も確認された。TreeQuestとして実装も公開されている。

- AB-MCTSは推論時における試行錯誤（trial-and-error）を効率化し、LLMによる解探索の深さと幅のバランスを最適化する。
- Multi-LLM AB-MCTSは、複数LLMの動的選択と協調による集団知能を実現。
- ARC-AGI-2ベンチマークで従来手法（Repeated Sampling）より高いPass@k率を達成。
- 個別LLMでは解けない問題も、複数LLMの協力で解決可能となる事例を示した。
- TreeQuestとしてアルゴリズム実装をオープンソースで公開。
- 今後は、より優れた最終解選択法や報酬設計の発展が課題とされる。