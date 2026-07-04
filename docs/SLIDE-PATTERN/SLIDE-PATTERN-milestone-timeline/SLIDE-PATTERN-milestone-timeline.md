# SLIDE-PATTERN-milestone-timeline

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview
**パターン名：** milestone-timeline
**概要：** 横軸に時間軸を引き、マイルストーンをマーカーで表示するタイムラインレイアウト。完了済みと予定のマーカーを視覚的に区別する。
**適したシーン：** プロジェクト計画・ロードマップの説明、進捗報告、スケジュール概要の共有

## Structure（構造）

コンテンツエリアを縦方向中央揃えで、上からフェーズラベル行・タイムラインコンテナ・凡例の順に配置する。フェーズラベル行は期間を等幅に区切って表示し、タイムラインコンテナは中央に水平線を通してその上下にマイルストーンマーカーと付随情報を均等配置する。最下部に完了・予定を示す凡例を右寄せで配置する。

    structure:
      frame: standard
      layout: milestone-timeline
      content_area:
        direction: column
        padding: "48px 96px（高さの約4%）"
        justify: center
        elements:
          - type: phase_label_row
            phases:
              - label: "Q1（1〜3月）"
              - label: "Q2（4〜6月）"
              - label: "Q3（7〜9月）"
              - label: "Q4（10〜12月）"
            item_style:
              flex: 1
              border_right: "1px solid #E0E0E0"
              padding: "8px 16px"
              font_size: 18px相当
              color: "#999999"
          - type: timeline_container
            position: relative
            elements:
              - type: timeline_line
                height: 4px
                background: "#CCCCCC"
              - type: milestone_markers
                items:
                  - date: "1/10"
                    label: "キックオフ"
                    description: "プロジェクト開始"
                    status: completed
                  - date: "3/31"
                    label: "要件確定"
                    description: "要件定義完了"
                    status: completed
                  - date: "6/30"
                    label: "設計完了"
                    description: "基本設計レビュー"
                    status: completed
                  - date: "9/15"
                    label: "開発完了"
                    description: "内部テスト開始"
                    status: planned
                  - date: "11/1"
                    label: "UAT完了"
                    description: "ユーザー受入テスト"
                    status: planned
                  - date: "12/15"
                    label: "本番リリース"
                    description: "サービス開始"
                    status: planned
                marker_style:
                  width: 28px
                  height: 28px
                  border: "4px solid #888"
                  border_radius: 50%
                completed_bg: "#CCCCCC"
                planned_bg: "#FFFFFF"
          - type: legend
            items:
              - symbol: "●"
                color: "#888888"
                label: "完了"
              - symbol: "○"
                color: "#888888"
                label: "予定"
            font_size: 18px相当
            color: "#888888"

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 写真は「写真添付エリア（後から実際の写真に差し替え）」、アイコンは「内容に沿ったSVGアイコンを生成して配置」として役割を記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

| 要素 | 役割 | 推奨テキスト量 |
|------|------|--------------|
| フェーズラベル行 | 期間の区分（Q1〜Q4等）を示す（18px相当） | 4〜5区間、各10文字以内 |
| タイムライン横線 | 時間の流れを示す基準線 | 装飾（固定高さ4px） |
| マイルストーンマーカー | 各マイルストーンの位置を示す丸印 | 完了=塗りつぶし、予定=空洞 |
| 日付テキスト（上） | マーカーの日付（18px相当） | 「M/D」形式、5文字以内 |
| マイルストーン名（下） | マイルストーンの名称（20px相当） | 5〜10文字 |
| 説明テキスト（下） | マイルストーンの内容補足（18px相当） | 8〜15文字 |
| 凡例 | 完了・予定の区別を説明（18px相当） | 「● 完了 ○ 予定」 |

## Usage Guide（AIへの使い方）

### プロンプト例

```
SLIDE.md と SLIDE-PATTERN-milestone-timeline.md を参照して、
以下のマイルストーンを含むタイムラインスライドを作成してください。

【フェーズ区分】
Phase1（4〜6月） / Phase2（7〜9月） / Phase3（10〜12月） / Phase4（翌1〜3月）

【マイルストーン】
- 4/1  : プロジェクト発足（完了）
- 5/31 : 要件定義完了（完了）
- 7/15 : プロトタイプ完成（予定）
- 9/30 : 開発フリーズ（予定）
- 11/15: テスト完了（予定）
- 3/1  : 正式リリース（予定）

デザインはSLIDE.mdに従ってください。
```

### 注意点
- マイルストーンは5〜7個が視認性の観点で適切
- 「完了」マーカーは塗りつぶし、「予定」マーカーは白抜きで区別する
- フェーズラベルは等幅に分割されるため、時間軸の比率に注意
- 現在日付を赤線などで示すとより進捗が伝わりやすい（SLIDE.mdのアクセントカラーを活用）
- 年をまたぐ場合は年号を「'25 Q1」のように省略表記するとよい
