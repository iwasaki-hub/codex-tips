# Codex CLI 実践Tips 9選（2026年11月版）

> **ChatGPTでこのガイドをプロジェクトに適用:** [初回設定（1回だけ）](URL_ACTIVATION.md)の後、このページのGitHub URLをChatGPTプロジェクトに**単独で送る**と、対象コードの現状診断・優先改善案・Codex用プロンプトを作成できます。URLだけではファイルを変更しません。プロジェクト指示・GitHubアクセス権が必要です。

- 確認日: 2026-11-21
- 対象: ターミナルでCodexを使う開発者（特にExpo / React Native / TypeScript / GitHub）
- 優先順位: **OpenAI公式の仕様**を優先。**海外ユーザーの体験談**は再現性を保証しない
- コマンドはCLIのバージョン、実行環境、権限設定によって動作が異なることがあるため `codex --help` や該当コマンドの `--help` も確認する

## すぐ使えるコマンド早見表

ターミナルで実行するもの:

| コマンド | 用途 |
| --- | --- |
| `codex` | 対話型CLIを開始 |
| `codex resume --last` | 現在のディレクトリに対応する直近の会話を再開 |
| `codex fork --last` | 直近の会話を新しい会話に分岐 |
| `codex exec --sandbox read-only "調査のみしてください"` | ファイル変更を禁止した非対話調査 |
| `codex exec --json "変更点をレビューして報告"` | 実行イベントをJSONLで出力 |
| `codex exec --worktree "独立したタスク"` | 管理されたGit Worktreeで非対話実行 |
| `codex --profile routine` | プロファイル設定で起動 |
| `codex mcp list` | 設定されたMCPサーバーを表示 |
| `codex doctor` | インストール・認証・設定などの診断 |

Codex CLI**内**に入力するもの（PowerShellやbashへ直接入力するコマンドではない）:

| コマンド | 用途 |
| --- | --- |
| `/status` | 現在のセッション状態・設定確認 |
| `/model` | モデルと利用可能な推論レベルの変更 |
| `/permissions` | 現在の承認・サンドボックス設定を選択 |
| `/plan` | 実装前に計画を作るモード |
| `/review` | コードレビュー |
| `/diff` | 変更差分の確認 |
| `/compact` | 長い会話のコンテキストを要約 |
| `/usage` | アカウントの利用状況を見る（認証方式による） |
| `/statusline` | フッターにモデル・ブランチ・利用状況などを表示 |

上記のコマンド構文・適用条件は[公式 CLIコマンドリファレンス][cli-ref]を参照。

---

## Tip 1. 長い会話を整理する: /compact

```text
/compact
```

遠回りした調査・大量のエラーログで長くなった会話を圧縮する。重要な前提が残っているか要約後に確認する。未解決の問題を消してしまわないよう、最初にファイルへ状況を記録してもよい。

**出典:** [公式CLI][cli-ref]、[海外の実践例A][reddit-a]

## Tip 2. タスクに応じて設定プロファイルを変える

利用者レベルの `~/.codex/routine.config.toml` の**例**:

```toml
# 軽めの定型作業用。サポートされる値はモデル依存。
model_reasoning_effort = "low"
```

起動例:

```powershell
codex --profile routine
```

プロファイルは `~/.codex/config.toml` に上書きして適用される。リポジトリ内の `.codex/config.toml` も存在し得るが、信頼したプロジェクトだけで読み込まれ、設定の優先順位に注意する。

**注意:** 推論レベルが低いほど最終的に効率的とは限らない。手戻り・テスト合格まで含めて比較する。モデルにより `low` が受け付けられるか異なる。

**出典:** [公式設定][config]、[海外の実践例A][reddit-a]

## Tip 3. まず対象を探し、必要な箇所だけ読む

おすすめの指示例:

> 関係するファイルを `rg` で特定してから必要箇所だけ読んでください。`node_modules`、`dist`、生成物は原則除外。長いログは失敗箇所と周辺行を抽出し、全量をそのまま会話に貼らないでください。

*Codexは既に検索を優先することがあるため、AGENTS.mdに冗長な強制ルールを追加する前に既存の動作を確認する。*

**出典:** [海外の実践例A][reddit-a]、[海外の実践例B][reddit-b]（体験談）

## Tip 4. /review と /diff をセットにする

CLI内:

```text
/review
/diff
```

レビューでバグ・影響範囲・不足テストを探し、差分は人間も確認する。Codex CLIの `codex review` は非対話レビューにも使える。指摘は必ずしも正確ではないため、CIと人間の最終確認を省略しない。

**出典:** [公式CLI][cli-ref]

## Tip 5. 会話は resume で再開、fork で分岐

