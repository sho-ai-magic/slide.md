# SLIDE-PATTERN-goal-kgi-kpi-dashboard

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview

**パターン名：** goal-kgi-kpi-dashboard
**概要：** 組織の目標ビジョンからKGI・KPIまでを4層構造で一覧表示するダッシュボードレイアウト（縦積み4層ダッシュボード）。
**適したシーン：** 年次目標発表、経営方針説明、事業計画スライド、OKR/KPI管理資料

## Structure（構造）

コンテンツエリアを上から下へ4層に分割する構造です。

```
┌──────────────────────────────────────────────┐
│ 第1層：目標（横幅フル・中央配置）             │
├────────────┬────────────┬────────────────────┤
│ KGI①       │ KGI②       │ KGI③               │ ← 第2層：KGI（3列）
├────────────┼────────────┼────────────────────┤
│ KPI①       │ KPI②       │ KPI③               │ ← 第3層：KPI+棒グラフ（3列）
├──────────────────────┬─────────────────────-─┤
│ 重点施策①            │ 重点施策②             │ ← 第4層：施策（2列）
└──────────────────────┴───────────────────────┘
```

    structure:
      frame: standard
      layout: vertical-4-layer-dashboard
      layer_goal:
        width: 100%
        align: center
      layer_kgi:
        columns: 3
        gap: 24px（高さの約2%）
      layer_kpi:
        columns: 3
        gap: 24px（高さの約2%）
        chart: mini-bar-chart
      layer_strategy:
        columns: 2
        gap: 24px（高さの約2%）

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

### コンテンツエリア全体
- `display: flex; flex-direction: column; padding: 16px 48px; gap: 16px; flex: 1;`

### 第1層：目標エリア
- 横幅フル、`background: #F0F0F0; border: 1px solid #CCCCCC; padding: 12px 32px; text-align: center;`
- エリアラベル（`.area-label`）: 「目標」
- 目標テキスト: `font-size: 26px; font-weight: bold; color: #333;`（組織の目標ビジョン文）

### 第2層：KGIエリア
- コンテナ: `display: flex; gap: 24px; padding: 8px 0;`
- 各KGIセル（3つ）: `flex: 1; border: 1px solid #E0E0E0; padding: 16px 24px; display: flex; flex-direction: column; gap: 8px;`
  - バッジ: `font-size: 20px; background: #E8E8E8; padding: 4px 12px; display: inline-block;` ← 「KGI①」「KGI②」「KGI③」（重要目標指標を識別するラベル）
  - 数値: `font-size: 44px; font-weight: bold; color: #333;`（KGIの達成目標数値、大きく強調）
  - 単位: `font-size: 22px; color: #555;`
  - 期限テキスト: `font-size: 20px; color: #999;`（達成期限）
  - 補足ラベル: `font-size: 22px; color: #555;`（何のKGIかを示す短い説明）

### 第3層：KPIエリア
- コンテナ: `display: flex; gap: 24px; padding: 8px 0;`
- 各KPIセル（3つ）: `flex: 1; border: 1px solid #E0E0E0; padding: 16px 24px;`
  - バッジ: 「KPI①」「KPI②」「KPI③」（KGIバッジと同スタイル）
  - ラベルテキスト: `font-size: 22px; font-weight: bold; color: #333;`（KPIの名称）
  - 簡易縦棒グラフ: `display: flex; align-items: flex-end; gap: 8px; height: 80px; padding-top: 8px;`
    - 各棒: `width: 24px; background: #CCCCCC;` + 適切な高さ（%で指定、実データの比率を反映）

### 第4層：施策エリア
- コンテナ: `display: flex; gap: 24px;`
- 各施策ボックス（2つ）: `flex: 1; background: #F5F5F5; border: 1px solid #CCCCCC; padding: 16px 24px;`
  - バッジ: 「重点施策①」「重点施策②」
  - テキスト: `font-size: 24px; font-weight: bold; color: #333;`（施策タイトル）

## Usage Guide（AIへの使い方）

このパターンをAIに指示する際のプロンプト例：

> 「SLIDE-PATTERN-goal-kgi-kpi-dashboardのレイアウトで、〔組織の目標ビジョン〕と、それに紐づく3つのKGI・各KGIに対応する3つのKPI・2つの重点施策を配置してください。デザインはSLIDE.mdに従ってください。」

### 適した場面
- 経営会議・全社方針発表での目標体系説明
- OKR/KPI設定会議のサマリースライド
- 四半期・年次レビュー資料のダッシュボードページ
- 事業計画書の冒頭ページ

### 入れるべき情報
| 層 | 記載内容 |
|----|--------|
| 第1層（目標） | 単一の目標ビジョン文またはスローガン（20〜40字程度） |
| 第2層（KGI） | 3つの重要目標指標（数値＋単位＋期限） |
| 第3層（KPI） | 各KGIに紐づく3つのKPI（進捗グラフ付き） |
| 第4層（施策） | 2つの重点施策タイトル |

### 注意点
- KGIの数値は大きく目立たせる（44px相当以上）
- 棒グラフの高さは実際のデータ比率を反映させること
- 各層のラベル（エリアラベル）は `area-label` クラスで統一する
- 施策テキストは1行に収まる短い表現を推奨
