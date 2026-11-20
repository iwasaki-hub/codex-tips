# VS Code × Codex × ChatGPT 実践Tips 8選

- 確認日: 2026-11-21
- 対象: VS Code・Codex拡張機能・ChatGPTで個人/小規模アプリを開発する人
- 主な利用例: Windows / Expo / React Native / TypeScript / GitHub
- 情報の区分: **公式に確認できる機能**と**海外開発者の運用事例（個人の体験談）**を区別する

## まず結論: 役割分担を固定する

| 役割 | 主に使うもの | 成果物 |
| --- | --- | --- |
| 仕様・設計・調査 | ChatGPT | 目的、制約、タスク分割、完成条件 |
| 既存コードの調査・実装・検証 | VS Code + Codex拡張機能 | 最小のコード差分、型チェック、Lint、テスト結果 |
| 品質管理・履歴・公開 | GitHub / CI / ホスティング | Pull Request、チェック結果、レビュー記録 |

**注意:** ChatGPTとCodexは常に同じ会話・作業状態を自動共有しているわけではない。実装指示は明示的に渡し、ファイルの最新状態は対象リポジトリで確認する。

---

## 01. 開いているファイル・選択範囲を活用する

**公式に確認できること:** Codex IDE拡張機能は、開いているファイル、選択したコード、最近のチャットをコンテキストとして参照できる。変更内容はエディタで差分確認できる。[公式: Codex IDE]

**手順:** 問題のある画面や関数を開く → 必要部分を選択 → 対象と変更しない範囲を指示する。

例:

> 選択した関数だけを修正してください。呼び出し元の型・公開APIは維持。変更理由を説明し、関係するテストだけ実行してください。

**注意:** 選択したコード以外に影響がある場合は、関連する呼び出し元も調査する。Codexが参照しているファイルを推測で決めつけない。

## 02. 複雑な機能は実装前に計画する

**公式に確認できること:** CodexはIDEで計画モードを利用できる。長期タスクでは完了条件と段階的な検証が重要。[公式: 長期タスク]

**使い分け:**
- 単純な文言・スタイル修正: 小さな変更を直接依頼
- DB、認証、複数画面の変更: 調査 → 計画 → 実装 → 検証の段階を分ける

例:

> まず読み取りだけで現状を調査。変更対象、実装順、影響範囲、テスト方法、復旧案をまとめてください。この段階ではファイルを書き換えないでください。

## 03. 1回の依頼は1機能・1完成条件に絞る

**実践パターン:** 海外開発者コミュニティでは、大きな依頼を小さく分割してレビューする方法が紹介されている。[海外の開発例1]

悪い例: 「献立アプリを完成させて」

改善例:

> 今週画面に既存のレシピデータを一覧表示してください。買い物操作・AI生成・保存機能は実装しない。既存3タブとデザイントークンを維持。型チェック・Lintに合格し、狭いiPhone幅で横はみ出しがないことを完成条件にしてください。

**ポイント:** 速さよりも、差分のレビュー可能性と戻しやすさを優先する。タスクが独立していれば、別ブランチやWorktreeも検討する。

## 04. AGENTS.mdと引き継ぎ文書を分けて運用する

**公式に確認できること:** Codexは実行時にプロジェクトなどのAGENTS.mdを読み、指示を適用する。[公式: AGENTS.md]

**おすすめの配置:**
- AGENTS.md: 技術スタック、ルール、禁止事項、テストコマンド、詳細文書の参照先
- docs/development/HANDOFF.md: 次にやること、未実施、ブロッカー
- docs/development/WORKLOG.md: 作業履歴と検証結果

**注意:** 既存の記録があるプロジェクトにSTATE.mdなどの類似ファイルを無駄に増やさない。AGENTS.mdは必要なルールだけを短く書く。海外の短文化の効果報告は個人の経験であり普遍的ではない。[海外の開発例2]

## 05. VS Codeの差分画面で変更を吟味する

**公式に確認できること:** Codex IDE拡張機能は編集内容を差分で確認できる。[公式: Codex IDE]

必ず見る場所:
1. 依頼と関係のないファイルが変わっていないか
2. 新しい依存関係・設定・環境変数が増えていないか
3. 認証情報、秘密鍵、生成物が混入していないか
4. 既存のUI・テスト・型定義を不用意に削除していないか

差分を検査した後、「この変更は残し、このファイルへの変更は戻す」などの追加指示を出す。

## 06. 実装と検証を1セットにする

**実践パターン:** コードを書くだけで完了にせず、実際の検証結果を報告させる。

Expo / TypeScriptアプリなら、まず対象のpackage.jsonのscriptsを確認した上で、該当するコマンドを実行する。

