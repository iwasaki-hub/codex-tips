# codex-tips

海外のCodexユーザーが共有している実践Tipsと、OpenAI公式ガイドの知見を日本語で整理したメモ集です。

**対象:** CodexでTypeScript / React / Expo / Supabaseなどのアプリを開発する人  
**最終確認:** 2026-11-21  
**方針:** 公式情報とコミュニティの体験談を分け、再利用できる雛形を残す

## URLだけで各プロジェクトに適用する（初回設定のみ必要）

ChatGPTの**プロジェクト設定 → プロジェクトの指示**に[URL連携用テンプレート](templates/CHATGPT_PROJECT_INSTRUCTIONS.md)を一度だけ追加します。その後は、適用したい実践編のURLを、そのChatGPTプロジェクトのチャットに**URLだけで送信**してください。

- **[一般編のURL](docs/CODEX_TIPS_JA.md)** — AGENTS.md、計画、レビュー、Skillsを対象リポジトリと照合
- **[Codex CLI編のURL](docs/CODEX_CLI_TIPS_JA.md)** — CLI運用・設定・自動化の改善点を診断
- **[VS Code × Codex × ChatGPT編のURL](docs/VSCODE_CODEX_CHATGPT_TIPS_JA.md)** — エディタ・ChatGPT・PRの使い分けを診断

**[設定手順と動作例はこちら](docs/URL_ACTIVATION.md)**。ガイドと対象のGitHubリポジトリを読める接続・権限が必要です。URLだけでは**読み取り・診断・計画まで**、ファイルの編集やPR作成は後続の明示的な適用指示で実施します。非公開リポジトリを公開に変更する必要はありません。

## まず読む

- **[Codex 実践Tips 8選](docs/CODEX_TIPS_JA.md)** — AGENTS.md、計画、完了条件、検証、レビュー、Skills、Worktree、引き継ぎ
- **[Codex CLI 実践Tips 9選](docs/CODEX_CLI_TIPS_JA.md)** — CLIコマンド、設定プロファイル、compact、レビュー、exec、Worktree、利用状況、MCP
- **[VS Code × Codex × ChatGPT 実践Tips 8選](docs/VSCODE_CODEX_CHATGPT_TIPS_JA.md)** — エディタの文脈共有、設計→実装→レビュー、Windows/macOSの違い
- **[VS Code × Codex × ChatGPT 専用プロンプト](templates/VSCODE_CODEX_CHATGPT_PROMPTS.md)** — 設計、選択範囲の修正、実装、レビュー、PR判断、引き継ぎ
- **[コピー用プロンプト集](templates/PROMPTS.md)** — 調査・計画、実装、レビュー、PR検証、引き継ぎ、Supabase変更前の確認
- **[AGENTS.md雛形](templates/AGENTS.example.md)** — 既存の開発ルールに合わせて調整するための最小テンプレート
- **[参考資料・出典](docs/SOURCES.md)** — 公式ガイドとRedditでの体験談を分類

## 8つのTips（要約）

| # | Tip | ポイント |
| --- | --- | --- |
| 1 | AGENTS.mdを短くする | いつも必要なルールと参照先だけを記載 |
| 2 | 複雑な変更だけ先に計画 | 小さな修正には過剰な計画を強制しない |
| 3 | 完了条件を明確にする | Goal / Context / Constraints / Done when |
| 4 | 影響範囲に応じて検証する | 作業中は関連テスト、マージ前は必須CI |
| 5 | レビューを独立させる | 実装後に差分・リスク・不足テストを点検 |
| 6 | 反復作業をSkills化する | 少数の明確なSkillを再利用 |
| 7 | Worktreeで並列作業を分離 | 競合を避けて最後に統合検証 |
| 8 | 長期作業はファイルに引き継ぐ | WORKLOG / HANDOFFなどを事実ベースで更新 |

## CLI編も追加

ターミナルで使う場合は [Codex CLI 実践Tips 9選](docs/CODEX_CLI_TIPS_JA.md) を参照してください。CLI内のスラッシュコマンドと、PowerShellなどのシェルで実行するコマンドを分けて説明しています。公式仕様と海外ユーザーの体験談も区別しています。

## VS Code × ChatGPT編も追加

エディタを中心に開発する場合は、[VS Code × Codex × ChatGPT 実践Tips](docs/VSCODE_CODEX_CHATGPT_TIPS_JA.md) と [専用プロンプト集](templates/VSCODE_CODEX_CHATGPT_PROMPTS.md) を活用してください。ChatGPTでの計画とCodexの編集・検証を分けて説明しています。

## 活用方法

1. [Tips本文](docs/CODEX_TIPS_JA.md)で目的と注意点を確認する。
2. [プロンプト集](templates/PROMPTS.md)から必要なものをコピーし、`[ ]`を置換する。
3. 対象リポジトリの既存ルール・実際のテストコマンドを確認してから実行する。
4. 有用だった手順だけを、そのプロジェクトのAGENTS.mdやSkillsに反映する。

> このリポジトリはTipsの保存場所です。掲載している雛形は自動適用されません。また、Redditで報告された節約率・品質改善は個人の体験談であり保証ではありません。

## 更新ルール

公式仕様と食い違う内容があれば公式情報を優先し、モデル更新後は古い制約が不要になっていないか見直します。リンク切れや有効性が未確認の情報は確認済みと書きません。
