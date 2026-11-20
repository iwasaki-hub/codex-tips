# 参考資料・出典管理

確認日: 2026-11-21

**分類**
- 公式: OpenAIが公開したガイド・開発事例。機能仕様や推奨の裏づけとして優先する。
- コミュニティ: Reddit等での個人の工夫・感想。効果は環境依存であり、一般化しない。
- このリポジトリの雛形は**紹介した知見を基にした提案**であり、各プロジェクトに適用済みとは限らない。

## 公式情報

| 資料 | 採用した知見 |
| --- | --- |
| [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra) | AGENTS.mdとSkillsの肥大化を避け、必要な情報を必要なときだけ参照する。古いモデル向け指示を見直す |
| [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex) | AGENTS.md、計画文書、ビルド単位、実行証跡の運用 |
| [Run long horizon tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex) | Plan→実装→テスト→観察→修正、完了条件、Worktree、長期作業の記録 |
| [Using Goals in Codex](https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex) | 複数ステップ作業で達成可能なゴールを明示 |
| [Custom Code Review rules for Codex](https://developers.openai.com/blog/custom-code-review-rules-for-codex) | リポジトリ固有のコードレビュー基準 |
| [Using skills to accelerate OSS maintenance](https://developers.openai.com/blog/skills-agents-sdk) | SkillsとCIを使い反復ワークフローを標準化 |

## 英語圏の開発者コミュニティ

| 投稿 | 採用した知見・留保 |
| --- | --- |
| [How to make codex work fantastically](https://www.reddit.com/r/codex/comments/1u97eq0/how_to_make_codex_work_fantastically/) | AGENTS.mdと指示を簡潔にするという体験談 |
| [I cut Codex token usage ~50% with one AGENTS.md rule](https://www.reddit.com/r/codex/comments/1t6iulo/i_cut_codex_token_usage_50_with_one_agentsmd_rule/) | 大きなツール出力の抑制と適切なテスト選択。約50%は投稿者の条件下の自己報告 |
| [How are you using multiple agents and worktrees?](https://www.reddit.com/r/codex/comments/1q3496o/how_are_you_using_multiple_agents_and_worktrees/) | Worktree・複数エージェント運用の議論。一般的な正解ではない |
| [PROMPT.md + STATE.md](https://www.reddit.com/r/codex/comments/1vw6po6/a_simple_trick_that_made_long_ai_agent_tasks_way/) | セッションを越える進捗・意思決定の保存。個人の体験談 |

## Codex CLI編の追加参考資料

CLIコマンドの具体的な例・安全上の注意は [Codex CLI 実践Tips 9選](CODEX_CLI_TIPS_JA.md) にまとめています。

| 資料 | 種別 / 採用した知見 |
| --- | --- |
| [Codex CLIコマンドリファレンス](https://developers.openai.com/codex/cli/reference) | 公式: exec、resume、fork、Worktree、スラッシュコマンドの構文と制約 |
| [Codex Config basics](https://developers.openai.com/codex/config-basic) | 公式: プロファイル、ユーザー/プロジェクト設定、優先順位 |
| [Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform) | 公式: 非対話実行・自動化と構造化出力 |
| [20 ways to waste fewer tokens in Codex](https://www.reddit.com/r/codex/comments/1xm88mo/20_ways_to_waste_fewer_tokens_in_codex_from_basic/) | コミュニティ: 指示の整理、MCP、プロファイル、テスト選択。保証された削減率ではない |
| [Reduced Codex token/credit usage](https://www.reddit.com/r/codex/comments/1wi4hup/reduced_codex_tokencredit_usage/) | コミュニティ: コンテキスト管理と検証手順の工夫。単一比較からの報告 |

## メンテナンス方針

1. 事実の根拠は公式ガイドを優先する。
2. 新モデルや機能の登場時は、以前の「必須手順」が過剰になっていないか見直す。
3. 効果・節約率を保証するような表現は使わない。
4. 発言の日付や前提条件と、現時点のCodex機能を混同しない。
5. 出典が消えた場合は未確認として扱い、実在を推測しない。

本文: [Codex 実践Tips 8選](CODEX_TIPS_JA.md)
