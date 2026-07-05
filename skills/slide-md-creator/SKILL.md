---
name: slide-md-creator
description: 既存のスライド・画像・WebサイトからデザインシステムSLIDE.mdと6ページのHTMLサンプルスライドを生成する。「このスライドのデザインシステムを作って」「SLIDE.mdを生成して」「SLIDE.mdを作りたい」「このサイトのデザインでSLIDE.mdを生成して」「slide-md-creator」と言われたときに使用する。
---

# slide-md-creator

既存のスライド・画像・Webサイト・テキストからデザイン情報を解析し、AIツール（Claude Design、NotebookLM、Google Slides等）向けのスライドデザインシステム定義ファイル（SLIDE.md）と6ページのHTMLサンプルスライドを生成する。

## STEP 1：入力の受け取り

スキル起動時、ユーザーが以下のいずれかを提供しているか確認する：

- 画像ファイル（スライドのスクリーンショット、ロゴ、ブランドガイド等）
- WebサイトのURL
- 既存スライドのファイル（PowerPoint / Google Slides）
- テキストによるデザインの説明

**何も提供されていない場合**、以下のように尋ねる：

> 「参考にしたいスライド・画像・WebサイトのURLはありますか？テキストでデザインのイメージを説明していただくことも可能です。」

複数の入力が提供された場合はすべてを受け取り、STEP 2の分析に使用する。

## STEP 2：デザイン要素の分析

受け取った入力を解析し、以下の要素を読み取る。読み取れた内容をユーザーに箇条書きで提示する。

### 複数ソース・複数ページの並列処理

**ソース（URL・画像・ファイル）が2つ以上ある、またはWebサイトから複数ページを取得する必要がある場合は、Agentツールを使って並列で分析する。**

#### ケース1：ユーザーが複数のソースを提供した場合

各ソースに対して独立したサブエージェントを同時に起動する：

    # 例：URL 2つ + 画像 1枚が提供された場合
    Agent(subagent_type="general-purpose", prompt="以下のURLを取得し、Colors・Typography・Layout・Component Styleの情報をMarkdownで返せ: https://example.com/")
    Agent(subagent_type="general-purpose", prompt="以下のURLを取得し、Colors・Typography・Layout・Component Styleの情報をMarkdownで返せ: https://example.com/about")
    Agent(subagent_type="general-purpose", prompt="添付画像 logo.png からColors・Typography・Component Styleの情報をMarkdownで返せ")

- 全サブエージェントの結果が揃ってから情報を統合して1つの分析結果を作成する
- 矛盾する値がある場合はより多くのソースで確認できた値を優先し、確認が取れない項目はSTEP 3でユーザーに確認する

#### ケース2：単一URLだがブランドを理解するために複数ページを読む必要がある場合

トップページだけでは情報が不足している場合（例：カラーが読み取れない、CSSが取得できない）、関連ページを並列取得する：

    # 例：トップ・プロダクト・Aboutページを同時取得
    Agent(subagent_type="general-purpose", prompt="https://example.com/ を取得してデザイン情報（Colors・Typography・Layout）をMarkdownで返せ")
    Agent(subagent_type="general-purpose", prompt="https://example.com/product を取得してデザイン情報（Colors・Typography・Layout）をMarkdownで返せ")
    Agent(subagent_type="general-purpose", prompt="https://example.com/about を取得してデザイン情報（Colors・Typography・Layout）をMarkdownで返せ")

- 並列取得するページ数は最大3〜4ページを目安にする
- すべての結果を受け取ってから統合分析に進む

#### ケース3：ソースが1つだけの場合

並列化は不要。通常のWebFetch・Readで分析して進める。

---

### 分析する要素

**Colors（カラー）**
- Primary（メインカラー）：最も多く使われている色。HEXコードで記録する。
- Secondary（サブカラー）：2番目に使われている色。HEXコードで記録する。
- Background（背景色）：スライドの背景色。HEXコードで記録する。
- Accent（強調色）：ボタン・下線・強調に使われている色。HEXコードで記録する。
- Text（テキスト色）：本文テキストの色（最も濃い色）。HEXコードで記録する。
- Text Sub（見出し・ラベル色）：本文より少し薄い色（例：#444746）。読み取れない場合はTextより15〜20%明るい値を推定する。
- Text Muted（補足・キャプション色）：さらに薄い色（例：#5E5E5E）。読み取れない場合はTextより30〜40%明るい値を推定する。
- Gradient（グラデーション）：グラデーションの有無と使用カラーストップ・方向。例：「Primary → Secondary → Accent・90deg（横）」または「なし」。グラデーションが使われている場合は方向（横・縦・斜め）も必ず記録する。

**Typography（タイポグラフィ）**
- 見出しフォント（Display）：フォント名・サイズ（px）・ウェイト（bold / regular等）
- 本文フォント（Body）：フォント名・サイズ（px）・ウェイト
- キャプションフォント：フォント名・サイズ（px）・ウェイト
- モノスペースフォント（Mono）：数値・コード・ページ番号などに使われているフォント名。読み取れない場合は「なし」と記録する。
- フォント名が不明な場合は「不明」と記録し、STEP 3で確認する

