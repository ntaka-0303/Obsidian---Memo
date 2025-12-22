https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
#AIエージェント 
## 1. Skills とは何か

- **Skills = Claude の機能を拡張する“モジュール型”の能力パッケージ**。  
    1つのSkillは、**メタデータ（name/description）＋手順（SKILL.md）＋任意のリソース（追加Markdown、テンプレ、スクリプト等）**をまとめたもの。Claudeは“関連すると判断したとき”に自動で使う。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- Skillsは**プロンプト（会話ごとの一回性の指示）**と違い、**再利用可能**で、同じガイダンスを何度も貼らなくてよくなる。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

## 2. なぜSkillsを使うのか（価値）

- **汎用エージェントを専門家化**：ドメインの手順・ベストプラクティス・判断軸を与え、特定業務に強くする。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- **繰り返し削減**：一度作っておけば、会話ごとに同じ説明を再投入しなくてよい（必要時にロードされる）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- **合成可能**：複数Skillを組み合わせて、より複雑なワークフローを構成できる。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

## 3. 仕組み（重要なメンタルモデル）

### 3.1 ファイルシステム前提（VM上の“フォルダ”として存在）

- Skillsは、Claudeが使える**コード実行環境（VM）上のディレクトリ**として配置され、Claudeはbash等でファイルを読み書きしながら利用する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- これにより、必要な情報だけを段階的に読む **progressive disclosure（段階的開示）** が成立する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

### 3.2 3段階のロード（＝トークン節約と確実性の両立）

- **Level 1: Metadata（常にロード）**  
    `SKILL.md` のYAML frontmatterにある `name` と `description` が、起動時点で system prompt に入る。Skillを大量に入れても“メタデータだけ”なので負担が小さい。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- **Level 2: Instructions（トリガー時にロード）**  
    descriptionがマッチすると、Claudeがbashで `SKILL.md` 本文を読み、手順がコンテキストに入る。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- **Level 3+: Resources / Code（必要に応じて）**  
    追加Markdownやテンプレ等は“参照されたときだけ”読む。**スクリプトはbashで実行され、コード本文はコンテキストに載らず、出力だけが載る**ので効率・再現性が高い。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

## 4. Skillの最小構造（必須要素）

- **必須：`SKILL.md` + YAML frontmatter（name/description）** ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- `name/description` にはバリデーション条件があり、特に description は「何をするか」だけでなく「いつ使うべきか（トリガー語・文脈）」まで含めるのが重要。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

## 5. Skillはどこで使えるか（運用の置き場所）

- **Claude API**：Anthropic管理の既成Skill（例：`pptx/xlsx/docx/pdf`）と、**自作Skill**の両方を同じ仕組みで利用できる。Skill指定は `container` で行い、コード実行やファイルAPIの前提（ヘッダ等）がある。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- **Claude Code**：カスタムSkillのみ。ディレクトリとして置けば自動発見される。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- **Claude Agent SDK**：`.claude/skills/` 配下に置き、設定（allowed_tools等）で有効化して自動発見される。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- **claude.ai**：既成Skillは裏側で利用される。カスタムSkillは設定からzipで追加できるが、**ユーザー単位で、組織で一元管理できない**点に注意。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

## 6. どう扱うべきか（設計・運用の方針）

### 6.1 Skillは「社内の業務プレイブック／オンボーディングガイド」を“実行可能”にしたものとして扱う

- 文体としても、OverviewではSkillを「新メンバー向けオンボーディング資料のように整理する」と説明している。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- つまり、Skillは**“個人のプロンプト集”ではなく、反復利用できる業務標準（SOP）**として設計するのが筋が良い。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

### 6.2 SKILL.md は「入口（概要＋分岐）」、詳細は分割（progressive disclosure）

- `SKILL.md` 本文は、必要十分な手順に絞り、詳細は別ファイルへ（段階的開示）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 目安として **SKILL.md本文は500行以内**が推奨。越えそうなら分割する。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 参照は **SKILL.md から1段**に留める（参照の参照を深くしない）。深いネストは部分読みで取りこぼしが起きやすい。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

### 6.3 “自由度”を設計する（壊れやすい業務ほど固定化）

- タスクの**脆さ（fragility）**と**状況依存性（variability）**に合わせて、指示の粒度を変える：
    - 高自由度：方針・観点（複数解が妥当）
    - 中自由度：擬似コードやパラメータ付き手順
    - 低自由度：実行するスクリプト／手順を固定（事故りやすい操作） ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

### 6.4 スクリプトは「Claudeに丸投げしない」

- スクリプトは“問題を解く”ように作る（Claudeに再解釈させず、**決定的に処理**する）。検証・フィードバックループも組み込む。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

### 6.5 モデル差分を前提にテストする

- Skillの効き方はモデルに依存するので、使う予定のモデル（Haiku/Sonnet/Opus等）で十分に試す。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))

## 7. セキュリティ（必須の扱い）

- Skillsは「指示＋コード」で能力を増やす分、**悪意あるSkillがツール実行やコード実行を悪用**しうる。基本は**信頼できる提供元（自作／Anthropic提供等）だけを使う**。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
- 外部URLから取得する内容は、取得先が変化・改ざんされうるため特にリスクが高い。導入時は**同梱ファイル・スクリプトを監査**し、「ソフトをインストールする」感覚で扱う。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))

---

## 8. 実務用チェックリスト（超短縮）

- description は **何をする／いつ使う**が具体語入りで書けている（3人称）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- `SKILL.md` は入口に徹し、本文は **500行以内**、詳細は分割。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 参照は **SKILL.mdから1段**、ネスト参照は避ける。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 壊れやすい操作は低自由度（固定手順／スクリプト）、状況依存は高自由度。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices "Skill authoring best practices - Claude Docs"))
- 信頼できないSkillは使わない／使うなら監査（外部URLは特に警戒）。 ([Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview "Agent Skills - Claude Docs"))
