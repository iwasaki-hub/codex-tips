# UIデザイン設計 実践Tips 10選（海外の公開リソースを使う）

- **確認日:** 2026-11-21
- **対象:** Codex / ChatGPT を使って Web・Next.js・React・iPhone向けUIを設計・実装する開発者
- **目的:** 見た目の模倣だけでなく、使いやすさ・一貫性・アクセシビリティまで根拠を持って設計する
- **位置づけ:** 海外発の公式ガイド・研究機関・デザインシステム・実装ドキュメント・参考ギャラリーを用途別に整理した資料。利用者数や「海外での人気順」のランキングではない

> **使い方:** まず設計原則とプラットフォーム指針を確認 → 画面事例を見る → デザイントークンと状態を設計 → 実装・検証する。掲載サイトのUIをそのまま複製することや、ギャラリーを仕様・安全基準として扱うことは避ける。情報の公開元と使いどころを区別する。
>
> **ChatGPTのURL自動適用について:** このページは新しい参考資料です。既存の[URL連携](URL_ACTIVATION.md)の対象は一般編・CLI編・VS Code編の3つのままであり、このURLだけを送って自動診断・編集が有効になるわけではありません。リポジトリのファイル変更・PR・マージ・デプロイは、それぞれ必要な明示指示と権限に従います。

## まず開きたい資料（用途別）

