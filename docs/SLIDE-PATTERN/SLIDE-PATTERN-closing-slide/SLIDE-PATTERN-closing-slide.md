# SLIDE-PATTERN-closing-slide

このファイルはスライド全面を使うレイアウトパターン定義書です。タイトルエリア・フッターは使用しません。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。文言はすべてサンプル（ダミーテキスト）です。

## Overview
**パターン名：** closing-slide
**概要：** プレゼンテーションの最終スライド。お礼メッセージ、会社名/発表者名、連絡先を中央に配置する締めくくりのスライド。
**適したシーン：** プレゼンテーションの最終ページ、セミナー・勉強会の終了スライド、提案資料の末尾

## Structure（構造）

```yaml
structure:
  frame: none
  layout: closing-slide
  content_area:
    type: full_size
    width: 1920px
    height: 1080px
    direction: column
    align: center
    justify: center
    background: "#FFFFFF"
    elements:
      - type: accent_line
        width: 96px
        height: 4px
        color: "#CCCCCC"
      - type: main_message
        text: "ご清聴ありがとうございました"
        font_size: 56px
        font_weight: bold
        color: "#333333"
        text_align: center
      - type: sub_message
        text: "Thank you for your attention"
        font_size: 28px
        color: "#999999"
        text_align: center
      - type: divider
        width: 640px
        height: 2px
        color: "#EEEEEE"
      - type: presenter_info
        font_size: 28px
        color: "#555555"
        text_align: center
        content: "会社名 / 発表者名"
      - type: contact_info
        font_size: 24px
        color: "#888888"
        text_align: center
        content: "メールアドレス・連絡先"
```

## Elements（各要素の役割）

| 要素 | 役割 | 推奨テキスト量 |
|------|------|--------------|
| アクセント線 | 視覚的な区切り・洗練された印象を与える | 装飾（固定幅96px） |
| メインメッセージ | お礼・締めくくりの主要メッセージ（56px相当） | 「ご清聴ありがとうございました」等 |
| サブメッセージ | 英語併記でグローバル対応（28px相当） | 英語訳1行 |
| 区切り線 | メッセージと発表者情報を分離 | 固定幅640px |
| 発表者情報 | 会社名・部署名・発表者名（28px相当） | 1〜2行、30文字以内 |
| 連絡先情報 | メールアドレス・URLなど（24px相当） | 1行、メール形式 |

## Usage Guide（AIへの使い方）

### プロンプト例

```
SLIDE.md と SLIDE-PATTERN-closing-slide.md を参照して、
以下の内容でクロージングスライドを作成してください。

【発表者情報】
サンプル株式会社　営業企画部
山田 太郎

【連絡先】
yamada@sample.co.jp

【メインメッセージ】
ご清聴ありがとうございました（デフォルトのまま）
```

### 注意点
- タイトルエリア・フッターは使用しない（スライド全面を使うレイアウト）
- メインメッセージは変更してもよいが、短く力強い文章を推奨
- 連絡先は実際のメールアドレスに置き換える
- 会社のブランドカラーを使う場合はSLIDE.mdのカラー定義に従う
- QRコードを追加したい場合は発表者情報エリアの右側に配置するとよい
