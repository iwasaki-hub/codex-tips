# Codex 実践Tips 8選（海外事例＋公式ガイド）

- 更新日: 2026-11-21
- 対象: Codexを使って個人・小規模チームで開発する人
- 目的: 品質を守りながら、指示・検証・引き継ぎの無駄を減らす
- 出典: [参考資料](SOURCES.md)（公式情報とRedditの体験談を区別）

> **注意:** 「海外で話題の方法」は万人に有効とは限りません。Redditの効果・節約率は個人の報告であり保証値ではありません。Codexのモデルや機能の変更に合わせて設定は見直してください。

## すぐ取り入れたい3つ

1. `AGENTS.md` を**短いルールと参照先**に整理し、細かい手順は必要なときだけ読ませる。
2. **複雑な作業だけ**先に計画し、「完了条件・変更禁止事項・検証方法」を明文化する。
3. **差分レビュー＋関連テスト＋PRの必須CI**で、速さと安全性を両立する。

## 01. AGENTS.mdを小さく保つ

**なぜ:** リポジトリ全体の巨大なルールを毎回読み込むと、作業に関係しない文脈や矛盾が増える。公式も不要な指示・大量のSkillsを減らし、関連資料を必要なときだけ読む方針を推奨している。[公式1][コミュニティ1]

**実践:** 共通ルール、検証コマンド、危険操作の境界、詳細資料へのリンクを簡潔に記載する。仕様や長い手順は別ファイルに移す。

**注意:** 「すべてのタスクで全設計資料を読む」は避ける。すでにルールがあるプロジェクトでは安易に置き換えない。

雛形: [AGENTS.example.md](../templates/AGENTS.example.md)

## 02. 複雑な変更はPlan First

**なぜ:** 設計やDB変更などの多段階作業では、変更前に方針・影響範囲・リスクを合わせやすい。[公式2][公式3]

**実践:** まず読み取り調査を行い、対象ファイル、実装手順、戻し方、検証を整理する。CodexのPlan modeが使える環境なら活用する。

**注意:** 誤字修正や単純な1行変更まで大きな計画を要求しない。計画だけの段階と実装段階を区別する。

## 03. 「Done when（完了条件）」を定義する

**なぜ:** 「画面を作る」だけでは、何が成功か判断できない。ゴール・制約・確認できる結果があると、作業の終了判定が明確になる。[公式3][公式4]

**実践:** 次の4点をセットで指示する。

- Goal: 何を達成するか
- Context: 関連する画面、ファイル、現状
- Constraints: 変更しない場所、追加禁止の依存など
- Done when: テスト、UI条件、確認項目

例: 「週間献立を表示。既存の3タブとデザイントークンは維持。新規依存は追加しない。型チェックとLintが通り、狭い画面で横はみ出しがないこと。」

## 04. 変更規模に合わせて検証を選ぶ

**なぜ:** 毎回すべての重いテストを実行すると、時間・実行量を使う。コミュニティでは影響のある検証から実行する工夫が共有されている。[コミュニティ2]

**実践例:**

| 変更 | 作業中の検証 |
| --- | --- |
| ドキュメント・文言 | 形式確認、必要に応じて型・Lint |
| UI/コンポーネント | TypeScript、ESLint、関連画面の目視 |
| ロジック | 上記＋関連単体テスト |
| 認証・DB・RLS | 権限/境界ケースのテスト、移行手順と戻し方 |
| リリース前 | プロジェクト規定の全必須CI＋実機などの必要な確認 |

**注意:** **マージ前の必須CIは省略しない。** 成功していない検証は「未実施」「失敗」と明示する。実行コスト削減の数字は環境依存。

## 05. 実装と別視点のレビューを分ける

**なぜ:** 実装後に差分を見直すと、既存機能への影響、誤った前提、足りないテストが見つかりやすい。Codex Code Reviewはリポジトリ独自のルールも使える。[公式5]

**実践:** Codexに「修正せずレビューのみ」を依頼し、重大度・影響ファイル・再現条件・不足テストを列挙させる。指摘を人間が確認した後に修正する。

**注意:** AIレビューはCI・人間の判断の代わりではない。誤検知や見逃しがある。

## 06. 反復作業だけをSkills化する

