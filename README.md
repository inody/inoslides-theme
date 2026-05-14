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
- `components/PseudoCode.vue`: 疑似コード・アルゴリズム表示用 component
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

## Migration From An Existing Deck

既存 deck を `inoslides-theme` に差し替えるときは、次の順で進めると安全です。

1. `package.json` に `inoslides-theme` を追加する
2. `slides.md` の frontmatter を `theme: inoslides-theme` に変更する
3. repo 内の local override file を確認する
4. theme にない custom layout を置き換える
5. dev と build の両方で検証する

### 1. Install

```bash
npm install inoslides-theme@git+https://github.com/inody/inoslides-theme.git
```

### 2. Switch The Theme Name

`slides.md` では package 名をそのまま指定します。

```yaml
---
theme: inoslides-theme
---
```

### 3. Check For Shadowing Files

Slidev では project 側の file が theme 側より優先されます。
以下のような同名 file が repo 内に残っていると、theme の更新が反映されません。

- `global-bottom.vue`
- `layouts/cover.vue`
- `layouts/center.vue`
- `layouts/two-cols.vue`
- `layouts/three-cols.vue`
- `components/Panel.vue`

theme の layout / component を使いたい場合は、これらの local file をできるだけ置かない方が安全です。

### 4. Replace Deck-Specific Layouts

`inoslides-theme` が提供するのは次です。

- `cover`
- `center`
- `two-cols`
- `three-cols`
- `Panel`

`emph*` のような deck-specific custom layout は含まれていないので、必要に応じて `Panel` や既存 layout に置き換えてください。

### 5. Image Paths Under `public/`

`public/` 配下の画像は、`/public/...` ではなく root path を Vue binding で渡す方が安全です。

```html
<img :src="'/figures/example.png'" />
```

この形なら、dev の warning と build 時の import 解釈ずれを避けやすくなります。

### 6. Validate Both Dev And Build

dev server だけ通っても、build で落ちることがあります。最低限、両方を確認してください。

```bash
npx slidev slides.md
npx slidev build slides.md
```

### 7. If `Panel` Does Not Render

次を確認してください。

- repo 内に `components/Panel.vue` が残っていないか
- 対象 slide の frontmatter 区切りが曖昧でないか

必要なら、その slide の先頭で明示的に frontmatter を切ると安定します。

```md
---
layout: default
---

## Main Result

<Panel title="Key Point">
...
</Panel>
```

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

## PseudoCode Component

```md
<PseudoCode title="DeepGSB Algorithm">

1. **Backward drift update** ($\phi$)
  - Generate forward trajectories using $(Y_\theta, Z_\theta)$
  - Update $(\hat{Y}_\phi, \hat{Z}_\phi)$ to minimize $\mathcal L(\phi)$

2. **Forward drift update** ($\theta$)
  - Generate backward trajectories using $(\hat{Y}_\phi, \hat{Z}_\phi)$
  - Update $(Y_\theta, Z_\theta)$ to minimize $\mathcal L(\theta)$

</PseudoCode>
```

`PseudoCode` は code block ではなく通常の slot content を使うので、Markdown と KaTeX をそのまま併用できます。
`label` prop を指定すると左上の pill 表示を差し替えられます。

## Still Optional Before Publishing

- npm publish を想定するなら `package.json` の `private` は見直す
- ライセンスやスクリーンショットが必要なら追加する