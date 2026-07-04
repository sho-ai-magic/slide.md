# SLIDE-PATTERN-quote-large-center

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview
**パターン名：** quote-large-center
**概要：** 大きな引用符と引用テキストを中央に配置する視覚的インパクトのあるコンテンツエリアレイアウト。名言・統計・重要メッセージを強調したいときに使用する。
**適したシーン：** 名言・格言の紹介、重要な統計数値の強調、キーメッセージの印象付け、セクション冒頭の導入スライド

## Structure（構造）

コンテンツエリア全体を縦方向・横方向ともに中央揃えにし、開き引用符・引用テキスト・区切り線・引用元・閉じ引用符を縦一列に並べる。開き引用符は左寄せ気味、閉じ引用符は右寄せ気味に配置して視線の流れを作り、引用テキストと引用元はともに中央揃えとする。

    structure:
      frame: standard
      content_area:
        direction: column
        align: center
        justify: center
        padding: "64px 160px（高さの約6%・幅の約8%）"
        gap: 32px
        elements:
          - type: quote_mark_open
            symbol: "“"
            font_size: 128px
            color: "#DDDDDD"
            align_self: flex-start
          - type: quote_text
            font_size: 36px
            color: "#333333"
            text_align: center
            line_height: 1.8
            font_style: italic
            lines: 2〜3行
          - type: divider
            width: 120px
            height: 2px
            color: "#CCCCCC"
          - type: attribution
            format: "— 出典・引用元"
            font_size: 26px
            color: "#888888"
            text_align: center
          - type: quote_mark_close
            symbol: "”"
            font_size: 128px
            color: "#DDDDDD"
            align_self: flex-end

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

| 要素 | 役割 | 推奨テキスト量 |
|------|------|--------------|
| 開き引用符「"」 | 視覚的アクセント・引用の開始を示す（128px相当） | 1文字（装飾） |
| 引用テキスト | メインメッセージ・名言・統計（36px相当） | 20〜60文字（2〜3行） |
| 区切り線 | 引用本文と出典を視覚的に分離 | 固定幅120px |
| 引用元テキスト | 出典・著者名・データソース（26px相当） | 「— ○○○」形式、15〜30文字 |
| 閉じ引用符「"」 | 視覚的アクセント・引用の終了を示す（128px相当） | 1文字（装飾） |

## Usage Guide（AIへの使い方）

### プロンプト例

```
SLIDE.md と SLIDE-PATTERN-quote-large-center.md を参照して、
以下の内容でスライドを作成してください。

【引用テキスト】
顧客が本当に求めているのは、製品ではなく
その製品がもたらす「体験」と「感情」である。

【引用元】
— サンプル白書 2024
```

### 注意点
- 引用テキストは短く力強い文章が効果的（60文字以内を推奨）
- 引用元は「— 著者名」または「— 出典（年）」の形式で統一する
- 統計を使う場合は「顧客の78%が〜と回答」のような断言形式が映える
- イタリック体は日本語フォントでは視覚的効果が薄いため、フォントウェイトで強調してもよい
- デザインはSLIDE.mdに従ってください。
