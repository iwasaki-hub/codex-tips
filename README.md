# codex-tips

海外のCodexユーザーが共有している実践Tipsと、OpenAI公式ガイドの知見を日本語で整理したメモ集です。

**対象:** CodexでTypeScript / React / Expo / Supabaseなどのアプリを開発する人  
**最終確認:** 2026-11-22  
**方針:** 公式情報とコミュニティの体験談を分け、再利用できる雛形を残す

## URLだけで各プロジェクトに適用する（初回設定のみ必要）

ChatGPTの**プロジェクト設定 → プロジェクトの指示**に[URL連携用テンプレート](templates/CHATGPT_PROJECT_INSTRUCTIONS.md)を一度だけ追加します。その後は、適用したい実践編のURLを、そのChatGPTプロジェクトのチャットに**URLだけで送信**してください。

- **[一般編のURL](docs/CODEX_TIPS_JA.md)** — AGENTS.md、計画、レビュー、Skillsを対象リポジトリと照合
- **[Codex CLI編のURL](docs/CODEX_CLI_TIPS_JA.md)** — CLI運用・設定・自動化の改善点を診断
- **[VS Code × Codex × ChatGPT編のURL](docs/VSCODE_CODEX_CHATGPT_TIPS_JA.md)** — エディタ・ChatGPT・PRの使い分けを診断

**[設定手順と動作例はこちら](docs/URL_ACTIVATION.md)**。**`codex-tips` はPublic**なので実践ガイドはGitHubログインなしで閲覧できます（ChatGPTがURLを取得できる環境が必要です）。適用先の開発リポジトリが**Privateの場合のみ**、その内容をChatGPTが読むにはGitHub接続など適切なアクセス権が必要です。適用先を公開する必要はありません。URLだけでは**読み取り・診断・計画まで**、ファイル編集やPR作成は後続の明示的な適用指示で実施します。

## まず読む

- **[Codex 実践Tips 8選](docs/CODEX_TIPS_JA.md)** — AGENTS.md、計画、完了条件、検証、レビュー、Skills、Worktree、引き継ぎ
- **[Codex CLI 実践Tips 9選](docs/CODEX_CLI_TIPS_JA.md)** — CLIコマンド、設定プロファイル、compact、レビュー、exec、Worktree、利用状況、MCP
- **[VS Code × Codex × ChatGPT 実践Tips 8選](docs/VSCODE_CODEX_CHATGPT_TIPS_JA.md)** — エディタの文脈共有、設計→実装→レビュー、Windows/macOSの違い
- **[UIデザイン設計 実践Tips 10選](docs/UI_DESIGN_TIPS_JA.md)** — 海外の公式ガイド・研究・実装例を、設計原則、iPhone、アクセシビリティ、トークン、React UI、検証に分類
- **[セキュリティ実践Tips 10選](docs/SECURITY_TIPS_JA.md)** — Codexの権限、プロンプトインジェクション、秘密情報、GitHub Actions、Expo・Supabaseの安全対策
- **[海外のWeb・モバイル脆弱性対策Tips 12選](docs/APP_VULNERABILITY_TIPS_JA.md)** — OWASP Top 10:2025、API認可、XSS/CSRF/SSRF、2026年のNext.js脆弱性、iPhone/Android対策、確認テスト
- **[セキュリティ診断用プロンプト](templates/SECURITY_REVIEW_PROMPTS.md)** — 読み取り専用診断、認可・RLS、PR/CIレビュー、安全な修正依頼
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

## UIデザイン設計編も追加

海外発の参考資料を探すときは [UIデザイン設計 実践Tips 10選](docs/UI_DESIGN_TIPS_JA.md) を参照してください。Apple HIG、Material Design、Nielsen Norman Group、W3C、Mobbin、Radix、shadcn/uiなどを、設計・実装・レビューの場面ごとにまとめています。**参考例とアクセシビリティ基準を混同しない**こと、確認済みの操作と未確認の実機テストを区別することを重視しています。

UIデザイン編は、既存の3つの「URLだけで適用する」ガイドを選んだ後、**UIに関連する課題がある場合に参照する補助ガイド**です。UI編のURL自体を4番目の起動条件に追加する方式ではありません。

## Codex実践編 → UIデザイン編への連携

[ChatGPTプロジェクト指示テンプレート](templates/CHATGPT_PROJECT_INSTRUCTIONS.md)を設定すると、既存の**一般編／CLI編／VS Code編**のURLを選択した後、対象リポジトリのUI・UX課題が確認できた場合のみ、[UIデザイン設計Tips](docs/UI_DESIGN_TIPS_JA.md)を併せて参照できます。

**Codex実践編を選択 → 現状診断 → UI課題があればUI設計Tipsを追加照合 → 統合改善案（最大3件）→ 明示的な依頼で実装**、という流れです。UI課題がない場合は連携せず、CLI・CI・認証など元の課題に集中します。実装やPR作成はURLだけでは実行しません。

運用例と条件は [URL連携ガイドの「UI連携」](docs/URL_ACTIVATION.md#codex実践編を選んだ後にuiデザイン設計tipsへ連携) にまとめました。**以前のプロジェクト指示を設定済みのChatGPTプロジェクトには、更新したテンプレートを再反映してください。** GitHubリポジトリを更新するだけで、設定済みの指示が自動更新されるわけではありません。

## セキュリティ実践Tipsも追加

[セキュリティ実践Tips 10選](docs/SECURITY_TIPS_JA.md)では、**AIエージェントの権限と信頼境界**、**秘密情報の漏えい防止**、**GitHub Actions / 依存関係**、**Expo / Next.js / Supabaseの認証・認可**を確認できます。アプリ本体の脆弱性は[海外のWeb・モバイルアプリ対策Tips 12選](docs/APP_VULNERABILITY_TIPS_JA.md)に分け、OWASP Top 10:2025、2026年のNext.js/モバイルの公式情報と検証条件も掲載しました。最初は読み取り専用の診断から始める[専用プロンプト集](templates/SECURITY_REVIEW_PROMPTS.md)も用意しました。

既存の一般編・CLI編・VS Code編のURLを使った診断中に、**根拠のあるセキュリティ課題が見つかった場合のみ**追加参照します。セキュリティ編のURL自体は新たな単独起動条件にはせず、既存の3つの入口を維持します。UI編と両方該当しても優先改善は**合計3件以内**です。**更新済みのプロジェクト指示は各ChatGPTプロジェクトに再反映が必要**です。

## 活用方法

1. [Tips本文](docs/CODEX_TIPS_JA.md)で目的と注意点を確認する。
2. [プロンプト集](templates/PROMPTS.md)から必要なものをコピーし、`[ ]`を置換する。
3. 対象リポジトリの既存ルール・実際のテストコマンドを確認してから実行する。
4. 有用だった手順だけを、そのプロジェクトのAGENTS.mdやSkillsに反映する。

> このリポジトリはTipsの保存場所です。掲載している雛形は自動適用されません。また、Redditで報告された節約率・品質改善は個人の体験談であり保証ではありません。

## 更新ルール

公式仕様と食い違う内容があれば公式情報を優先し、モデル更新後は古い制約が不要になっていないか見直します。リンク切れや有効性が未確認の情報は確認済みと書きません。
