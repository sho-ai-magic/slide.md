# SLIDE-PATTERN-summary-three-points

このファイルはスライドのコンテンツエリア（タイトル行より下の領域）のレイアウトパターン定義書です。SLIDE.mdと組み合わせてAIツールに渡すことで、このパターンのスライドを生成できます。タイトルエリア・フッター（ブランドフッター・ページ番号）・装飾はSLIDE.mdの `Slide Frame` セクションで定義されるため、このファイルには含みません。文言はすべてサンプル（ダミーテキスト）です。

## Overview
**パターン名：** summary-three-points
**概要：** 「まとめ」スライド用のシンプルな3点ポイントリスト。チェックマーク付きで視認性が高く、プレゼンのクロージングに最適。
**適したシーン：** プレゼンのまとめ・要点の整理・議論の結論提示・次のアクションへの導入

## Structure（構造）

```yaml
structure:
  frame: standard
  layout: summary-three-points
  content_area:
    display: flex
    flex-direction: column
    justify-content: center
    padding: "48px 160px"
    gap: 32px

  summary_label:
    text: "SUMMARY"
    style: "area-label（font-size: 22px、color: #999、letter-spacing: 0.08em）"
    margin_bottom: 8px

  point_items:
    count: 3
    each:
      display: flex
      align-items: flex-start
      gap: 40px
      padding: "28px 0"
      border-bottom: "1px solid #F0F0F0"
      check_icon:
        width: 56px
        height: 56px
        border: "2px solid #CCCCCC"
        display: flex
        align-items: center
        justify-content: center
        font_size: 28px
        color: "#888"
        flex_shrink: 0
        content: "✓"
      text_block:
        heading:
          font_size: 30px
          font_weight: bold
          color: "#333"
        body:
          font_size: 24px
          color: "#666"
          margin_top: 8px
```

## Elements（各要素の役割）

※ タイトル（H1）・フッターはSLIDE.mdの Slide Frame で定義されるため、この表には含めない。コンテンツエリアの要素のみを記述する。
※ 写真は「写真添付エリア（後から実際の写真に差し替え）」、アイコンは「内容に沿ったSVGアイコンを生成して配置」として役割を記述する。
※ 文字要素には、元スライドの見た目から見積もった1920px基準のフォントサイズの目安を役割欄に添える。

| 要素 | 役割 | 推奨文字数 |
|------|------|-----------|
| SUMMARYラベル | まとめスライドであることを明示するセクション識別子（22px相当） | 固定（"SUMMARY"） |
| チェックボックスアイコン（✓） | 各ポイントが達成・確認済みの事項であることを視覚的に示す（28px相当） | 固定（"✓"） |
| ポイント見出し | まとめの要点・結論を簡潔に記述（30px相当） | 20〜35文字 |
| ポイント補足テキスト | 見出しを補完する詳細説明や根拠（24px相当） | 30〜60文字 |

## Usage Guide（AIへの使い方）

### プロンプト例

```
SLIDE.mdのデザインシステムと、以下のSLIDE-PATTERN-summary-three-pointsパターンを使って
スライドを1枚生成してください。

【ポイント 1】
- 見出し: 顧客体験の向上が売上増加の最大要因
- 補足: 満足度調査の改善により既存顧客の継続率が前年比で向上し、売上にも直結した

【ポイント 2】
- 見出し: デジタル化により業務効率が大幅に改善
- 補足: 手作業の多くをシステムに移行し、1人あたりの処理件数が大きく増加した

【ポイント 3】
- 見出し: 次四半期は新市場開拓を最優先課題とする
- 補足: 新規エリアでの展開準備を進め、来期中のローンチを目指す
```

### 注意点
- 3つのポイントは重要度順または論理的な順序で並べること
- 見出しは体言止めか短文で統一し、補足テキストと文体を揃える
- チェックマークは完了・確認済みの印象を与えるため、今後の展望より達成済みの事実に使うと効果的
- 4点以上ある場合はポイントを絞るか、別パターン（numbered-list-with-body等）を検討する
- デザインはSLIDE.mdに従ってください。
