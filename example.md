---
theme: .
title: Theme Example
coverDate: 2026-05
coverTitle: Research Report Theme Example
coverMeta:
  - Sample deck for layout and component usage
  - inoslides-theme
coverBodyTitle: Contents
---

1. Default typography
2. Panel component
3. PseudoCode component
4. Center layout
5. Two-cols layout
6. Two-cols with lower row
7. Three-cols layout

---

# Default Typography

## Heading Spacing

通常スライドでの見出し・本文・**Markdown の強調**・数式の見え方を確認するためのページです。

- 本文は theme の sans stack を使います
- 見出しは `h1`, `h2`, `h3` まで bold です
- `h1` の次に `h2` が来る場合は少しだけ余白が広がります

$$
\mathcal E^{\mathrm{fwd}} + \mathcal E^{\mathrm{bwd}} \le 2\mathcal C_E(\varepsilon_f^2 + \varepsilon_F^2)
$$

---

# Panel Component

<Panel title="Key Point">

- `Panel` は囲み用の汎用 component です
- `title` prop か `#title` slot を使えます
- KaTeX もそのまま入れられます: $f(x,t,m_t) \approx \hat f_\psi(x,t)$

</Panel>

---

# PseudoCode Component

<PseudoCode title="DeepGSB Algorithm">

1. **Backward drift update** ($\phi$)
  - Generate forward trajectories using $(Y_\theta, Z_\theta)$
  - Update $(\hat{Y}_\phi, \hat{Z}_\phi)$ to minimize $\mathcal L(\phi)$

2. **Forward drift update** ($\theta$)
  - Generate backward trajectories using $(\hat{Y}_\phi, \hat{Z}_\phi)$
  - Update $(Y_\theta, Z_\theta)$ to minimize $\mathcal L(\theta)$

</PseudoCode>

---
layout: center
---

# Center Layout

1 枚まるごと中央寄せしたいときに使います。

---
layout: two-cols
leftSpan: 6
leftAlign: left
rightAlign: center
topAlign: left
bottomAlign: right
---

::top::

# Two Cols

::left::

## Left

- `leftSpan` で比率を指定
- `leftAlign` / `rightAlign` で左右の text align を指定

::right::

<Panel title="Right Panel">

右側に component を置く例です。

</Panel>

::bottom::

bottom slot

---
layout: two-cols
leftAlign: left
rightAlign: left
---

::left::

## Upper Left

- 上段

::right::

## Upper Right

- 上段

::lowerleft::

## Lower Left

- `lowerleft` / `lowerright` を置くと下段が増えます

::lowerright::

## Lower Right

- 下段

---
layout: three-cols
class: text-left
---

::top::

# Three Cols

::left::

## Left

- column 1

::middle::

## Middle

- column 2

::right::

## Right

- column 3

::bottom::

.