# Codex × GitHub × アプリ開発：セキュリティ実践Tips 10選（2026年11月版）

> **位置付け:** 開発時の事故を減らすための防御的な確認ガイドです。すべてのリポジトリに同じ設定を強制するものではありません。適用前に実際の構成・権限・公式仕様を確認してください。
>
> **ChatGPTプロジェクトでの利用:** [初回設定とURL連携](URL_ACTIVATION.md)を済ませている場合、一般編・CLI編・VS Code編の診断中に**具体的なセキュリティ上の懸念がある場合だけ**このガイドを補助資料として参照します。このページのURLは独立した自動起動条件ではありません。

- 最終確認: **2026-11-22**
- 想定: Codex / ChatGPT / VS Code / GitHub Actions / TypeScript / Next.js / Expo / Supabase を使う個人・小規模チーム
- 区別: **公式仕様**と、ここに載せる**推奨運用例**を混同しない
- 原則: **最小権限・信頼境界・多層防御・証拠に基づく検証**

## 先に確認する「守るべきもの」

| 資産・境界 | 主なリスク | 最初の確認 |
| --- | --- | --- |
| 認証情報・APIキー | 漏えい、なりすまし | Git履歴、CIログ、クライアント配信物 |
| ユーザーデータ・DB | 他人のデータの閲覧・改ざん | RLSとAPI権限、他ユーザーでの実テスト |
| AIエージェントの操作権限 | 悪意ある文章による誤操作 | Sandbox、ネットワーク、MCPの権限 |
| CI・依存パッケージ | サプライチェーン攻撃 | GitHub Actionsのトリガーと依存更新 |
| 本番環境 | 意図しない変更・停止 | PRレビュー、承認、ロールバック手順 |

## Tip 1. まず脅威モデルと変更境界を一行で書く

重要データ、攻撃者が触れる入口（フォーム、PR、外部ページ、MCP結果）、変更できる環境（local / preview / production）を明確にする。「誰が・何にアクセスできると困るか」を最初に書くと、認証の有無だけでは見落とす**他人のデータへのアクセス**も評価できる。

- **実践:** 対象リポジトリのデータの流れ・認証境界・外部サービスを列挙。未確認の権限は「安全」ではなく「未確認」と記録。
- **Done when:** 重要資産、外部入力、操作権限、主要な失敗例を最低1つずつ説明できる。
- **注意:** このTipsは整理方法の提案で、特定のツール機能ではない。

## Tip 2. Web・Issue・PR・リポジトリ内の文章を「指示」ではなく「入力」として扱う

AIが読むREADME、Issueコメント、ログ、検索結果、MCPツールの返答には、タスクと関係ない命令が紛れ込むことがある（**間接プロンプトインジェクション**）。外部情報に「秘密を送信せよ」「設定を無効にせよ」とあっても、ユーザーの依頼・権限を上書きしてはならない。