**Layout（レイアウト）**
- スライドサイズ：16:9 / 4:3 / その他
- 余白：上下左右のおおよその余白サイズ（px）
- 整列の基準：左寄せ / 中央寄せ / 右寄せ

**Slide Frame（枠要素）**
- タイトルエリア：上部左寄せ / 上部中央揃え、装飾の有無（アクセントバー・下線 等）
- ページ番号：右下 / 左下 / なし、表示フォーマット（01 / 1 / 1/5 等）
- 装飾：アクセントバー・装飾図形・ロゴ等の有無と配置

**Background Accent（背景装飾）**
- 半透明の装飾（グラデーション丸・装飾図形等）の有無
- ある場合：配置（右上・左下 等）・使用カラー（Primaryカラーの薄め / Secondaryカラーの薄め 等）・おおよそのサイズ感
- ない場合：「なし」と記録する

**Component Style（コンポーネントスタイル）**
- カードの角丸：スライドのコンテンツカードに使われている角丸サイズ（例：12px / 22px / なし）
- カードの影：影の有無と強さ（例：あり（薄め）/ あり（標準）/ なし）
- 縦バー：見出し左側に縦バーが使われているか（あり / なし）。ある場合は色（単色 / グラデーション）と幅
- 箇条書きマーカー：ドットの色（単色 / Primary・Secondary・Accentの複数色 / グラデーション）と形状（丸 / 四角 / 線等）
- 番号スタイル：番号付きリストのスタイル（グラデーション円に白数字 / 単色円 / テキストのみ 等）

**Tone（雰囲気）**
- フォーマル / カジュアル / ミニマル / ボールド / 等
- どんなシーンに合うか（ビジネス発表・社内共有・カンファレンス 等）

### 分析結果の提示例

> 「以下のデザイン要素を読み取りました：
>
> **カラー**
> - Primary：#4285F4（ブルー）
> - Secondary：#9168C0（パープル）
> - Background：#F8FAFD（ほぼ白）
> - Accent：#D96570（ピンク）
> - Text：#1F1F1F（ほぼ黒）
> - Text Sub：#444746（濃いグレー）
> - Text Muted：#5E5E5E（グレー）
> - Gradient：Primary → Secondary → Accent・90deg（横）および180deg（縦）
>
> **タイポグラフィ**
> - 見出し（Display）：Manrope / 54〜96px / 800
> - 本文（Body）：Manrope / 18〜22px / 400〜500
> - キャプション：Manrope / 12px / 500
> - モノスペース（Mono）：Roboto Mono / ページ番号・数値・コードに使用
>
> **レイアウト**
> - スライドサイズ：16:9（1920 × 1080px）
> - 余白：上下90〜96px・左右140px
> - 整列：左寄せ
>
> **背景装飾**：右上と左下に半透明グラデーション丸あり（Secondary / Primary カラー・透明度8〜16%）
>
> **コンポーネントスタイル**
> - カード：border-radius 22px・薄い影あり・border 1px solid rgba(0,0,0,0.04)
> - 縦バー：グラデーション縦バー（幅8px、Primary→Secondary→Accent）あり
> - 箇条書きマーカー：Primary / Secondary / Accentの3色丸ドット
> - 番号スタイル：グラデーション円に白数字
>
> **雰囲気**：ボールド・モダン、テクノロジー系カンファレンス・プロダクト紹介向け」

## STEP 3：確認の対話

STEP 2の分析結果を提示した後、不明・不確かな点についてユーザーに確認する。

### 確認のルール

- 質問は**合計3〜5問以内**に抑える
- 1つのメッセージにつき**1問だけ**聞く
- 読み取れた情報については聞かない（不明な情報だけを確認する）
- ユーザーが「わからない」と答えた場合は、一般的な値（Webセーフフォント、標準余白等）を使用して進める

### 確認すべき優先順位

1. フォント名が「不明」の場合 → 「見出しや本文に使われているフォントをご存知ですか？（例：Noto Sans JP、Hiragino Kaku Gothic等）」
2. カラーが読み取れなかった場合 → 「〇〇の色をご存知でしたら教えてください」
3. スライドサイズが不明の場合 → 「スライドのサイズは16:9（横長）と4:3（やや正方形）のどちらですか？」
4. 雰囲気の確認 → 「このデザインはどんな場面での使用を想定していますか？（例：社内会議・顧客向けプレゼン・カンファレンス等）」

### 確認完了の判断

以下のいずれかになったら確認終了とする：
- 5問の確認が終わった
- 未確認の情報がなくなった
- ユーザーが「進めてください」等の意思を示した

確認完了後、「では内容をもとにSLIDE.mdとsample.htmlを生成します」と伝えてSTEP 4に進む。

## STEP 4：SLIDE.mdの生成

STEP 2〜3で確定したデザイン情報を使い、以下のテンプレートに値を埋めて `SLIDE.md` を生成する。

