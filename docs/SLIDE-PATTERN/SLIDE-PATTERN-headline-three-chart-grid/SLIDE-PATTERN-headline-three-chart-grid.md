# SLIDE-PATTERN-headline-three-chart-grid

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview

**パターン名：** headline-three-chart-grid
**概要：** コンテンツエリア上部の大きな見出し（キーメッセージ）と説明文、下部に3種類のグラフを並べてデータを多角的に解説するレイアウト（上下分割：見出し＋3列グラフ）。
**適したシーン：** 市場分析報告、データドリブンな洞察提示、業績サマリー、調査結果発表

## Structure（構造）

コンテンツエリアを上部見出しゾーンと下部グラフゾーンに分割する構造です。

```
┌──────────────────────────────────────────────┐
│ 大見出し（キーメッセージ・1〜2行）            │ ← 上部見出しゾーン
│ ─────────────────────────────────────── │
│ 説明テキスト（2行）                          │
├────────────┬────────────┬────────────────────┤
│ 横棒グラフ  │ 縦棒グラフ  │ 折れ線グラフ        │ ← 下部グラフゾーン（3列）
│ （パネル1） │ （パネル2） │ （パネル3）         │
└────────────┴────────────┴────────────────────┘
```

    structure:
      frame: standard
      layout: headline-top-chart-grid-bottom
      headline_zone:
        elements: [main-headline, divider, description]
      chart_grid:
        columns: 3
        gap: 32px（高さの約3%）
        panels: [horizontal-bar, vertical-bar, line-chart]

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

### コンテンツエリア全体
- `display: flex; flex-direction: column; padding: 24px 80px; gap: 24px;`

### 上部見出しエリア
- `flex-shrink: 0;`
- 大見出し（コンテンツ本体のキーメッセージ。スライドタイトルではない）: `font-size: 30px; font-weight: bold; color: #333; line-height: 1.5;` — 1〜2行
- 区切り線: `border: none; border-top: 1px solid #CCCCCC; margin: 12px 0;`
- 説明テキスト: `font-size: 20px; color: #555; line-height: 1.7;` — 2行

### 下部グラフエリア（3列）
- コンテナ: `flex: 1; display: flex; gap: 32px; padding-top: 8px;`
- 各グラフパネル（3つ）: `flex: 1; border: 1px solid #E0E0E0; padding: 24px;`
  - パネル見出し: `font-size: 20px; font-weight: bold; color: #333;`
  - サブテキスト: `font-size: 18px; color: #888; margin-bottom: 12px;`

#### パネル1：横棒グラフ
- 5〜7本の横棒: `display: flex; align-items: center; gap: 12px; margin-bottom: 8px; font-size: 18px; color: #555;`
- 棒部分: `height: 16px; background: #CCCCCC;` + 幅でデータ比率を表現

#### パネル2：縦棒グラフ
- グラフコンテナ: `display: flex; align-items: flex-end; gap: 6px; height: 160px;`
- 各棒: `width: 28px; background: #CCCCCC;` + 高さでデータ比率を表現

#### パネル3：折れ線グラフ（SVG）
```html
<svg width="100%" height="160" viewBox="0 0 160 80" preserveAspectRatio="none">
  <polyline points="10,60 40,45 70,55 100,25 130,20 150,30"
    fill="none" stroke="#CCCCCC" stroke-width="2"/>
  <line x1="0" y1="70" x2="160" y2="70" stroke="#EEEEEE" stroke-width="1"/>
</svg>
```
（viewBox内座標・stroke-widthはスケール追従のため変換しない。SVGのwidth/heightのみ1920px基準に変換する）

### 凡例・出典テキスト
- 凡例: `font-size: 18px; color: #888; margin-top: 12px;`
- 出典: `font-size: 18px; color: #AAAAAA; text-align: right; margin-top: 8px;`

## Usage Guide（AIへの使い方）

このパターンをAIに指示する際のプロンプト例：

> 「SLIDE-PATTERN-headline-three-chart-gridのレイアウトで、〔伝えたいキーメッセージ〕を大見出しに、〔根拠・補足〕を説明文に配置し、下部に〔比較データ・時系列データ・トレンド〕の3つのグラフを配置してください。デザインはSLIDE.mdに従ってください。」

### 適した場面
- 複数の指標を一画面で比較したいデータ分析スライド
- 市場動向・競合比較・業績推移を同時に見せたい場合
- プレゼンのデータパートの冒頭サマリーページ

### 入れるべき情報
| エリア | 記載内容 |
|--------|--------|
| 大見出し | 最も伝えたいキーメッセージ（30〜50字程度） |
| 説明テキスト | 見出しの根拠・補足説明（2行以内） |
| パネル1 | カテゴリ別の比較データ（横棒グラフ） |
| パネル2 | 期間別の量的変化（縦棒グラフ） |
| パネル3 | 時系列トレンド（折れ線グラフ） |

### 注意点
- 大見出しは1〜2行に収める（3行以上になるとグラフ領域が狭くなる）
- 各パネルのサブテキストにデータ期間・単位を明記すること
- 3つのグラフは互いに補完し合う関係にする（同一データを3種類で見せない）
- 出典テキストは全パネル共通で1行まとめて表示してもよい
