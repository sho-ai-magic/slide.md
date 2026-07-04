# SLIDE-PATTERN-cycle-diagram-with-labels

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview

**パターン名：** cycle-diagram-with-labels
**概要：** コンテンツエリア中央に回転サイクル図形（円形の矢印フロー）を配置し、四隅に各フェーズの詳細テキストを配置するレイアウト
**適したシーン：** PDCAサイクル、業務改善サイクル、循環プロセスの説明

## Structure（構造）

コンテンツエリア中央に大きなサイクル図形（円形または円弧型の矢印を持つ4分割構成）を配置する。図形の四隅（左上・右上・左下・右下）にそれぞれのフェーズ名と説明文ボックスを配置する。図形の中心にはコアコンセプト名を置く。

    structure:
      frame: standard
      layout: center-diagram-with-corner-labels
      center:
        type: circular-cycle-diagram
        segments: 4
        center-label: [core-concept-text]
      corners:
        top-left: [phase-name, description]
        top-right: [phase-name, description]
        bottom-left: [phase-name, description]
        bottom-right: [phase-name, description]

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 写真は「写真添付エリア（後から実際の写真に差し替え）」、アイコンは「内容に沿ったSVGアイコンを生成して配置」として役割を記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

| 要素 | 配置 | 役割 |
|---|---|---|
| サイクル図形 | コンテンツエリア中央 | 4フェーズの循環・繰り返しを視覚化 |
| 中心ラベル | 図形の中心 | プロセス全体のコアコンセプト名（本文28px相当・補足22px相当） |
| フェーズ名（H3相当） | 四隅各エリア上部 | そのフェーズ（Plan/Do/Check/Action等）の名称（28px相当） |
| フェーズ説明 | フェーズ名の下 | そのフェーズの概要説明（2〜4行、22px相当） |

## Usage Guide（AIへの使い方）

このパターンをAIに指示する際のプロンプト例：

> 「SLIDE-PATTERN-cycle-diagram-with-labelsのレイアウトで、PDCAサイクルを表現し、四隅にPlan/Do/Check/Actionの説明を記述してください。中心に「継続改善」と表示してください。デザインはSLIDE.mdに従ってください。」

**注意点：**
- 4フェーズ固定のパターン。3フェーズや5フェーズのサイクルには向かない
- 四隅の説明文は各100字程度（3〜4行）に収めるとバランスが良い
- サイクル図形のセグメントカラーはフェーズごとに濃淡を変えると見やすい
