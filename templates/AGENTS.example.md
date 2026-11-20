# AGENTS.md 雛形（コピー用）

> これは**雛形**です。`codex-tips` 自体のAGENTS.mdではありません。対象プロジェクトにコピーする前に、既存のAGENTS.md・開発ルールを確認し、必要な項目だけ採用してください。

```md
# AGENTS.md

## Project
- 目的: [アプリの目的を1〜2行で記入]
- Stack: Expo SDK 58 / React Native / TypeScript
- 詳細仕様: [該当する設計書の相対パス]

## Scope
- 変更範囲を依頼に必要な部分に限定する。
- 無関係なリファクタリングや依存追加をしない。
- 既存の実装・設計と現在のブランチ状態を優先する。

## Validation
- 変更箇所に関連する型チェック・Lint・テストを実行する。
- PR前にはリポジトリの必須CIをすべて通す。
- 実行していない確認を成功と書かない。
- 実機検証とWebビルド成功を混同しない。

## Safety
- secrets / .env.local をコミットしない。
- 本番DB変更、課金、外部公開、破壊的操作は事前承認を得る。
- mainへの直接Pushやマージはリポジトリ運用ルールに従う。

## Context
- 設計変更時は [設計書のパス] を参照。
- DB変更時は [DB設計・RLS文書のパス] を参照。
- デプロイ時は [公開手順のパス] を参照。
- 長期タスクは既存の WORKLOG.md / HANDOFF.md を更新する。
```

## 運用のコツ

- 項目を増やすより、不要な指示を削る。
- すべての設計書を毎回読むようには指示しない。
- 実際の検証コマンドは各リポジトリのpackage.jsonを確認して記載する。
- 危険な境界は明確にし、安全なローカル検証には十分な自律性を与える。
- 定期的に不要になったルールを削除する。

参考: [Codex 実践Tips](../docs/CODEX_TIPS_JA.md) / [公式: Rethinking skills and prompts](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