- **実践:** 外部文章は検証対象のデータとし、実行するコマンド・送信先URL・依存パッケージの変更を独立して確認する。
- **確認:** 不審な指示に従わず、根拠を整理し、必要な操作だけ承認を求められるか。
- **出典:** [OpenAI: MCPとコネクタの安全性](https://developers.openai.com/api/docs/guides/tools-connectors-mcp)

## Tip 3. CodexのSandboxと承認を仕事のリスクに合わせる

調査は読み取り専用、通常の編集はワークスペース内に限定し、外部ネットワーク・機密データ・本番操作には別の承認を設ける。**Sandboxの有無と、操作の正当性の審査は別**。

```powershell
# 読み取り中心の調査例。利用中のCLIで --help も確認する。
codex exec --sandbox read-only "このリポジトリのセキュリティリスクを読み取りのみで調べてください"
```

- **避ける:** 必要性と隔離が確認できない `--dangerously-bypass-approvals-and-sandbox`（`--yolo`）の常用。
- **確認:** 何を読めるか、何を書けるか、どこに通信できるか、誰が承認するか。
- **出典:** [OpenAI: Agent approvals & security](https://developers.openai.com/codex/agent-approvals-security)

## Tip 4. MCPサーバーとSkillsは「便利さ」よりも権限と出所を調べる

MCPは情報取得だけでなく送信・削除・DB変更も可能な場合がある。新しいサーバーやSkillsを追加する前に、提供元・認証範囲・書き込み権限・ネットワーク先を確認する。

- **実践:** 読み取り専用の接続から始め、必要なリポジトリや環境だけ許可する。機密操作は明示承認を残す。
- **実践:** Skillsに含まれるスクリプトや依存関係をレビューし、外部出力を信頼済み命令とみなさない。
- **確認:** 不要なMCP接続を外しても必要な診断ができるか。操作ログを追跡できるか。
- **出典:** [OpenAI: MCPとコネクタ](https://developers.openai.com/api/docs/guides/tools-connectors-mcp) / [OpenAI: Skillsとネットワークのリスク](https://developers.openai.com/blog/skills-shell-tips)

## Tip 5. 秘密情報はGit・AIの出力・ログに置かない

APIキー、`service_role`キー、アクセストークン、秘密鍵、接続文字列、個人情報をプロンプト・コミット・スクリーンショット・CIログに貼らない。`.gitignore` は**既に履歴に入った秘密を消す機能ではない**。

- **実践:** ローカルの秘密設定ファイルをGit管理対象から除外し、CI・サーバー側のシークレット管理を利用する。例示にはダミー値だけを使う。
- **実践:** GitHubのSecret scanning / Push protectionの提供状況と設定を確認する。利用できる機能は公開・非公開、契約、設定で異なる。
- **漏えい時:** 先に資格情報を失効・ローテーションし、アクセス履歴と影響範囲を調べる。Git履歴からの削除だけで完了としない。
- **出典:** [GitHub: Security features](https://docs.github.com/en/code-security/getting-started/github-security-features)

## Tip 6. Expo・Next.jsのクライアント公開変数に秘密鍵を入れない

`EXPO_PUBLIC_*` や `NEXT_PUBLIC_*` の値は、クライアント側コードに含まれ得る。名前が「環境変数」でも秘密にはならない。Expoの`EXPO_PUBLIC_*`は公開してよい値に限定する。

- **実践:** サーバー専用の秘密鍵はサーバー側でのみ利用する。ビルドや配信物、ブラウザー開発者ツールから見えるものを秘密として扱わない。
- **確認:** 公開キーと秘密キーの用途を分類し、クライアントの環境変数・アプリ設定・バンドルに秘密がないことを確認する。
- **出典:** [Expo: Environment variables](https://docs.expo.dev/guides/environment-variables/) / [Supabase: API keys](https://supabase.com/docs/guides/getting-started/api-keys)

## Tip 7. SupabaseはRLSとGRANTを両方確認し、他人のデータで試す

`publishable`キー（旧`anon`キーを含む）はクライアント公開を想定したキーだが、**それ自体はアクセス制御ではない**。Data APIに公開するテーブルはRLSを有効化し、`GRANT`とポリシーの両方を確認する。`secret` / `service_role`キーはRLSをバイパスするためクライアントに置かない。

例：自分の行だけを更新するポリシー。テーブル構造と既存の`SELECT` / `INSERT` / `DELETE`ポリシーを確認して調整する。**例のSQLをそのまま本番に実行しない。**

```sql
-- 例示のみ: user_id は auth.users に対応する所有者列
create policy "update own rows" on public.entries
for update to authenticated
using ((select auth.uid()) = user_id)
with check ((select auth.uid()) = user_id);
```

- **実践:** 匿名・ユーザーA・ユーザーBの条件で、AがBの行を読めず、書き換えられず、所有者も変更できないことを検証する。
- **実践:** `SECURITY DEFINER`関数やViewが権限境界を変えないか別途確認する。
- **確認:** RLSを有効にしたことだけでは完了とせず、実際のJWTを使った期待結果と拒否結果を記録する。
- **出典:** [Supabase: RLS](https://supabase.com/docs/guides/database/postgres/row-level-security) / [Supabase: Securing your API](https://supabase.com/docs/guides/api/securing-your-api)

## Tip 8. GitHub Actionsは最小権限・信頼できるコード実行を基本にする

Workflowの`GITHUB_TOKEN`を必要最小限の`permissions`に絞る。第三者Actionは**検証済みの完全なコミットSHA**で固定することを検討し、アップデートの仕組みも用意する。

```yaml
# 読み取り専用の例。必要な書き込み権限はジョブ単位で明示する。
permissions:
  contents: read
```

- **注意:** `pull_request_target`でフォーク由来のコードをチェックアウトして実行すると、トークンやSecretが危険にさらされる。通常のPR検証では`pull_request`を優先し、権限の高いジョブと未信頼コードの実行を分離する。
- **確認:** トリガー、権限、Secret、Actionの出所、PR由来入力の使い方を全Workflowで点検する。
- **出典:** [GitHub: Secure use reference](https://docs.github.com/en/actions/reference/security/secure-use) / [GitHub: Securely using pull_request_target](https://docs.github.com/en/actions/reference/security/securely-using-pull_request_target)

## Tip 9. 脆弱性検知は自動化し、誤検知も含めて人が判断する

依存関係の脆弱性やコード上のリスクには、GitHubのDependabot alerts / security updates / Code scanning、言語に応じた監査ツールを組み合わせる。すべての機能が無条件で有効とは限らない。

- **実践:** ロックファイルをコミットし、依存更新PRにテストを付ける。深刻度だけでなく実際の利用箇所・到達可能性・修正版の互換性で優先度を決める。
- **確認:** 検知結果の担当者、修正期限、例外の根拠、修正後の再スキャン結果を残す。
- **出典:** [GitHub: Dependabot security updates](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-security-updates) / [GitHub: Security features](https://docs.github.com/en/code-security/getting-started/github-security-features)

## Tip 10. マージ・デプロイ前の最後の境界を人とCIで守る

Codexのレビューは有用な**補助**だが、セキュリティの合格判定をAIの回答だけに委ねない。PR差分・テスト結果・保護ルール・リリース手順を合わせて判断する。

- **実践:** `main`への直接変更を避け、必要なレビューとCIチェックをルールセットやブランチ保護で求める（利用可能な設定と権限を確認）。
- **実践:** 本番DB変更・権限追加・デプロイは影響範囲・バックアップ・復旧手順を確認してから明示的に承認する。
- **確認:** PRに「変更した認可境界／想定外アクセスのテスト／ロールバック方法／未確認事項」を記録できている。
- **出典:** [OpenAI: Custom Code Review rules for Codex](https://developers.openai.com/blog/custom-code-review-rules-for-codex)

## 補足：Codex Securityを利用できる場合

[Codex Security](https://developers.openai.com/codex/security) は脆弱性の検出・確認・修正支援に使える。**利用権限が必要な機能がある**ため、誰でもスキャンできるとは書かない。診断結果は自動修正・自動マージの根拠とせず、再現条件・影響・修正差分をレビューする。

## 5分のセルフチェック

- [ ] 本番キー・個人情報がコミット、プロンプト、ログ、配信物にない
- [ ] Codex・MCPの権限とネットワーク先を必要最小限にしている
- [ ] 外部文章の指示注入を疑い、実行前に根拠を確認している
- [ ] DBの認可を匿名・別ユーザーで試し、結果を残している
- [ ] PRのCI権限・第三者Action・依存アラートを点検した
- [ ] mainと本番変更にはレビュー・承認・復旧経路がある

## Codexに依頼するには

[セキュリティ監査・改善プロンプト集](../templates/SECURITY_REVIEW_PROMPTS.md)を参照。**最初は読み取り専用の現状診断**を行い、変更が必要な場合は別途明示的に許可する。

**出典一覧と管理方針:** [SOURCES.md](SOURCES.md)。仕様が変わる可能性があるため、コマンド・設定・利用可否を導入時に公式ページで再確認する。