例:
- 型チェック: npm run typecheck（scriptがある場合）
- Lint: npm run lint
- 関連テスト: npm test または該当する個別コマンド
- Webビルド: npm run build:web（scriptがある場合）
- Expo環境確認: npx expo-doctor（必要な場面）

**区別する:** Webビルド成功 ≠ iPhone実機表示の確認 ≠ iOSネイティブビルドの成功。未実施をPASSと記録しない。PRマージ前はリポジトリが定める必須CIをすべて通す。

## 07. 短い変更はローカル、長い作業はCloudも検討する

**公式に確認できること:** IDE拡張機能から、短いローカル作業と長いクラウド側のタスクを使い分けられる。[公式: Codex IDE]

運用例:
- ローカル: UIの小変更、選択範囲の修正、即座に結果を確認したい作業
- Cloud: 独立した調査、広い範囲のテスト作成、まとまった実装案の準備

**注意:** 接続・プラン・プロジェクト設定・権限の条件は環境ごとに異なる。Cloudでの結果もPRとテストでレビューし、本番公開や秘密情報の使用を無条件に許可しない。

## 08. WindowsのCodexとmacOSのChatGPT連携を区別する

**公式に確認できること:**
- Windows向けCodexにはネイティブのサンドボックス設定とWSL利用の選択肢がある。[公式: Windows設定]
- **ChatGPTの「Work with Apps」用VS Code拡張機能は、ChatGPT macOSアプリ用**。VS Codeで動くCodex IDE拡張機能とは別。[公式: Work with Apps]

**Windowsでの現実的な構成:**
1. VS CodeでGitHubリポジトリを開く。
2. Codex IDE拡張機能でコードを読む・変更する。
3. ChatGPTで設計や問題整理を行い、計画を必要部分だけCodexへ渡す。
4. AGENTS.md、HANDOFF.md、PRの最新状態を共通の情報源にする。
5. Codexの実行権限を確認し、破壊的な操作・本番変更は明示承認で進める。

**注意:** ChatGPTにVS Codeの現在の開いているファイルが必ず自動共有されるとは考えない。

---

## 推奨する実践ワークフロー（KondateBiyoriの例）

1. **ChatGPT:** 次の機能を定義して1PRの粒度に分割。制約・Done whenを作成。
2. **VS Code/Codex:** 最新main、ブランチ、差分、AGENTS.md、仕様書を確認する。
3. **Codex:** 必要なファイルだけ編集。変更範囲が逸脱していないか差分を確認。
4. **VS Code:** npmのscriptに沿って型・Lint・関連テストを実行。
5. **GitHub:** PR → Actionsと必要なPreviewを確認 → 人間がレビュー → 承認されたマージ。
6. **記録:** WORKLOG.md・HANDOFF.mdを事実ベースで更新。

### コピー用プロンプト

[VS Code × Codex × ChatGPT 専用プロンプト集](../templates/VSCODE_CODEX_CHATGPT_PROMPTS.md)

## 公式資料・海外ユーザーの事例

### 公式
- [Codex IDE拡張機能](https://developers.openai.com/codex/ide) — 開いているファイルや選択範囲を使う、エディタ内差分、クラウドへの委任
- [AGENTS.md](https://developers.openai.com/codex/guides/agents-md) — プロジェクト指示の読み込み
- [Run long horizon tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex) — 計画・段階的検証
- [Codex設定リファレンス](https://developers.openai.com/codex/config-reference) — Windows用サンドボックスなどの設定
- [Work with Apps用VS Code拡張機能](https://help.openai.com/ja-jp/articles/10128592-how-to-install-the-work-with-apps-visual-studio-code-extension) — **macOSのChatGPTアプリ向け**であること
- [Work with Apps on macOS](https://help.openai.com/ja-jp/articles/10119604-work-with-apps-on-macos) — 対応環境・動作

### 海外開発者の体験談
- [Reddit: My best workflow so far for building projects](https://www.reddit.com/r/codex/comments/1tf4s07/my_best_workflow_so_far_for_building_projects/) — 小さなタスク分割と設計の工夫（個人の体験）
- [Reddit: Is there a better way to use ChatGPT + Codex?](https://www.reddit.com/r/codex/comments/1vtmmjt/is_there_a_better_way_to_use_chatgpt_codex/) — 役割分担と連携の議論（効果は保証されない）
- [Reddit: How to make codex work fantastically](https://www.reddit.com/r/codex/comments/1u97eq0/how_to_make_codex_work_fantastically/) — AGENTS.mdの扱い（個人の意見）

他のガイド: [一般Tips](CODEX_TIPS_JA.md) / [CLI編](CODEX_CLI_TIPS_JA.md) / [出典一覧](SOURCES.md)