```powershell
codex resume --last
codex fork --last
```

`resume` は同じ会話の続き、`fork` は同じ履歴から別方針を検討する新しい会話。`--last` は原則現在の作業ディレクトリにスコープされる。

**出典:** [公式CLI][cli-ref]

## Tip 6. codex exec で小さな処理を非対話化する

**読み取り専用調査:**

```powershell
codex exec --sandbox read-only "既存のExpo画面構成を調べ、変更せずに要約してください"
```

**実行結果を機械で扱う場合:**

```powershell
codex exec --json "コードのリスクを調査し報告してください" > codex-events.jsonl
```

`--json` はイベントをJSONLとして出力する。運用に組み込む前に実際の出力・終了コード・認証方法を確かめる。JSONLには機密情報が混ざる可能性があるため、**生成ファイルをGitへ無条件にコミットしない**。

**出典:** [公式CLI][cli-ref]、[公式の自動化事例][exec-article]

## Tip 7. Worktreeで独立したタスクを分離

```powershell
codex exec --worktree "画面UIのアクセシビリティだけを調査してください"
```

管理されたWorktreeで別作業を始められる。CLI内の `/worktree` も利用できる環境がある。

- ローカル実行とGitリポジトリが前提
- `codex exec --worktree` は `resume`、`review`、`--ephemeral` 等と併用不可
- 同じファイルの並行変更を避け、統合後に再テスト

**出典:** [公式CLI][cli-ref]

## Tip 8. /usage と /statusline で測る

CLI内:

```text
/usage
/statusline
```

`/usage` はアカウント認証状態によって表示可能な利用情報が変わる。`/statusline` ではモデル名、推論、コンテキスト、Gitブランチ、トークンなどから表示項目を選べる。

**比較する指標:** 1回あたりのトークンだけでなく、同じ完了条件を満たすまでの総利用量・再試行・実行時間・最終品質。

**出典:** [公式CLI][cli-ref]、[海外の実践例B][reddit-b]

## Tip 9. MCPは必要なものを使う

```powershell
codex mcp list
```

使っていないMCPサーバーやツールは不要な呼び出し・複雑さの原因になる場合がある。設定の整理はリポジトリではなく**自分のユーザー設定**に影響することが多い。接続先を変更する前に現在の設定をバックアップする。

**出典:** [公式CLI][cli-ref]、[海外の実践例A][reddit-a]

---

## Expo / TypeScript開発に合わせたおすすめ手順

1. リポジトリで `codex` を起動する。既存の `AGENTS.md` を確認。
2. DB変更・大きな機能追加なら `/plan`。小さな修正なら目的・制約・Done whenを明示して開始。
3. 狭い範囲で実装し、TypeScript / ESLint / 関連テストを実行。実機の確認はWebビルド成功と区別。
4. `/review` と `/diff` で確認。
5. GitHub PRを作成し、リポジトリの必須CIとPreviewの結果を確認してからマージ。
6. 長期作業は既存の `WORKLOG.md` / `HANDOFF.md` に実施結果を残す。

### 安全ルール

- `--dangerously-bypass-approvals-and-sandbox` は通常開発で使わない。
- 本番DB、公開環境、秘密情報、課金、マージなどの操作は権限と影響を確認し、必要な承認を得る。
- `codex exec --worktree` は作業ツリー分離であり、外部副作用の隔離を保証するものではない。
- 検証を実行しなかった場合は未実施と明記する。

## 公式・海外コミュニティ参考資料

[cli-ref]: https://developers.openai.com/codex/cli/reference
[config]: https://developers.openai.com/codex/config-basic
[exec-article]: https://developers.openai.com/blog/codex-as-a-platform
[reddit-a]: https://www.reddit.com/r/codex/comments/1xm88mo/20_ways_to_waste_fewer_tokens_in_codex_from_basic/
[reddit-b]: https://www.reddit.com/r/codex/comments/1wi4hup/reduced_codex_tokencredit_usage/

- [OpenAI公式: CLIコマンドリファレンス][cli-ref] — コマンド構文、サブコマンド、組み合わせ上の制約
- [OpenAI公式: 基本設定][config] — config.toml、設定の優先順位、プロファイル
- [OpenAI公式: Codexをプラットフォームとして使う][exec-article] — exec・スクリプト・CI
- [Reddit: 20 ways to waste fewer tokens in Codex][reddit-a] — 海外開発者のチェックリスト。効果は保証されない
- [Reddit: Reduced Codex token/credit usage][reddit-b] — セッション・ログ・検証負荷の個人体験。掲載値は単一比較

他のTips: [Codex実践Tips 8選](CODEX_TIPS_JA.md) / [出典一覧](SOURCES.md)
