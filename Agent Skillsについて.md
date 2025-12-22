https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
#AIエージェント 
## 1) まず守るべき3原則

- **短く書く（Concise is key）**：Skill は会話履歴や他の情報と同じコンテキスト枠を取り合う。必要なことだけを書く（「Claude は賢い」を前提に、説明しすぎない）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- **“自由度”を適切に設定する**：壊れやすい/手順厳守の作業は「低自由度（コマンド固定・手順固定）」、状況依存で複数解がある作業は「高自由度（方針だけ）」にする。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- **使うモデル全部でテスト**：同じ Skill でもモデル差で必要なガイド量が変わるので、想定モデル（Haiku/Sonnet/Opus など）で実運用タスクを回して確認する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 2) “選ばれるSkill”にするコツ（description が最重要）

- `description` は **Skill発見/選択の鍵**。**「何ができるか」＋「いつ使うべきか（トリガー語・文脈）」**を入れる。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 視点は **三人称で統一**（system prompt に注入されるため、I/you 混在は選択ミスの原因になりうる）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 例：PDFなら「PDF/フォーム/抽出」など、Excelなら「.xlsx/表/ピボット」など、検索に引っかかる語を入れる。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 3) 情報の置き方：SKILL.mdは“入口”、詳細は分割

- **Progressive disclosure（段階的開示）**：`SKILL.md` は「クイックスタート＋参照リンク」で薄くし、詳細は別ファイルに分ける。必要になったときだけ読ませる。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- **参照のネストを深くしない**：`SKILL.md → advanced.md → details.md` みたいに深くすると、途中で “部分読み” されて情報欠落が起きやすい。**SKILL.md から1段でリンク**する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 長い参照ファイル（例：100行超）は **目次（TOC）** を先頭に置くと、部分読みでも全体像が掴める。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 4) ワークフローは「手順＋チェックリスト＋検証ループ」

- 複雑作業は **段階的手順**に分解し、チェックリスト形式にすると抜け漏れが減る。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 品質が重要な作業は **フィードバックループ**（例：validator → 修正 → 再検証）を明示する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 5) “古くなる情報”と“言葉の揺れ”を排除

- 日付で分岐するような **時限情報は避ける**。どうしても書くなら「Old patterns」などに隔離して、本文の正解を汚さない。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 用語は **一語に統一**（API endpoint / URL / route が混ざる、などは認識のブレを生む）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 6) 書き方の“型”：テンプレ・例・条件分岐

- 出力形式が重要なら **厳密テンプレ**、状況対応させたいなら **柔軟テンプレ**を提示する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- スタイルが重要なSkillは **入出力例（Examples）**が効く（説明より例が強い）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 判断が必要な作業は **条件分岐の導線**（「新規作成ならA、編集ならB」）を明示する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 7) コード/スクリプトを同梱するなら

- “Claudeに丸投げするスクリプト”はNG。**エラーハンドリングを含めて自律的に解決できる**ように書く（定数も理由を添える）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- **ユーティリティスクリプトを用意**すると、生成コードより堅牢で、トークンも節約できる。さらに「実行するのか／参照として読むのか」を明確に書く。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 重要操作は「計画ファイル作成→検証→実行→検証」みたいに **検証ステップを挟む**と事故が減る。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 8) 実行環境の注意（claude.ai と API は違う）

- **claude.ai** は npm/PyPI/GitHub からの導入が可能だが、**Anthropic API の実行環境はネットワークやランタイム導入が制限**される（前提差を意識して依存関係を書く）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 9) 最終チェックリスト（超要約）

- **description** が「何をする/いつ使う」を具体語入りで書けている
- `SKILL.md` 本文は **500行以内**、超えるなら分割（段階的開示） ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 参照は **SKILL.mdから1段**、長文参照はTOC
- 手順はチェックリスト化、重要作業は検証ループ
- 依存パッケージ/実行制約が明記され、スクリプトは“丸投げ”しない ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
