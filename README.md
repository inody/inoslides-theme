# inoslides-theme

research report 用に整理した Slidev theme です。

## Local Setup

theme repo を clone した直後の、ほぼ何もない環境から local で sample deck を立ち上げる最小手順です。

```bash
git clone https://github.com/inody/inoslides-theme.git
cd inoslides-theme
npm install
npm run dev -- example.md
```

build だけ確認したい場合は以下です。

```bash
npm run build -- example.md
```

手元の deck file 名が `slides.md` なら、同じ repo 内で以下のように起動できます。

```bash
npm run dev -- slides.md
```

## What Is Included

- `layouts/cover.vue`: frontmatter から cover を組み立てる layout
- `layouts/center.vue`: 中央寄せ 1 カラム layout
- `layouts/two-cols.vue`: 比率と align を指定できる 2 カラム layout
- `layouts/three-cols.vue`: 3 カラム layout
- `components/Panel.vue`: 汎用囲み component
- `styles/layouts.css`: layout と typography の single source of truth

## Run The Example

```bash
npm run dev -- example.md
```

この sample deck で layout と component の使い方を一通り確認できます。

## Use From Another Project

別の Slidev project からこの theme を使う場合は、theme の置き方で指定方法が変わります。

### 1. Theme を dependency として install する場合

project root の `package.json` に例えば以下のように入れます。

```json
{
  "dependencies": {
    "inoslides-theme": "git+https://github.com/inody/inoslides-theme.git"
  }
}
```

この場合、`slides.md` では package 名をそのまま使います。

```yaml
---
theme: inoslides-theme
---
```

これは `tmp/` で実際に build 確認済みの使い方です。

### 2. Theme directory を project 内に直接置く場合

例えば project root の直下に `theme/` directory を置いたなら、`slides.md` では相対 path を使います。

```yaml
---
theme: ./theme
---
```

### Recommendation

theme repo を fetch / install して使う前提なら、通常は `theme: inoslides-theme` です。
`theme: ./theme` は theme source を project 内に直接持っているときだけ使います。

## Cover Layout

`cover` は slot に生 HTML を書くより、frontmatter で指定して使う前提です。

```yaml
---
theme: .
coverDate: 2026-05
coverTitle: Research Progress Report
coverMeta:
  - Applied Mathematics Group
  - Your Name
coverBodyTitle: 本日のアウトライン
---
```

本文側には、outline など下部に置きたい内容だけを書きます。

## Two Cols Layout

使える frontmatter:

- `leftSpan`: 左カラムの幅。`1` から `9`。default は `5`
- `leftAlign`: `left | center | right`
- `rightAlign`: `left | center | right`
- `topAlign`: top slot の text align
- `bottomAlign`: bottom slot の text align

使える slots:

- `top`
- `left`
- `right`
- `lowerleft`
- `lowerright`
- `bottom`

## Three Cols Layout

使える slots:

- `top`
- `left`
- `middle`
- `right`
- `bottom`

## Panel Component

```md
<Panel title="Key Point">

- ここに本文

</Panel>
```

`title` prop の代わりに `#title` slot も使えます。

## Still Optional Before Publishing

- npm publish を想定するなら `package.json` の `private` は見直す
- ライセンスやスクリーンショットが必要なら追加する