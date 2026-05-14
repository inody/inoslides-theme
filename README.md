# inoslides-theme

Imperial report 用に整理した Slidev theme です。

## What Is Included

- `layouts/cover.vue`: frontmatter から cover を組み立てる layout
- `layouts/center.vue`: 中央寄せ 1 カラム layout
- `layouts/two-cols.vue`: 比率と align を指定できる 2 カラム layout
- `layouts/three-cols.vue`: 3 カラム layout
- `components/Panel.vue`: 汎用囲み component
- `styles/layouts.css`: layout と typography の single source of truth

## Run The Example

```bash
npm exec -- slidev example.md
```

この sample deck で layout と component の使い方を一通り確認できます。

## Cover Layout

`cover` は slot に生 HTML を書くより、frontmatter で指定して使う前提です。

```yaml
---
theme: .
coverDate: 2026-05
coverTitle: Imperial College London留学 中間報告
coverMeta:
  - 数理工学研究領域
  - 井上大輔
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