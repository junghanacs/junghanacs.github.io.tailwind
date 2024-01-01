---
author: Hugo Authors
title: Math Typesetting
date: 2019-03-08
description: A brief guide to setup KaTeX
math: true
---

Mathematical notation in a Hugo project can be enabled by using third party JavaScript libraries.
<!--more-->

In this example we will be using [KaTeX](https://katex.org/)

- Create a partial under `/layouts/partials/math.html`
- Within this partial reference the [Auto-render Extension](https://katex.org/docs/autorender.html) or host these scripts locally.
- Include the partial in your templates like so:  

```bash
{{ if or .Params.math .Site.Params.math }}
{{ partial "math.html" . }}
{{ end }}
```

- To enable KaTex globally set the parameter `math` to `true` in a project's configuration
- To enable KaTex on a per page basis include the parameter `math: true` in content files

**Note:** Use the online reference of [Supported TeX Functions](https://katex.org/docs/supported.html)

{{< math.inline >}}
{{ if or .Page.Params.math .Site.Params.math }}
<!-- KaTeX -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.css" integrity="sha384-zB1R0rpPzHqg7Kpt0Aljp8JPLqbXI3bhnPWROx27a9N0Ll6ZP/+DiW/UqRcLbRjq" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.js" integrity="sha384-y23I5Q6l+B6vatafAwxRu/0oK/79VlbSz7Q9aiSZUvyWYIYsd+qj+o24G5ZU2zJz" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous" onload="renderMathInElement(document.body);"></script>
{{ end }}
{{</ math.inline >}}

### Examples

{{< math.inline >}}
<p>
Inline math: \(\varphi = \dfrac{1+\sqrt5}{2}= 1.6180339887…\)
</p>
{{</ math.inline >}}

Block math:
$$
 \varphi = 1+\frac{1} {1+\frac{1} {1+\frac{1} {1+\cdots} } } 
$$


# HEelo

$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$


$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}
$$

## 표기법: 조합

-   조합을 의미하는 Combination 을 다음과 같이 괄호를 써서 위아래로 표기하고, 이항 계수라 부른다.
-   의미는 똑같지만 $$aCb$$ 는 $$a \times C \times b$$로 착각하기 좋은 모양이라 $$\binom{a}{b}$$를 선호한다.

$$

\begin{align}
a C b & = \binom{a}{b} \\
3 C 2 & = \binom{3}{2} = 3 \\
\end{align}

$$


## 이항 정리

****Binomial Theorem****

$$

\begin{align}
(x+y)^n & = \sum_{j=0}^n \binom{n}{j} x^{n-j} y^j \\
    & = \binom{n}{0} x^{n} y^0
        + \binom{n}{1} x^{n-1} y^1
        + ...
        + \binom{n}{n-1} x^{1} y^{n-1}
        + \binom{n}{n} x^{0} y^{n} \\
\end{align}

$$

중학교 때 배운 곱셈 공식을 생각해 볼 수 있을 것이다.

$$(a+b)^2 = a^2 + 2ab + b^2$$

이 등식의 계수는 왜 $$1, 2, 1$$ 이 될까?

식을 전개하는 과정이 a 와 b 가 들어있는 주머니에서 중복을 무시하고 2 번 꺼내서 나열하는 것과 같기 때문이다.

$$

\begin{array}{rlllllllll}
(a+b)^2 & = & a^2 & + & 2ab & + & b^2 \\
        &   & aa  &   & ab  &   & bb  \\
        &   &     &   & bb  &   &     \\
\end{array}

$$

$$(a+b)^3$$ 형태도 마찬가지다.

$$

\begin{array}{rlllllllll}
(a+b)^3 & = & a^3 & + & 3a^2b & + & 3ab^2 & + & b^3 \\
        &   & aaa &   & aab   &   & bba   &   & bbb \\
        &   &     &   & aba   &   & bab   &   &     \\
        &   &     &   & baa   &   & abb   &   &     \\
\end{array}

$$

즉, $$ (a+b)^n $$ 형태의 각 항의 계수는 조합 $$\binom{n}{k}$$ 형태로 표현할 수 있다.
