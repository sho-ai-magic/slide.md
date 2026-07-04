# SLIDE-PATTERN-risk-matrix-2x2

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview
**パターン名：** risk-matrix-2x2
**概要：** 影響度（縦軸）×発生確率（横軸）の2×2マトリクスでリスクを4象限に分類・整理するコンテンツエリアレイアウト。各セルにリスク項目をバッジ形式で表示する。
**適したシーン：** リスク分析・評価、プロジェクトリスク管理、課題の優先度整理、経営課題の可視化

## Structure（構造）

コンテンツエリアを左右に分割し、左側（幅約75%）にマトリクス本体、右側（幅約25%）に凡例エリアを配置する。マトリクス本体はさらに縦軸ラベル（左）と2×2グリッド（右）に分かれ、グリッド下部に横軸ラベルを置く。各セルは背景の濃淡で優先度を視覚的に区別し、内部にセル見出し・説明・リスク項目バッジを縦に並べる。

    structure:
      frame: standard
      content_area:
        direction: row
        padding: "32px 96px（高さの約3%・幅の約5%）"
        gap: 48px
        children:
          - id: matrix_body
            width: "75%"
            elements:
              - type: y_axis_label
                text: "影響度"
                writing_mode: vertical-rl
                font_size: 24px
                color: "#666666"
              - type: grid_2x2
                cells:
                  - position: top_left
                    label: "重点管理"
                    description: "高影響・低確率"
                    background: "#F5F5F5"
                    items: ["コンプライアンス違反", "システム障害"]
                  - position: top_right
                    label: "最優先対応"
                    description: "高影響・高確率"
                    background: "#EEEEEE"
                    items: ["人材不足", "コスト超過"]
                  - position: bottom_left
                    label: "経過観察"
                    description: "低影響・低確率"
                    background: "#FFFFFF"
                    items: ["軽微な仕様変更"]
                  - position: bottom_right
                    label: "対応計画"
                    description: "低影響・高確率"
                    background: "#F0F0F0"
                    items: ["スケジュール遅延", "認識齟齬"]
              - type: x_axis_label
                text: "発生確率"
                font_size: 24px
                color: "#666666"
                align: center
          - id: legend_area
            width: "25%"
            elements:
              - type: legend_item
                label: "最優先対応"
                background: "#EEEEEE"
              - type: legend_item
                label: "重点管理"
                background: "#F5F5F5"
              - type: legend_item
                label: "対応計画"
                background: "#F0F0F0"
              - type: legend_item
                label: "経過観察"
                background: "#FFFFFF"

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

| 要素 | 役割 | 推奨テキスト量 |
|------|------|--------------|
| 縦軸ラベル（影響度） | Y軸の意味を示す（24px相当） | 「影響度」など3〜5文字 |
| 横軸ラベル（発生確率） | X軸の意味を示す（24px相当） | 「発生確率」など4〜6文字 |
| 象限ラベル（重点管理等） | 各セルの分類名（24px相当） | 4〜8文字 |
| リスク項目バッジ | 具体的なリスク内容（22px相当） | 各セル1〜3個、10文字以内 |
| 凡例エリア | セルの意味と優先度を補足説明（凡例テキスト24px相当・補足20px相当） | 各4項目 |

## Usage Guide（AIへの使い方）

### プロンプト例

```
SLIDE.md と SLIDE-PATTERN-risk-matrix-2x2.md を参照して、
以下のリスクをマトリクスに整理したスライドを作成してください。

【最優先対応（高影響・高確率）】
- 主要メンバーの離職
- 予算超過

【重点管理（高影響・低確率）】
- セキュリティインシデント
- 法規制の変更

【対応計画（低影響・高確率）】
- 軽微な仕様変更
- ベンダー対応遅延

【経過観察（低影響・低確率）】
- 競合サービスの機能追加
```

### 注意点
- 各セルのリスク項目は1〜3個が適切（多すぎると読みにくくなる）
- 象限ラベルは変更してもよい（「即時対応」「監視継続」など）
- 縦軸・横軸の意味はプロジェクトに合わせて変更可能（例：「コスト影響」×「技術的難易度」）
- バッジのグレー濃度で優先度を視覚的に区別している
- デザインはSLIDE.mdに従ってください。