### 出力フォルダの決定

ファイルはカレントディレクトリ内の `SLIDE-md/` フォルダの中に `SLIDE-md-{source}` というフォルダを作成して保存する。`{source}` はソースの種類に応じて以下のルールで決める：

- **URLの場合：** ドメイン名から主要な単語を抽出する（例：`https://www.apple.com` → `apple`、`https://www.google.co.jp` → `google`）
- **ファイルの場合：** ファイル名（拡張子なし）を使う（例：`company-deck.pptx` → `company-deck`）
- **テキスト説明の場合：** 説明文からブランド名や代表的なキーワードを1つ抽出する

フォルダ名はすべて小文字・半角英数字・ハイフンのみを使用する。作成するフォルダの例：`SLIDE-md/SLIDE-md-apple`、`SLIDE-md/SLIDE-md-google`、`SLIDE-md/SLIDE-md-toyota`

### SLIDE.mdの出力形式

以下の形式で出力すること。各項目の `[　]` の中に分析・確認で得た実際の値を入れる。Markdownの各セクションの後にYAML形式でも同じ値を記載し、AIツールが値を正確に読み取れるようにする。

---
出力ファイル名：SLIDE-md/SLIDE-md-{source}/SLIDE.md

# SLIDE.md

このファイルはスライドデザインシステムの定義書です。AIツール（Claude Design、NotebookLM、Google Slides等）にこのファイルを渡すことで、一貫したデザインのスライドを生成できます。

## Overview

**参照ソース：** [参考にしたスライド・サイト・ブランドの名称や説明]
**マッチするシーン：** [このデザインが合う場面・用途・雰囲気]

## Colors

