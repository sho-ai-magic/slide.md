# SLIDE-PATTERN-two-col-text-body

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview

**パターン名：** two-col-text-body
**概要：** 左右2カラムにそれぞれ「見出し + 本文テキスト」を配置する基本的な本文レイアウト。比較・対比・2つのトピックの並列説明に最適。
**適したシーン：** 2項目の比較・説明、課題と解決策、Before/After、2つの視点の対比

## Structure（構造）

コンテンツエリアを左右2カラムに分割する。各カラムはエリアラベル・見出し・区切り線・本文テキストの構造を持つ。カラム間に縦区切り線を配置する。

    structure:
      frame: standard
      layout: content-area-two-column
      padding: 48px 96px（1920px基準）
      gap: 64px（1920px基準）
      column-divider: vertical-line (1px, #EEEEEE)
      left-column:
        elements:
          - area-label (uppercase, muted)
          - heading (font-size: 32px相当, bold, #333)
          - divider-line (4px, 80px, #CCCCCC)
          - body-text (font-size: 26px相当, #555, line-height: 1.8)
      right-column:
        elements:
          - area-label (uppercase, muted)
          - heading (font-size: 32px相当, bold, #333)
          - divider-line (4px, 80px, #CCCCCC)
          - body-text (font-size: 26px相当, #555, line-height: 1.8)

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 写真は「写真添付エリア（後から実際の写真に差し替え）」、アイコンは「内容に沿ったSVGアイコンを生成して配置」として役割を記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

| 要素 | 配置 | 役割 |
|---|---|---|
| エリアラベル | 各カラム上部 | カラムのカテゴリ・ラベルを小さく表示（例：POINT 01、20px相当） |
| 見出し | ラベルの下 | カラム内容の主題を太字で表示（32px相当） |
| 区切り線 | 見出しの下 | 見出しと本文の視覚的分離（短い横線） |
| 本文テキスト | 区切り線の下 | 3〜4行の説明文・本文（26px相当） |
| カラム間縦線 | 中央 | 2カラムを視覚的に分離する細い縦線 |

## Usage Guide（AIへの使い方）

このパターンをAIに指示する際のプロンプト例：

> 「SLIDE-PATTERN-two-col-text-bodyのレイアウトで、左カラム見出し「[左見出し]」本文「[左本文]」、右カラム見出し「[右見出し]」本文「[右本文]」のスライドを作成してください。デザインはSLIDE.mdに従ってください。」

**注意点：**
- 左右の本文量をできるだけ揃えると見栄えが良い
- エリアラベルに「POINT 01 / POINT 02」「課題 / 解決策」などを入れると対比が分かりやすくなる
- 本文は3〜4行が目安。それ以上になる場合は箇条書きパターン（image-left-text-right等）を検討すること
