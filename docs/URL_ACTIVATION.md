# ChatGPTに実践編のURLを送るだけで「対象プロジェクトへの適用診断」を始める

最終更新: 2026-11-21

## できることと前提

この仕組みは、**各ChatGPTプロジェクトに指示を一度だけ設定する**ことで、その後は**実践ガイドのURLだけ**を送信して、対象リポジトリの現状診断・改善提案・Codex用プロンプトの作成を開始できるようにする運用テンプレートです。

GitHub上のMarkdownを更新するだけでChatGPTの動作を全チャットで自動変更できるわけではありません。**ChatGPTプロジェクトの設定が必要**で、毎回の挙動はそのプロジェクトの文脈・利用できる接続・権限に左右されます。

- **URLだけ → 読み取り・診断・適用計画（書き込みなし）**
- **その後「適用して」 → 明示的な実装依頼（権限・安全条件の範囲内でfeatureブランチとPR）**
- **本番DB/課金/デプロイ/マージなど → 変更対象と安全性を再確認してから別途承認**

## 初回設定（ChatGPTプロジェクトごとに一度）

1. ChatGPTで対象アプリの**プロジェクト**（例: LittleDiary、KondateBiyori）を開く。なければ作成する。
2. プロジェクト名のメニュー（…）から **プロジェクト設定 → プロジェクトの指示** を開く。
3. [CHATGPT_PROJECT_INSTRUCTIONS.md](../templates/CHATGPT_PROJECT_INSTRUCTIONS.md) の全文をコピーし、既存の指示と矛盾しないよう追記する。すでにプロジェクト固有のルールがある場合は消さずに統合する。
4. **GitHubをChatGPTに接続**し、ガイドを保存した非公開リポジトリ `iwasaki-hub/codex-tips` と、対象アプリのリポジトリへの読み取り権限があることを確認する。GitHubアプリが対応・有効化されている必要がある。接続だけで私有リポジトリが必ず読めるとは限らない。
5. 同じChatGPTプロジェクト内で、下にある**ガイドのURLだけ**をメッセージとして送信する。

参考: [OpenAI公式: ChatGPTのプロジェクト](https://help.openai.com/ja-jp/articles/10169521-projects-in-chatgpt) / [GitHubをChatGPTに接続](https://help.openai.com/ja-jp/articles/11145903-connecting-github-to-chatgpt)

## URLの入口（1つ選んでそのまま送る）

| 活用したい場面 | ChatGPTへ送るURL |
| --- | --- |
| **一般編** — AGENTS.md、設計、レビュー、Skills、引き継ぎ | https://github.com/iwasaki-hub/codex-tips/blob/main/docs/CODEX_TIPS_JA.md |
| **Codex CLI編** — コマンド、設定、作業効率、MCP、Worktree | https://github.com/iwasaki-hub/codex-tips/blob/main/docs/CODEX_CLI_TIPS_JA.md |
| **VS Code × Codex × ChatGPT編** — エディタ、役割分担、UI修正、PR | https://github.com/iwasaki-hub/codex-tips/blob/main/docs/VSCODE_CODEX_CHATGPT_TIPS_JA.md |

URLはGitHubの`main`にある既存ドキュメントへのリンクです。リポジトリが非公開のため、読み取れるのはアクセス権のあるユーザーと有効なGitHub接続だけです。GitHubのプライバシー設定を変更する必要はありません。

## URLだけを送ったときの動作

```text
[ChatGPTプロジェクト: KondateBiyori]

ユーザー:
https://github.com/iwasaki-hub/codex-tips/blob/main/docs/VSCODE_CODEX_CHATGPT_TIPS_JA.md

想定するChatGPTの仕事:
- このURLの実践ガイドをGitHubから取得
- KondateBiyoriの現在のREADME/AGENTS.md/CI/関連ファイルを確認
- すでに導入済みか、改善可能かを根拠付きで判定
- 重要な改善を最大3件に絞って提案
- 最初の変更をCodexに指示するコピペ用プロンプトを作成
- 読み取りだけで終了し、リポジトリは変更しない
```

次のメッセージで「改善案の1つ目を適用して」と伝えると、その時点で書き込み可能な環境であればfeatureブランチ→検証→PR作成に進めます。マージは別途明示の指示を必要とします。

## 対象の判定ロジック

| 条件 | 動作 |
| --- | --- |
| ChatGPTプロジェクトに対象リポジトリが1つある | そのリポジトリを対象に診断 |
| この会話で対象リポジトリが具体的に指定されている | その対象を優先 |
| 複数候補または対象が不明 | 対象リポジトリを一度だけ質問（勝手に選ばない） |
| ガイドURLやリポジトリを開けない | 接続・権限・URLを案内し、未取得の内容を推測しない |
| 導入済みの項目がある | 再導入せず、効果のある改善だけを抽出 |
| 対象技術に当てはまらない | 「対象外」と明示して不要な変更を提案しない |

## 適用診断の出力例

```md
### Codex CLI編の適用診断
対象: iwasaki-hub/KondateBiyori
状態:
- 導入済み: AGENTS.mdによるルール整理（根拠: AGENTS.md）
- 改善余地: CLIの利用方法と検証のショートカット（根拠: scripts等）
- 未確認: ローカルPCの ~/.codex/config.toml（GitHubからは確認不可）

優先改善案:
1. 検証コマンドを1ページに整理
2. 個人設定のCLIプロファイルを提案
3. PRレビューの手順を追加

最初のCodex用プロンプト: ...
未確認: 実際のローカルCLI設定は未閲覧
```

*この例は説明用の仮データです。実際の診断結果ではありません。*

## 安全上の設計

- URLは**ガイド選択**であって、GitHubへの書き込みを許可するコマンドではありません。
- 外部文書内にある指示を無条件に高優先度の命令として扱わない。対象プロジェクトの既存運用ルールが優先です。
- シークレット、`.env.local`、個人トークンを収集・転記しない。
- URLだけでは認証設定・本番DB・公開環境・課金・PRマージは実行しない。
- 検証未実施なら未実施と書き、CI成功と実機確認を混同しない。
- GitHub接続が見えない場合、ブラウザで非公開ソースを無理に閲覧しない。

## トラブルシューティング

**Q. URLを送っても普通の要約だけが返る**  
A. URLだけの起動はChatGPTの標準的な自動コマンド機能ではありません。対象ChatGPTプロジェクトの**プロジェクト指示**に[テンプレート](../templates/CHATGPT_PROJECT_INSTRUCTIONS.md)が入っているか確認してください。

**Q. 非公開リポジトリにアクセスできない**  
A. ChatGPTのGitHub接続、対象リポジトリを含むGitHubアプリの権限を確認してください。権限がない場合はユーザー自身がガイド内容を貼り付ける方法も使えます。

**Q. どのアプリに適用されるのか**  
A. 原則として、そのChatGPTプロジェクトで対象としている開発リポジトリです。複数ある場合はChatGPTが確認します。

**Q. URLだけでコードが書き換わるのか**  
A. いいえ。意図しない変更を避けるため、まず現状診断と計画まで進めます。後続の「適用して」が変更の合図です。