| 目的 | 資料・公開元 | 使いどころ |
| --- | --- | --- |
| iPhoneアプリの設計 | [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/) / Apple | iOSらしい導線・レイアウト・アクセシビリティ |
| Android系の設計 | [Material Design 3](https://m3.material.io/) / Google | デザインシステムとコンポーネント設計。閲覧にJavaScriptが必要 |
| 基本のUX判断 | [10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) / Nielsen Norman Group | 状態のフィードバック、用語の分かりやすさ、エラー回復などをレビュー |
| 認知・選択の負担 | [Laws of UX](https://lawsofux.com/) / Jon Yablonski | 選択肢・情報量・押しやすさに関する設計仮説を立てる |
| Webのアクセシビリティ基準 | [WCAG 2.2](https://www.w3.org/TR/WCAG22/) / W3C | 文字コントラスト、フォーカス、操作ターゲットなどの達成基準 |
| タブ・ダイアログの実装 | [WAI-ARIA APG](https://www.w3.org/WAI/ARIA/apg/) / W3C | キーボード操作、role、状態・フォーカスの扱い |
| レスポンシブWeb | [Learn Responsive Design](https://web.dev/learn/design) / web.dev | 横幅、文字、レイアウト、国際化への適応 |
| 検証済みのパターンを学ぶ | [GOV.UK Design System](https://design-system.service.gov.uk/) / 英国政府 | フォーム・エラー・タスク別の具体的なパターン |
| トークンとコンポーネント | [U.S. Web Design System](https://designsystem.digital.gov/) / 米国政府 | カラー、余白、再利用可能なUI、アクセシビリティ |
| 実際の画面を観察する | [Mobbin](https://mobbin.com/) / 画面事例集 | iOS・Webの画面やフローの参考例。利用範囲はプランによる |
| Reactの実装例 | [Radix Primitives](https://www.radix-ui.com/primitives) / Radix | スタイルを自由に設計するアクセシブルなUI部品 |
| ReactのUIひな形 | [shadcn/ui](https://ui.shadcn.com/docs) / shadcn | 変更可能なコンポーネントコードとテーマ設計 |
| トークン運用 | [Design Tokens](https://www.figma.com/resource-library/design-tokens/) / Figma | 色・余白・文字・意味付けを変数で管理 |
| UI状態ごとの確認 | [Storybook](https://storybook.js.org/docs/writing-stories) / Storybook | ボタン・フォームなどの状態を個別に表示し、レビュー |
| ECサイトのUX調査 | [Baymard UX Research](https://baymard.com/research-articles) / Baymard Institute | ECのナビゲーション、検索、商品、購入フローの根拠。一般アプリには安易に拡張しない |

## 01. 最初に「この画面で何を達成するか」を決める

**参考:** [Nielsen Norman Groupの10原則](https://www.nngroup.com/articles/ten-usability-heuristics/) / [Laws of UX](https://lawsofux.com/)

**実践:** 色やグラデーションの前に、ユーザー・主要タスク・最初の行動・成功した状態を1〜2文にする。1画面で主役にする行動を絞り、優先度の低い情報は下へ送る。

**レビュー:** 現在地と次の操作が分かるか、用語が利用者の言葉か、何かが失敗したときに戻れるか。心理学の原則は設計仮説であり、ユーザーテストの代替ではない。

## 02. 参考画面は「見た目」よりもパターンで比較する

**参考:** [Mobbin](https://mobbin.com/) / [GOV.UK Patterns](https://design-system.service.gov.uk/patterns/)

**実践:** 目的が近い3例ほどを比較し、①情報の順序、②ナビゲーションの位置、③主要CTA、④空・エラー・完了時の状態をメモする。どの仕組みが自分のユーザーに合うかを説明してから採用する。

**注意:** ギャラリーは**インスピレーション**。掲載例がWCAG準拠・使いやすさ検証済みとは限らない。スクリーンショット・ロゴ・画像・アイコンを転用する前にライセンス・利用規約を確認する。

## 03. iPhone優先でも、幅と入力方法を固定しない

**参考:** [Apple HIG](https://developer.apple.com/design/human-interface-guidelines/designing-for-ios) / [web.dev Responsive Design](https://web.dev/learn/design)

**実践:** 狭い画面で最も重要な要素から配置。長い日本語、文字の拡大、横向き、ソフトウェアキーボード、Safe Area、固定ヘッダー・下部ナビとの重なりを確認する。WebではCSSメディアクエリや柔軟なレイアウトを用い、特定のiPhoneのピクセル数だけを前提にしない。

**注意:** Apple HIGはiOS設計の指針。Webとネイティブアプリのコンポーネントは同一ではない。

## 04. カラー・余白・タイポグラフィをトークン化する

**参考:** [Figma Design Tokens](https://www.figma.com/resource-library/design-tokens/) / [USWDS](https://designsystem.digital.gov/)

**実践:** `primary`、`text`、`surface`、`border`、`spacing`、`radius`などの**意味を持つ名前**を設計する。個々のコンポーネントに同じ色コードを繰り返さず、状態やテーマでまとめて変更できるようにする。

**レビュー:** 「オレンジのボタン」が必要なのか、「primary actionを強調する」が本当の目的なのか。デザイントークンは変更を楽にするための仕組みであり、最初から巨大な設計システムを作る必要はない。

## 05. 見た目より先に操作・状態・フィードバックを定義する

**参考:** [GOV.UK Components](https://design-system.service.gov.uk/components/) / [NN/gの10原則](https://www.nngroup.com/articles/ten-usability-heuristics/)

**実践:** 重要なUIを、通常・hover・focus・pressed・disabled・loading・empty・error・successの状態で確認する。特にログイン・送信・削除などは「押した後に何が起こるか」を仕様として明記する。

**注意:** **押しても何も起こらないボタン**を完成品として扱わない。機能が未実装なら「準備中」と伝え、実際の操作ができるように誤認させない。

## 06. アクセシビリティは最後の修正ではなく設計条件にする

**参考:** [WCAG 2.2](https://www.w3.org/TR/WCAG22/) / [WAI-ARIA APG](https://www.w3.org/WAI/ARIA/apg/)

**実践:** WCAG 2.2 AAを目安に、通常の文字のコントラスト比**4.5:1以上**、大きな文字は**3:1以上**を検証する。操作ターゲットはWCAG 2.2 AAの**24×24 CSS px**（例外あり）を下回らないようにし、実用上は44〜48px程度の余裕を優先する。44×44は「Enhanced」の別基準で、AAの必須サイズと混同しない。

**レビュー:** キーボードのみで操作・フォーカス確認できるか、色だけで状態を示していないか、スクリーンリーダーで役割が分かるか。タブでは[APGのTabs Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/)を参考に、矢印キー・選択状態・パネル関連付けを設計する。

## 07. デザインシステムは「借りて調整」し、前提を混同しない

**参考:** [Material Design 3](https://m3.material.io/) / [Radix Primitives](https://www.radix-ui.com/primitives) / [shadcn/ui](https://ui.shadcn.com/docs)

**実践:** デザイン原則の参考、実装コードの採用、見た目のコピーは別の判断。Next.js/Reactのダイアログやタブなどはアクセシブルな実装を調べ、既存のUI・デザイントークンと整合する最小範囲で導入する。

**注意:** Radix・shadcn/uiは主に**Web/React向け**。Expo/React Nativeでそのまま動くと想定しない。既存プロジェクトにコンポーネントライブラリを勝手に追加しない。

## 08. 画面を「コンポーネントとその状態」でレビューする

**参考:** [Storybook](https://storybook.js.org/docs/writing-stories) / [Figma Design Tokens](https://www.figma.com/resource-library/design-tokens/)

**実践:** ボタン、フォーム、カード、タブなどを単独で確認。通常状態だけでなく、長いラベル・エラーメッセージ・空データ・読み込み中・フォーカス状態を見せる。小規模アプリならStorybookを追加する前に既存のPreviewやテスト画面で同じ確認を行ってもよい。

## 09. 自動テストと人間の画面確認を混同しない

**参考:** [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) / [WCAG 2.2](https://www.w3.org/TR/WCAG22/) / [web.dev](https://web.dev/learn/design)

**実践:** 型チェック・Lint・ビルドに加えて、ブラウザのレスポンシブ表示、キーボード操作、必要なら実機でタップ・スクロール・色・余白を確認する。Lighthouseなどの監査は問題の発見に役立つが、監査スコアだけで使いやすさやWCAG適合を保証しない。

**報告:** 「CI成功」「Previewビルド成功」「iPhone Safari操作確認」は別の結果として記載。実施していない項目をPASSと書かない。

## 10. デザイン更新は1回で確定せず、比較・検証・修正する

**参考:** [Apple HIG](https://developer.apple.com/design/human-interface-guidelines/) / [NN/gの10原則](https://www.nngroup.com/articles/ten-usability-heuristics/)

**実践:**
1. 目的・対象ユーザー・主要タスク・既存コードの制約を定義
2. 参考資料から2〜3方向のワイヤーフレーム・案を比較
3. 選んだ案について色・文字・余白・操作・状態を決定
4. 小さなブランチで実装してCIとPreviewを検証
5. iPhoneなど実際の利用環境で確認し、必要箇所だけ修正
6. PRで変更点・リスク・未確認事項を整理し、明示承認後にマージ

## Codex / ChatGPTへ渡すコピー用プロンプト

### A. まずは設計レビューのみ（ファイル変更なし）

```text
対象: [リポジトリ / 画面]
ユーザーと主な目的: [...]
参考: [URLやスクリーンショット]
制約: [ブランド、既存タブ、認証、依存追加、変更禁止事項]

既存のREADME、AGENTS.md、画面、スタイル、実装済み操作を確認し、
次の観点で現状と改善案を整理してください。
1. 情報の優先順位・主要操作・ナビゲーション
2. レスポンシブ・文字拡大・Safe Area
3. カラー/余白/文字とトークンの一貫性
4. WCAG 2.2とWAI-ARIA APGに照らした操作性
5. loading / empty / error / success等の状態
6. 実装影響・検証方法・Done when

参考サイトの見た目はコピーせず、採用理由を説明してください。
改善案は最大3案、根拠・リスク・検証項目付きで提示してください。
この段階ではコード変更、PR、マージ、デプロイはしないでください。
```

### B. 選択後の実装（別途、明示的な依頼がある場合のみ）

```text
選択したデザイン案: [...]
対象ブランチ: feature/[...]
変更する画面と変更しない箇所: [...]
既存のデザイントークン・導線・アクセシビリティを確認し、
最小の差分でレスポンシブUIを実装してください。
状態・キーボード操作・コントラスト・狭い画面を検証し、
実際の lint / typecheck / test / build の結果を区別して報告。
Preview/実機確認の未実施項目も明記してPRを作成してください。
mainへの直接Push・PRマージ・本番デプロイはしないでください。
```

## 更新・利用時の注意

- **出典の種類を区別:** Apple・W3C・政府系Design Systemは公式ガイド／基準、NN/g・Baymardは調査・専門的知見、Mobbinは実例ギャラリー、Radix・shadcn/ui・Storybookは実装支援。すべてが同じ意味での「標準」ではない。
- **人気と有効性を混同しない:** 「海外の開発者全員が使う」「この見た目なら成果が上がる」とは主張しない。利用する製品・利用者で検証する。
- **アクセス条件とリンク:** ギャラリーや記事には登録・有料部分がある場合がある。リンクや仕様は今後更新され得る。
- **著作権:** インスピレーションと、素材・画面をそのまま再配布することは異なる。利用条件を確認する。
- **自動適用はしない:** このTipsは参照資料であり、対象リポジトリのUIを自動変更しない。

関連: [Codex一般Tips](CODEX_TIPS_JA.md) / [VS Code × Codex × ChatGPT編](VSCODE_CODEX_CHATGPT_TIPS_JA.md) / [出典一覧（Codex中心）](SOURCES.md)