**なぜ:** Skillsは何度も使う検証・レビュー・リリース手順を再現しやすい。公式のOSS事例でもSkills＋GitHub Actionsによる整備が紹介されている。[公式1][公式6]

**候補:** `expo-check`、`pr-review`、`supabase-migration` など。

**実践:** 同じ手順を何度か繰り返した後でSkillを作る。説明文は「いつ使うか」を絞り、詳細は補助ファイルへ。Skillの導入自体が目的にならないようにする。

**注意:** Skillを大量作成すると説明文の競合・文脈肥大化を招く。Skillは自動実行・自動マージの許可ではない。

## 07. Git Worktreeで並列作業を分離する

**なぜ:** 同じ作業ツリーで複数のCodexが編集すると競合しやすい。Worktreeなら作業ツリー・ブランチを分けられる。[公式3][コミュニティ3]

**例:** AはUI、Bはテスト、Cは読み取り専用調査を担当。共有ファイルや生成物を同時に変更しないよう担当範囲を分ける。

**注意:** 並列化によりレビュー負荷と統合コストは上がる。小規模変更なら1作業ずつの方が安全なこともある。統合後は再テストする。

## 08. 長期作業は引き継ぎファイルに残す

**なぜ:** 会話が長くなると決定事項や現在地が追いづらくなる。公式の長期タスク事例とコミュニティの体験談で、仕様・計画・進捗をファイルに分けて記録する方法が紹介されている。[公式3][コミュニティ4]

**実践:** 既存の `WORKLOG.md` や `HANDOFF.md` があるなら活用する。別名の `STATE.md` を重複して作らない。

最低限の記録:
- 現在のブランチと対象PR
- 完了した作業と未完了の作業
- 決定事項と理由
- 実際に実行したテストと結果
- 次の一手とブロッカー

**注意:** 会話の要約を事実とみなさず、最新のリポジトリ・CI・デプロイ結果で確認する。

---

## Expo / React Native / Supabase開発での適用順

1. 各リポジトリの既存 `AGENTS.md` を読み、必要な共通ルールだけ残す（勝手に改変しない）。
2. [コピー用プロンプト](../templates/PROMPTS.md)から「調査・計画」「実装」「レビュー・引き継ぎ」を使う。
3. Expo/TypeScriptの型・Lint・関連テストを実施。PR前は規定の全CIを実施。
4. Supabase移行やRLS変更では、テスト環境での検証・適用対象・バックアップ/復旧手順を確認し、本番への適用は明示承認後に行う。
5. 繰り返し手順が固まってからSkill化し、並列作業は独立した変更に限定する。

## 更新時のチェック

- モデル・Codex機能が変わっても当てはまるか
- 公式推奨か、海外ユーザーの体験談か
- 既存開発ルールと矛盾しないか
- 成功・未実施・失敗の境界が明確か
- 出典URLが有効か

### 参考資料（本文の番号に対応）

- [公式1] [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
- [公式2] [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
- [公式3] [Run long horizon tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex)
- [公式4] [Using Goals in Codex](https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex)
- [公式5] [Custom Code Review rules for Codex](https://developers.openai.com/blog/custom-code-review-rules-for-codex)
- [公式6] [Using skills to accelerate OSS maintenance](https://developers.openai.com/blog/skills-agents-sdk)
- [コミュニティ1] [Reddit: How to make codex work fantastically](https://www.reddit.com/r/codex/comments/1u97eq0/how_to_make_codex_work_fantastically/)
- [コミュニティ2] [Reddit: I cut Codex token usage ~50% with one AGENTS.md rule](https://www.reddit.com/r/codex/comments/1t6iulo/i_cut_codex_token_usage_50_with_one_agentsmd_rule/)
- [コミュニティ3] [Reddit: How are you using multiple agents and worktrees?](https://www.reddit.com/r/codex/comments/1q3496o/how_are_you_using_multiple_agents_and_worktrees/)
- [コミュニティ4] [Reddit: PROMPT.md + STATE.md](https://www.reddit.com/r/codex/comments/1vw6po6/a_simple_trick_that_made_long_ai_agent_tasks_way/)

関連リンクの分類・注意書きは [SOURCES.md](SOURCES.md) に記載。
