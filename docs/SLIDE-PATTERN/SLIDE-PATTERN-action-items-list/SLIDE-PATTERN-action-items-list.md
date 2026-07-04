# SLIDE-PATTERN-action-items-list

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview
**パターン名：** action-items-list
**概要：** アクションアイテム（次のステップ・タスク一覧）を番号付きリストで示すコンテンツエリア。担当者・期限カラムを持ち、会議の締めや計画発表に使いやすい。
**適したシーン：** 会議のまとめ・プロジェクトキックオフ・次のステップの共有・タスク割り当て

## Structure（構造）

```yaml
frame: standard
layout: action-items-list
content_area:
  display: flex
  flex-direction: column
  padding: "32px 96px"（高さの約3%・幅の約5%）
  gap: 0

action_label:
  text: "ACTION ITEMS"
  style: "area-label（font-size: 22px、color: #999、letter-spacing: 0.08em）"
  margin_bottom: 16px

header_row:
  display: grid
  grid_template_columns: "64px 1fr 200px 200px"
  gap: 32px
  padding: "16px 0"
  border_bottom: "2px solid #CCCCCC"
  font_size: 22px
  color: "#999"
  columns:
    - "No"
    - "アクション"
    - "担当"
    - "期限"

action_rows:
  count: 4〜5
  each:
    display: grid
    grid_template_columns: "64px 1fr 200px 200px"
    gap: 32px
    padding: "20px 0"
    border_bottom: "1px solid #F0F0F0"
    cells:
      number:
        width: 64px
        height: 64px
        background: "#F0F0F0"
        border_radius: 50%
        font_size: 24px
        font_weight: bold
        color: "#555"
        display: flex
        align_items: center
        justify_content: center
      action_text:
        font_size: 28px
        color: "#333"
      assignee:
        font_size: 26px
        color: "#555"
        background: "#F5F5F5"
        padding: "4px 16px"
        border_radius: 8px
        display: inline-block
      deadline:
        font_size: 26px
        color: "#666"
```

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 写真は「写真添付エリア（後から実際の写真に差し替え）」、アイコンは「内容に沿ったSVGアイコンを生成して配置」として役割を記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

| 要素 | 役割 | 推奨文字数・値 |
|------|------|--------------|
| ACTION ITEMSラベル | アクションアイテム一覧であることを明示（22px相当） | 固定（"ACTION ITEMS"） |
| ヘッダー行 | 各カラムの列ラベル（22px相当） | 固定（No / アクション / 担当 / 期限） |
| 番号バッジ | タスクの順番・優先順位を示す番号（24px相当） | 1桁〜2桁の数字 |
| アクション内容 | 実施すべき具体的なタスク・アクション（28px相当） | 25〜50文字 |
| 担当者 | そのタスクの責任者・担当チーム名（26px相当） | 4〜10文字 |
| 期限 | タスクの完了期日（26px相当） | 「MM/DD」または「〇月〇日」形式 |

## Usage Guide（AIへの使い方）

### プロンプト例

```
SLIDE.mdのデザインシステムと、以下のSLIDE-PATTERN-action-items-listパターンを使って
スライドを1枚生成してください。

【アクションアイテム一覧】
1. アクション: 要件定義書の最終レビューと承認
   担当: サンプル担当A（PM）
   期限: 6/10

2. アクション: デザインモックアップの作成（3画面分）
   担当: サンプル担当B（デザイン）
   期限: 6/12

3. アクション: APIエンドポイント仕様書の作成
   担当: サンプル担当C（開発）
   期限: 6/12

4. アクション: ステークホルダーへの進捗報告メール送付
   担当: サンプル担当A（PM）
   期限: 6/14

5. アクション: テスト環境のセットアップ確認
   担当: 開発チーム全員
   期限: 6/15
```

### 注意点
- アクション内容は「〜すること」「〜を行う」など動詞で終わる形式に統一する
- 担当者は個人名またはチーム名で記載し、複数担当の場合は「チーム名 全員」などと表記する
- 期限は短い形式（月/日）を使用してカラム幅を節約する
- 4〜5件が最適。6件以上になる場合は次回以降のアイテムと分けるか、別ページに分割する