| 役割 | カラー名 | HEXコード |
|---|---|---|
| Primary | [色名] | [#XXXXXX] |
| Secondary | [色名] | [#XXXXXX] |
| Background | [色名] | [#XXXXXX] |
| Accent | [色名] | [#XXXXXX] |
| Text | [色名] | [#XXXXXX] |
| Text Sub | [色名] | [#XXXXXX] |
| Text Muted | [色名] | [#XXXXXX] |

**グラデーション：** [「Primary → Secondary → Accent・90deg（横）」など / なし]

    colors:
      primary: "[#XXXXXX]"
      secondary: "[#XXXXXX]"
      background: "[#XXXXXX]"
      accent: "[#XXXXXX]"
      text: "[#XXXXXX]"
      text_sub: "[#XXXXXX]"
      text_muted: "[#XXXXXX]"
      gradient_main: "[linear-gradient(90deg, Primary, Secondary, Accent) / なし]"
      gradient_vert: "[linear-gradient(180deg, Primary, Secondary, Accent) / なし]"

## Typography

| 役割 | フォント | サイズ | ウェイト |
|---|---|---|---|
| 見出し（H1） | [フォント名] | [Xpx] | [Bold/Regular] |
| 見出し（H2） | [フォント名] | [Xpx] | [Bold/Regular] |
| 本文 | [フォント名] | [Xpx] | [Regular] |
| キャプション | [フォント名] | [Xpx] | [Regular] |
| モノスペース | [フォント名 / なし] | [-] | [-] |

    typography:
      heading_h1:
        font: "[フォント名]"
        size: "[Xpx]"
        weight: "[700/400]"
      heading_h2:
        font: "[フォント名]"
        size: "[Xpx]"
        weight: "[700/400]"
      body:
        font: "[フォント名]"
        size: "[Xpx]"
        weight: "[400]"
      caption:
        font: "[フォント名]"
        size: "[Xpx]"
        weight: "[400]"
      mono:
        font: "[フォント名 / なし]"
        usage: "[数値・コード・ページ番号 等 / なし]"

## Layout

- **スライドサイズ：** [16:9 / 4:3]（幅[X]px × 高さ[X]px）
- **余白（上下）：** [Xpx]
- **余白（左右）：** [Xpx]
- **テキスト整列：** [左寄せ / 中央寄せ]

    layout:
      slide_size: "[16:9 / 4:3]"
      width: "[X]px"
      height: "[X]px"
      padding_vertical: "[X]px"
      padding_horizontal: "[X]px"
      text_align: "[left / center]"

## Slide Frame

スライドの全ページに共通して適用される「枠」の要素を定義します。SLIDE-PATTERN-{name}.mdはコンテンツエリアの構造のみを定義するため、タイトルエリア・ページ番号・装飾はすべてここで一括管理します。これにより、どのパターンを使っても枠の見た目が統一されます。

**タイトルエリア：** [配置と装飾の説明（例：上部左寄せ、直下にアクセントカラーのバーを配置）]
**ページ番号：** [配置とフォーマット（例：右下・「01 / 6」形式 / なし）]
**ページ番号スタイル：** [グラデーションテキスト / 単色テキスト（TextカラーやTextMutedカラー）、モノスペースフォント使用の有無]
**ブランドフッター：** [配置と内容（例：左下・デザインシステム名をText Mutedカラーで表示 / なし）]
**背景アクセント：** [半透明グラデーション丸などの装飾の有無・配置・使用カラー（例：右上にSecondaryカラー12%透明・左下にPrimaryカラー8%透明の丸 / なし）]
**縦バー：** [見出し左側の縦バーの有無・スタイル（例：グラデーション縦バー・幅8px / なし）]

    slide_frame:
      title_area:
        position: "[top-left / top-center]"
        text_align: "[left / center]"
        decoration: "[accent-bar / underline / none]"
      page_number:
        position: "[bottom-right / bottom-left / none]"
        format: "[01 / 1 / 1/5 等]"
        style: "[gradient-text / solid]"
        font: "[mono / body]"
      brand_footer:
        position: "[bottom-left / bottom-right / none]"
        content: "[デザインシステム名 / ブランド名 / なし]"
      background_accent:
        type: "[radial-gradient-circle / none]"
        placement: "[right-top, left-bottom / none 等]"
        color: "[Secondary薄め / Primary薄め 等]"
      section_bar:
        style: "[gradient-vertical / solid / none]"
        width: "[Xpx / -]"

## Component Style

スライド内で使用するコンポーネントの基本スタイルを定義します。

**カード：** [角丸・影・枠線の標準スタイル（例：border-radius: 22px、薄い影あり、border: 1px solid rgba(0,0,0,0.04)）]
**箇条書きマーカー：** [ドットの色・形状（例：Primary / Secondary / Accentの3色丸ドット / Primaryカラーの単色丸ドット）]
**番号スタイル：** [番号付きリストのスタイル（例：グラデーション円に白数字 / 単色円 / テキストのみ）]

    component_style:
      card:
        border_radius: "[Xpx]"
        shadow: "[0 1px 3px rgba(60,64,67,0.08),0 4px 16px rgba(60,64,67,0.05) / none]"
        border: "[1px solid rgba(0,0,0,0.04) / none]"
      bullet:
        color: "[Primary / Primary-Secondary-Accent / gradient]"
        shape: "[circle / square / line]"
      number:
        style: "[gradient-circle / solid-circle / text-only]"
        color: "[gradient-main / Primary / none]"

## Do / Don't

**Do（やること）**
- [このデザインで推奨されること]
- [このデザインで推奨されること]

**Don't（やってはいけないこと）**
- [このデザインで避けるべきこと]
- [このデザインで避けるべきこと]
---

### Do / Don't の生成方針

- **Do** はデザインの特徴から自然に導かれるルールを記述する（例：ミニマルなデザインなら「1スライド1メッセージを徹底する」）
- **Don't** はデザインと相反する要素を記述する（例：モノクロデザインなら「原色や派手なグラデーションを使わない」）
- 各2〜4項目を目安にする

## STEP 5：sample.htmlの生成

SLIDE.mdで定義したデザイントークン（色・フォント・余白）を適用した6ページのHTMLスライドを生成し、STEP 4で作成した `SLIDE-md/SLIDE-md-{source}/` フォルダ内に `sample.html` として保存する。

### ページ構成（固定）

| ページ番号 | スライドの種類 | 主な内容 |
|---|---|---|
| 1 | デザインシステム概要 | カラーパレット・パーツ・タイポグラフィ・レイアウト・Do/Don'tを1画面に集約した仕様書ページ |
| 2 | 表紙（タイトルスライド） | タイトル・サブタイトル・会社名や日付のプレースホルダー |
| 3 | セクションタイトル | セクション番号・セクション名・説明文 |
| 4 | 箇条書き（本文スライド） | 見出し・番号付きカード形式の4〜5項目リスト |
| 5 | データチャート | 見出し・棒グラフ（左）＋円グラフ（右）（CSSのみで実装） |
| 6 | まとめ | 見出し・3列カード（価値と説明）・締めのCTAボタン |

### 全スライド共通の実装方針

#### スライドサイズとフォント

- **スライドサイズ：1920 × 1080 px（16:9）**
- **フォント：本文・UIは `'Manrope', system-ui, sans-serif`、数値・コードは `'Roboto Mono', monospace`**
- Google Fonts から `Manrope:wght@400;500;600;700;800` と `Roboto+Mono:wght@700` を `<link>` で読み込む
- ただしSLIDE.mdで別のフォントが指定されている場合はそちらを優先する

#### コンテンツ密度の原則

1920×1080pxの大きなキャンバスを有効に使い、スカスカな印象にならないよう以下を守ること：

- **見出しは大きく**：ページ内の主見出し（h1相当）は最低 `62px`、表紙タイトルは `100px` 以上を基準とする
- **本文・説明テキストは読みやすく**：本文は最低 `18px`、カード内説明は `18〜22px` を基準とする（1920px基準でこのサイズ。0.75スケール後でも14〜16px相当になる）
- **コンテンツがキャンバスを埋める**：余白は設計上必要な呼吸感のためにとるが、コンテンツエリアが上下左右に偏らず、スライド全体に広がるようにする
- **カード・リストアイテムは縦に広げる**：`flex:1` や `min-height:0` を活用して、コンテンツカードがスライドの縦方向を埋めるようにする

#### ブラウザ表示のスケーリング

1920pxのスライドをブラウザで確認しやすいよう、以下のCSSをbodyに適用する：

    body {
      background: #E8EAED;
      padding: 40px 20px;
    }
    .slide-wrapper {
      width: 1920px;
      transform-origin: top left;
      transform: scale(0.75);
      margin-bottom: -270px; /* 1080 × (1 - 0.75) = 270px */
    }
    .slide-outer {
      margin: 0 auto 32px;
      width: 1440px; /* 1920 × 0.75 */
    }

各スライドは `.slide-outer > .slide-wrapper > .slide` の構造で出力し、ブラウザ上では1440×810px相当で表示されるようにする。

#### CSSカスタムプロパティの定義

HTMLの `<style>` タグ内で以下のカスタムプロパティを定義する。SLIDE.mdの値を埋める：

    :root {
      --color-primary:    [SLIDE.mdのPrimaryカラー];
      --color-secondary:  [SLIDE.mdのSecondaryカラー];
      --color-accent:     [SLIDE.mdのAccentカラー];
      --color-background: [SLIDE.mdのBackgroundカラー（canvas等）];
      --color-text:       [SLIDE.mdのTextカラー（ink等）];
      --color-text-sub:   [SLIDE.mdのText Subカラー（body等）];
      --color-text-muted: [SLIDE.mdのText Mutedカラー（muted等）];
      --color-surface:    [SLIDE.mdのsurface-cardカラー。定義がなければBackground+8%暗くした値];
      --color-surface-soft: [SLIDE.mdのsurface-softカラー。定義がなければBackground+4%暗くした値];
      --color-border:     [SLIDE.mdのhairline/borderカラー。定義がなければrgba(0,0,0,0.08)];
      --font-display:     [SLIDE.mdの見出しフォント], system-ui, sans-serif;
      --font-body:        [SLIDE.mdの本文フォント（見出しと異なる場合）], system-ui, sans-serif;
      --font-mono:        [SLIDE.mdのモノスペースフォント / 'DM Mono'], monospace;
      --gradient-main: linear-gradient(90deg, [Primary], [Secondary], [Accent]);
      --gradient-vert: linear-gradient(180deg, [Primary], [Secondary], [Accent]);
      --slide-padding-v:  [SLIDE.mdの上下余白];
      --slide-padding-h:  [SLIDE.mdの左右余白];
    }

    .slide {
      width: 1920px;
      height: 1080px;
      background: var(--color-background);
      font-family: var(--font-display);
      color: var(--color-text);
      box-sizing: border-box;
      position: relative;
      overflow: hidden;
    }

**グラデーションが「なし」のデザインの場合**：`--gradient-main` / `--gradient-vert` には新たにグラデーションを作らず、**Primaryカラーの単色**を設定する（例：`--gradient-main: #0B57D0;`）。`background: var(--gradient-main)` もグラデーションテキストもそのまま単色として描画されるため、以降のページ仕様の `var(--gradient-main)` / `var(--gradient-vert)` は読み替え不要でそのまま使える。ソースにないグラデーションを勝手に追加しないこと。

**重要：カード・サーフェスの背景色について**
- カードの背景は **白（#ffffff）を使わない**。`var(--color-surface)`（surface-card相当）または `var(--color-surface-soft)` を使う
- ボーダーは `rgba(0,0,0,0.04)` を使わず、`var(--color-border)`（hairline相当）を使う
- これによりウォームトーン・ナチュラルなデザインが保たれる

#### 全ページ共通の装飾要素

すべてのスライド（ページ1を除く）に以下を適用する。ただし、**SLIDE.mdで対応する定義が「なし」となっている要素は適用しない**（例：Background Accentが「なし」なら（A）を置かない、縦バーが「なし」なら（B）を置かない、ブランドフッターが「なし」なら（D）を置かない）：

**（A）背景アクセント丸**
スライドの隅に `position:absolute` の半透明グラデーション円を2〜3個配置する。例：

    <div style="position:absolute;top:-180px;right:-120px;width:520px;height:520px;
      border-radius:50%;background:radial-gradient(circle,rgba([Secondary-RGB],0.12),transparent 65%);
      pointer-events:none;"></div>
    <div style="position:absolute;bottom:-220px;left:180px;width:560px;height:560px;
      border-radius:50%;background:radial-gradient(circle,rgba([Primary-RGB],0.08),transparent 65%);
      pointer-events:none;"></div>

**（B）セクション見出しバー**
ページの見出し左に縦バーを置く（ページ2を除く）：

    <div style="width:8px;height:40px;border-radius:4px;background:var(--gradient-vert);"></div>

**（C）ページ番号（右下）**
全ページ右下に `position:absolute; right:[左右余白]; bottom:32px` で配置：

    <div style="position:absolute;right:80px;bottom:32px;display:flex;align-items:baseline;gap:6px;z-index:2;">
      <span style="font-family:var(--font-mono);font-size:16px;font-weight:800;line-height:1;
        background:var(--gradient-main);-webkit-background-clip:text;background-clip:text;
        -webkit-text-fill-color:transparent;">0X</span>
      <span style="font-family:var(--font-mono);font-size:12px;color:var(--color-text-muted);">/ 6</span>
    </div>

**（D）ブランドフッター（左下）**
ページ2を除く全ページ左下に表示：

    <div style="position:absolute;left:[左右余白];bottom:34px;display:flex;align-items:center;gap:10px;z-index:2;">
      <span style="font-size:13px;font-weight:700;color:var(--color-text-muted);letter-spacing:0.02em;">[ブランド名またはデザインシステム名]</span>
    </div>

**（E）カードコンポーネント**
コンテンツをカード形式で表示する場合の標準スタイル（角丸・影・枠線はSLIDE.mdのComponent Styleの値があればそちらを優先し、なければ以下を目安にする）：

    background:var(--color-surface); border:1px solid var(--color-border); border-radius:16px;
    box-shadow:0 1px 2px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.03); padding:36px;

- `--color-surface` は白（#fff）ではなくSLIDE.mdのsurface-card相当の色（ウォームホワイト・クリーム系）を使う
- `--color-border` は `rgba(0,0,0,0.04)` ではなくSLIDE.mdのhairline相当の色（ウォームグレー系）を使う
- 以降のページ仕様に出てくる「白背景カード」は、すべてこの（E）標準カードスタイルを指す。暗色デザインの場合は影を控えめにする（またはなしにする）

- JavaScriptは使用しない
- `<section class="slide">` を `.slide-wrapper` 内に置き、各ページを縦に並べる

### ページ1：デザインシステム概要ページの仕様

このページは「仕様書ページ」として生成する。カラーパレット・スライドで使うパーツ・タイポグラフィ・レイアウト・Do/Don'tを1画面に集約し、ユーザーがデザインシステムを一目で把握できるようにする。

**レイアウト構造：**

    ┌─────────────────────────────────────────────────────────────────────┐
    │  [ヘッダー] ロゴ/スパーク  DESIGN SYSTEM名（グラデーションテキスト）  │
    │                                     Style Sheet / v1.0 · 16:9    │
    ├─────────────────────────────────────────────────────────────────────┤
    │  [基本カラーパレット 行]                                              │
    │  ブランドの主要色 | ベースカラー（背景・テキスト・枠線）| データ・状態カラー（あれば） │
    ├────────────────────┬─────────────────────────┬──────────────────────┤
    │ スライドで使うパーツ │ タイポグラフィ            │ Do & Don't           │
    │ （flex:1.6）       │ （flex:1.05）            │ （flex:1）           │
    │ ボタン・矢印・      │ タイプスケール一覧・       │ 緑背景のDo欄         │
    │ グラデーション・    │ フォント名・              │ 赤背景のDon't欄       │
    │ チップ・カード等    │ レイアウト仕様            │                      │
    └────────────────────┴─────────────────────────┴──────────────────────┘

**実装の詳細：**

- **ヘッダー**：左にデザインシステム名をグラデーションテキスト（`font-size:48px; font-weight:800; background:var(--gradient-main); -webkit-background-clip:text; -webkit-text-fill-color:transparent`）で表示し、右に「Style Sheet / v1.0 · 16:9」を `font-size:17px; color:var(--color-text-muted)` で表示する
- **カラーパレット行**：ブランドに合わせた主要色（**10〜12色**程度）を1行に収める。色をグループ（例：Brand / Accent / Text / Surface など機能別に3〜4グループ）に分け、グループ間を `width:1px; background:var(--color-border)` の縦線セパレーターで区切る。**実装方法：** 全体を `display:flex; gap:16px; align-items:stretch` で横並びにし、各グループを `display:flex; gap:10px` でまとめる。各カラーカード：`border-radius:10px; overflow:hidden; flex:1` で囲み、上部にスウォッチ（`height:76px`）、下部に `padding:8px 10px` でカラー名（`font-size:18px; font-weight:700`）とHEXコード（`font-size:15px; font-family:var(--font-mono); color:var(--color-text-muted)`）を縦並び。**選色の目安：** Brand系3色・Accent系2色・Text系3色・Surface系3色（計11色）など。StatusカラーやOn-Dark系など多すぎると1行に収まらないため省く。
- **下段3カラム**：flexboxで `flex:1.6 / flex:1 / flex:0.85` に分割し、各カラムを `background:var(--color-surface); border:1px solid var(--color-border); border-radius:16px` のカードで囲む（白背景は使わない）
- **下段左「スライドで使うパーツ」**：`display:grid; grid-template-columns:1fr 1fr; gap:14px 28px; align-content:space-between` で2列グリッドに配置し、コンテンツが上詰めにならないよう上下均等に分散する。ソースに特定のコンテンツがなくても、デザインシステムの雰囲気・カラー・フォントに合わせて **ボタン・タイトルバー・番号付きリスト・箇条書き・カード・チップ/ラベル・仕切り線・ページ番号** の8パーツを必ず生成する。各パーツのサイズ基準：パーツラベル `font-size:15px; font-weight:700`、ボタン `font-size:18px; padding:14px 28px; border-radius:999px`、番号・アイコン円 `width:30px; height:30px`、箇条書きドット `width:10px; height:10px`、リストテキスト `font-size:18px`
- **下段中「タイポグラフィ」**：タイプスケール（表紙タイトル→大見出し→中見出し→小見出し→本文大→本文小→キャプション、計7段階）を `display:flex; flex-direction:column; justify-content:space-between; flex:1` で均等配置し、各行を `display:flex; justify-content:space-between` で左にサンプルテキスト・右に `font-size:15px; font-family:var(--font-mono)` でウェイト・サイズ情報を表示する。下部にフォント名確認ブロックを追加する。さらに「レイアウト」としてスライドサイズ・余白・整列をドット線区切りで記載する
- **下段右「Do & Don't」**：上半分にDo欄（背景はSLIDE.mdのSuccess系の薄い色、なければ `#E6F4EA`）、下半分にDon't欄（背景はPrimaryカラーの薄い色、なければ `#FCE8E6`）を配置する。DoアイコンはSuccessカラーの丸（`width:34px; height:34px`）に `font-size:18px` で `✓`、Don'tアイコンはPrimaryカラーの丸（`width:34px; height:34px`）に `font-size:18px` で `✕` を表示する。Do/Don'tラベルは `font-size:22px; font-weight:800`、本文テキストは `font-size:18px; line-height:1.55`
- **セクション見出し**：各カラムの左上に `width:6px; height:28px; border-radius:3px; background:var(--gradient-vert)` の縦バー＋タイトルを `font-size:26px; font-weight:800` で表示する
- このページには全ページ共通のブランドフッター・ページ番号は**適用しない**（ページ1はそれ自体が仕様書のため）

### ページ2〜6のコンテンツ方針

**ページ2〜6のコンテンツはすべてダミーデータで生成する。**

- ソースから読み取ったタイトル・本文・数値・固有名詞をそのままコンテンツに使用しないこと
- 会社名・組織名は必ず **「サンプル株式会社」** に統一する（ソースが個人・チーム・ブランド名であっても同様）
- ページタイトル・本文・グラフデータ・カード説明文はすべて汎用的なビジネスプレゼンテーション向けのダミーテキストにする
- ただし、**デザインシステムのカラー・フォント・余白・スタイルは正確に反映**すること（あくまでコンテンツの文字情報がダミーなのであって、見た目のデザインはソースに忠実に）

**ダミーコンテンツの例（参考）**

| ページ | タイトル例 | 内容例 |
|---|---|---|
| 2（表紙） | 年度事業計画 / 新サービスのご紹介 | サブタイトル＋「サンプル株式会社」＋日付 |
| 3（セクション） | Section 01 — Overview / 事業概要 | 概要説明文 |
| 4（箇条書き） | 成長を支える3つの柱 / 重点施策 | 顧客体験・業務効率化・新規事業などのダミー項目 |
| 5（グラフ） | 四半期業績の推移 / 売上構成比 | 架空の数値データ（Q1〜Q4など） |
| 6（まとめ） | 今後の取り組み / 次のステップ | 強みの強化・新市場開拓・基盤整備などのダミーカード |

### ページ2〜6の実装仕様

**ページ2（表紙）**

- `display:flex; flex-direction:column; align-items:center; justify-content:center; text-align:center; padding:0 160px`
- ロゴやブランドのシンボル（`width:96px; height:96px; margin-bottom:30px`）を上部に表示する（SVGアイコンがない場合はグラデーション円で代替）
- アイキャッチテキスト（`font-size:16px; font-weight:700; letter-spacing:0.32em; text-transform:uppercase; color:var(--color-text-muted)`）でカテゴリや概要を表示する
- メインタイトル `h1`：グラデーションテキスト（`font-size:118px; font-weight:800; letter-spacing:-0.03em; line-height:1; background:var(--gradient-main); -webkit-background-clip:text; -webkit-text-fill-color:transparent`）
- サブタイトル：`font-size:30px; font-weight:500; color:var(--color-text-sub); margin-top:30px`
- グラデーションライン：`width:340px; height:10px; border-radius:999px; background:var(--gradient-main); margin:46px 0`
- 日付・組織名：`font-family:var(--font-mono); font-size:18px; font-weight:600; color:var(--color-text-muted)` で並べ、区切りに小さな `●` を挿入する

**ページ3（セクションタイトル）**

- `display:flex; flex-direction:column; justify-content:center; padding:0 140px`
- 背景装飾として巨大な透明数字を右側に `position:absolute; right:90px; top:50%; transform:translateY(-50%)` で配置：`font-family:var(--font-mono); font-size:480px; font-weight:800; opacity:0.10; background:var(--gradient-vert); -webkit-background-clip:text; -webkit-text-fill-color:transparent`
- コンテンツ本体：`display:flex; align-items:stretch; gap:28px` で左に `width:10px; border-radius:5px; background:var(--gradient-vert)` の縦バー、右に以下を縦並び
  - セクション番号：`font-family:var(--font-mono); font-size:18px; font-weight:700; letter-spacing:0.28em; color:var(--color-text-muted); text-transform:uppercase`
  - 大見出し `h1`：`font-size:96px; font-weight:800; letter-spacing:-0.03em; line-height:1.04`
  - 説明テキスト：`font-size:24px; font-weight:500; color:var(--color-text-sub); line-height:1.6; max-width:920px`

**ページ4（箇条書き）**

- `display:flex; flex-direction:column; padding:[上下余白] [左右余白]`
- 見出しエリア：縦バー＋`h1`（`font-size:62px; font-weight:800; letter-spacing:-0.02em`）を `display:flex; align-items:center; gap:16px; margin-bottom:14px` で横並び
- サブテキスト：`font-size:24px; font-weight:500; color:var(--color-text-muted); margin-bottom:48px; margin-left:24px`
- リストエリア：`display:flex; gap:20px; flex:1; min-height:0`（横並び。カード数が3なら横3列、4以上なら縦リストに変える）
- 各リストアイテム：`background:var(--color-surface); border:1px solid var(--color-border); border-radius:14px; padding:32px 36px; flex:1` のカード内に縦並びで、番号円（`width:64px; height:64px; border-radius:50%; background:var(--gradient-main); color:#fff; font-weight:800; font-size:26px`）・見出し（`font-size:30px; font-weight:700; margin-top:20px`）・説明文（`font-size:21px; color:var(--color-text-muted); line-height:1.6; margin-top:12px`）を配置（白背景は使わない）

**ページ5（グラフ）**

- `display:flex; flex-direction:column; padding:[上下余白] [左右余白]`
- 見出しエリア：縦バー＋`h1`（`font-size:62px`）＋サブテキスト（ページ4と同構造）
- グラフエリア：`display:flex; gap:40px; flex:1; min-height:0`
  - **棒グラフ（`flex:1.3`）**：`background:var(--color-surface-soft); border:1px solid var(--color-border); border-radius:16px` のカード内に縦棒を `align-items:flex-end` で並べる。最新月のバーのみグラデーション（`background:var(--gradient-vert); box-shadow:0 4px 12px rgba([Secondary-RGB],0.3)`）を使用し強調する。他のバーはPrimaryカラーのアルファ値違いで濃淡をつける
  - **ドーナツグラフ（`flex:1`）**：`background:var(--color-surface-soft); border:1px solid var(--color-border); border-radius:16px` のカード内に `conic-gradient` で円グラフを実装（外円`width:280px; height:280px`）。中心は `background:var(--color-background); border-radius:50%; width:160px; height:160px` に主要値（`font-size:44px; font-weight:800`）と凡例ラベル（`font-size:18px`）を表示する。右側に凡例をカラーブロック付きで縦列する

**ページ6（まとめ）**

- `display:flex; flex-direction:column; padding:[上下余白] [左右余白]`
- 見出しエリア：縦バー＋`h1`（`font-size:62px`）＋サブテキスト（ページ4と同構造）
- カードエリア：`display:flex; gap:32px; flex:1; min-height:0`
- 各カード（`background:var(--color-surface); border:1px solid var(--color-border); border-radius:16px; padding:48px 40px`）内に縦並びで（白背景は使わない）：
  - アイコン正方形：`width:72px; height:72px; border-radius:18px`（背景はPrimary/Secondary/Accentカラーを10〜15%透明にした色、中に1〜2文字のブランドカラーの漢字や英字を `font-size:32px; font-weight:800` で表示）
  - 見出し：`font-size:34px; font-weight:800; letter-spacing:-0.01em; margin-top:24px`
  - 説明文：`font-size:22px; color:var(--color-text-muted); line-height:1.6; margin-top:14px`
- CTAボタン：`display:flex; justify-content:center; margin-top:44px` に `padding:20px 60px; border-radius:999px; background:var(--gradient-main); color:#fff; font-size:28px; font-weight:700; box-shadow:0 6px 20px rgba([Secondary-RGB],0.35)` で締めのメッセージを表示する

### 生成完了の通知

sample.html生成後、以下のようにユーザーに伝える：

> 「`SLIDE-md/SLIDE-md-{source}/` フォルダにSLIDE.mdとsample.htmlを生成しました。
>
> - `SLIDE.md`：デザインシステムの定義書。AIツールに渡す際のメインファイルです。
> - `sample.html`：ブラウザで開くとデザインを確認できます（各スライドが75%縮小で表示されます）。AIツールへの参考資料としても使えます。
>
> このSLIDE.mdとsample.html、および別途作成したスライドパターンファイルをAIツールに渡すことで、このデザインでスライドを生成できます。」
