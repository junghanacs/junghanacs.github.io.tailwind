---
title: "Org-mode Examples for Hugo Blogging"
author: ["Junghan Kim"]
description: "Org 파일로 Hugo 블로깅 위한 예제 (Org -&gt; Markdown)"
date: 2023-06-05
lastmod: 2024-01-08T15:59:00+09:00
tags: ["hugo", "org-mode"]
draft: false
toc: true
math: true
---

Org-mode 에서 작성한 문서를 Hugo Markdown 으로 변환하기는 쉽다. 근데 각주, 인용,
태그, 요약, 코드, 일부 내용 감추기 등을 어떻게 하는가? 여기에 대한 답을 찾는다.
ox-hugo 의 모든 예제는 다음 주소에 있다. 여기서 찾아보자.&nbsp;[^fn:1]

<!--more-->


## <span class="org-todo done DONT">DONT</span> Ox-Hugo Header and Toc Generation {#ox-hugo-header-and-toc-generation}

<span class="timestamp-wrapper"><span class="timestamp">[2024-01-03 Wed 10:45] </span></span> Toc 생성을 누가 할 것인가? 섹션 번호를 넣을 것인가?
헤드라인 레벨을 어디까지만 넣을 것인가? 에 대해서 문서에 따라 설정한다.
기본 정책은 Hugo 에서 생성하며 섹션 넘버

아래에 대략 정리

```text

#+title:
#+author:
#+email: junghanacs@gmail.com
#+language: ko
#+startup: fold
#+description:
,
#+macro: latest-export-date (eval (format-time-string "%F %T %z"))
#+macro: word-count (eval (count-words (point-min) (point-max)))
,
#+HUGO_SECTION:
#+HUGO_SERIES: "Emacs Guide"
#+HUGO_CATEGORIES: Emacs
#+EXPORT_FILE_NAME: jh-emacs.md

# #+options: ':t toc:4 num:t H:8
# #+hugo_custom_front_matter: :toc false

#+EXPORT_HUGO_PANDOC_CITATIONS: t
#+cite_export: csl

#+hugo: more
```


## Markup {#markup}

org-mode 마크업은 다음의 주소에서 확인 바람.&nbsp;[^fn:2]
ox-hugo 관련 내용은 다음 주소에서 확인.[^fn:3]
결과를 비교하면 org-code 와 verbatim 이 다르다. 맞춰줘야 한다.
verbatim 은 맞추기가 까다롭다. <kbd>kbd</kbd> 을 많이 사용하라!

> -   헤딩에는 _ 만 사용하라!
> -   본문에는 ~, = 깔끔하다.
> -   \*, / 은 편하게 사용하되 헤딩에만 피해라!

-   **org-bold** : \*
-   <kbd>org-code</kbd> : ~ (org-hugo-use-code-for-kbd t)
-   _org-italic_ : /
-   ~~org-strike~~ : +
-   <span class="underline">org-underline</span> : _
-   `org-verbatim` : =

{{< figure src="/images/20230614-2109-screenshot.png" >}}


## Summary 블록 {#summary-블록}

요약문은 한글, 컬럼 80 으로 2.5 줄 정도 가능하다. 간단히 쓰는게 항상 답이다.
간단히 요약하기란 쉽지 않다. 하지만 중요하다. 아래와 같이 포스팅 맨 위에
짧은 글을 넣으면 된다. `#+hugo: more` 이 구분자 역할을 한다. 왠만하면 문서에
html 코드를 넣고 싶지 않다. 제공하는 방법을 사용한다.

```text
My post summary.

#+hugo: more

My post content.
```


## Citation 서지 정보 {#citation-서지-정보}

다음과 같이 상/하단에 넣어야 한다. 레퍼런스를 달아 봅니다.
`SPC B i` 로 바인딩을 해 놓았다. (<a href="#citeproc_bib_item_1">Graham 2014</a>) (<a href="#citeproc_bib_item_2">Jethro Kuan 2022</a>)
citar 사용법을 여기서 다룰 것은 아니다. 아무튼 깔끔하게 들어간다.

```text
#+hugo_pandoc_citations: t
#+cite_export: csl
,[cite:@HaekeowaHwaga14]
#+print_bibliography:
Below, the "References" heading will be auto-inserted.
```

현재 라인 다음에 References 이 추가 된다.

## References

<style>.csl-entry{text-indent: -1.5em; margin-left: 1.5em;}</style><div class="csl-bib-body">
  <div class="csl-entry"><a id="citeproc_bib_item_1"></a>Graham, Paul. 2014. <i>해커와 화가</i>. <a href="http://www.yes24.com/Product/Goods/11775130">http://www.yes24.com/Product/Goods/11775130</a>.</div>
  <div class="csl-entry"><a id="citeproc_bib_item_2"></a>Jethro Kuan. 2022. “How I Take Notes with Org-roam.” 2022. <a href="https://jethrokuan.github.io/org-roam-guide/">https://jethrokuan.github.io/org-roam-guide/</a>.</div>
</div>


## Footnote 각주 관리 {#footnote-각주-관리}

각주는 이렇게 들어갑니다.&nbsp;[^fn:4]


## No Export {#no-export}

블로그 리포는 공개되어 있다. 여기에 Markdown 파일이 그대로 있다. 숨기고 싶은
또는 숨겨야 하는 내용이 분명히 있을 것이다. 그렇다면 org 파일에서 아래와 같은
방법으로 숨기면 된다. 아예 private 프로퍼티를 넣고 ox-hugo 에서 걸러주는
방법도 있다. 나는 왠만하면 다 내용을 오픈하고자 한다. 다만 문제가 되거나
퀄리티가 많이 부족한 부분은 `헤딩` 수준에서 숨기길 원한다.


### PRIVATE 설정 {#private-설정}

<span class="timestamp-wrapper"><span class="timestamp">[2023-07-10 Mon 10:10] </span></span> 내보내기 할 때 연결 된 노트가 미리 내보내기 되어 있어야
한다. 불편한 부분이다. 아직 내보내기 할 상태가 아닌데도 내보내기 할 필요가
있을까? 그렇다면 방법은 :private: 를 프로퍼티에 넣는다. (커스텀 수정)
URL 이 있다면 URL 링크로 변경 되고 그게 아니라면 텍스트로 표시 된다.


### noexport 태그 활용 {#noexport-태그-활용}

아래 헤딩은 안보입니다. 뭔가 더 있는데 안보이죠? 그럼 된겁니다.

```text
* 숨기고 싶은 헤딩이라면 태그를 달아라 :noexport:
```


### 파일 숨기기 (비추) {#파일-숨기기--비추}

연결 된 노트를 작성하는 경우라면 좋지 않은 방법이다. 링크를 블록하는
처리를 해줘야 한다.

```text
,:EXPORT_FILE_NAME: excluded-post
```


## 코드, 인용, 예시 블록 {#코드-인용-예시-블록}

-   ':' 을 애용한다. 1 라인 블록.

    ```text
    안녕하세요. 간단해서 좋습니다.
    ```
-   example : 앞에 탭 사이즈 공백이 들어 간다.
    ```text
        위에 요약문의 분량입니다. 대략 2.5 줄 정도 입니다. 한글로.
    ```
-   quote

    > 위에 요약문의 분량입니다. 대략 2.5 줄 정도 입니다. 한글로.
-   src
    소스코드 블록이다.
    ```emacs-lisp
        (with-eval-after-load 'ox-hugo
          (setq org-hugo-auto-set-lastmod 't
                org-hugo-section "posts"
                org-hugo-suppress-lastmod-period 43200.0
                )
    ```
-   center

    <style>.org-center { margin-left: auto; margin-right: auto; text-align: center; }</style>

    <div class="org-center">

    위에 요약문의 분량입니다. 대략 2.5 줄 정도 입니다. 한글로.

    </div>
-   verse

    <div class="verse">

    위에 요약문의 분량입니다. 대략 2.5 줄 정도 입니다. 한글로.<br />

    </div>


## 태그를 키워드로 변환 {#태그를-키워드로-변환}

키워드로 빼고 태그는 명시하는게 좋겠다. 태그와 카테고리는 관리가 필요하다.
지식 관리를 할 때 태그, 카테고리는 매우 중요한 분류 방법이다.
태그를 마구잡이로 잡는 것은 좋지 않은 방법이다. 대략 분류를 해놓고 그 안에서
설정을 하는게 좋다. 특히 디지털 가든에서는 태그 관리가 더 중요하다.
그래서 파일 태그가 이리저리 많더라도 변환 할 때는 키워드로 할당되도록 한다.
즉 블로그의 태그는 적절하게 관리한다. 자동화가 언제나 효과적인 것은 아니다.

```text
#+hugo_front_matter_key_replace: tags>keywords
```


## ob-translate 블록 번역 {#ob-translate-블록-번역}

<span class="timestamp-wrapper"><span class="timestamp">[2023-06-08 Thu 12:52]</span></span>
블록 번역 테스트.

```text
,#+BEGIN_SRC translate :src en :dest ko :noexport
```

코드 블록을 번역 하여 하단에 삽입한다.

```translate
  Doom is a configuration framework for GNU Emacs tailored for Emacs bankruptcy
  veterans who want less framework in their frameworks, a modicum of stability
  (and reproducibility) from their package manager, and the performance of a
  hand rolled config (or better). It can be a foundation for your own config or
  a resource for Emacs enthusiasts to learn more about our favorite operating
  system.
```

Doom 은 프레임워크에 적은 프레임워크, 패키지 관리자의 약간의 안정성(및 재현성),
수동 구성의 성능(또는 그 이상)을 원하는 Emacs 파산 베테랑을 위해 맞춤화된 GNU
Emacs 용 구성 프레임워크입니다. 자신의 구성을 위한 기초가 될 수도 있고 Emacs
애호가가 선호하는 운영 체제에 대해 자세히 알아볼 수 있는 리소스가 될 수도
있습니다.


## <span class="org-todo todo TODO">TODO</span> org-translate-mode {#org-translate-mode}



활용 방법이 있을까?


## Header Template {#header-template}



capture 를 하면 아래와 같이 노트의 타입에 맞게 헤더가 생성 된다. publish,
lastmod 는 직접 수정 한다. 그래야 깔끔하다.

```text
,:PROPERTIES:
,:ID:       3dcd5b7a-9e78-41a9-a3da-xxxxxxxx
,:END:
#+title: HELLO WORLD
#+date: [2023-06-22 Thu 10:27]
#+hugo_publishdate: <2023-06-22 Thu 10:27>
#+hugo_lastmod: <2023-06-22 Thu 10:27>
#+filetags:
#+HUGO_DRAFT: true
#+HUGO_SECTION: notes
```

노트를 캡처 하면 아래와 같다.

{{< figure src="/images/20230622-1048-screenshot.png" caption="<span class=\"figure-number\">Figure 1: </span>Sample notes after **org-roam-capture**" width="100%" >}}

그 다음에 template 을 가져 온다. 자동으로 가능한 부분을 거의 다 제거 했다.
내보내기 전에 확인하고 직접 하는 것이 노트에 대한 나의 자세가 아닐까 싶다.

아 물론 SETUPFILE 을 이용해서 표준화 시킬 수 있다. 그렇게 했었다. 근데 이 또한
섣부른 자동화가 아닐까? 하루에 1-2 개 노트를 만드는데 뭘 더 자동화 하려는
것인가?! 귀하게 다루자. 받들어 모시자.

```text

(hugofront "
,# :ROAM_ALIASES: \"==\"
#+SUBTITLE:
#+URL:
#+LANGUAGE: ko
,# #+STARTUP: overview

,# == TAGS ==
,# 🌱 🪴 🌳
#+filetags: :draft:
#+filetags: :seedling:
#+HUGO_TAGS:

,# == Taxonomies ==
,# #+HUGO_CATEGORIES:
,# #+HUGO_SERIES:

,# == Glossary ==
#+glossary_sources: glossary-general

,# == Front-matter ==
#+hugo_front_matter_key_replace: tags>keywords
,# #+hugo_front_matter_key_replace: aliases>nil
,# #+hugo_paired_shortcodes: hint details mermaid sidenote
#+EXPORT_HUGO_PANDOC_CITATIONS: t
,# #+print_bibliography:

,# == Summary ==

#+attr_shortcode: info
#+begin_hint" n> r> n>
",#+end_hint

#+hugo: more

* HIDDEN :noexport:
* ChangeLog :noexport:

")
```


## Details and Summary {#details-and-summary}

디테일은 따로 원하는 대로

details simple

{{< details >}}
detail only : You will learn that later below css section.
{{< /details >}}

detail with title

{{< details >}}
<summary>Why is this in <b>green</b>?</summary>

You will learn that later below css section.
{{< /details >}}

summary 블록을 사용하면 다음과 같다. 헤딩 레벨을 무시.

<summary>Why is this in <b>summary</b>?</summary>

일반 리스트는 헤딩 아래에 들어간다. 다음과 같다.

-   일반 리스트 Why is this in **green**?


## Images {#images}



이미지 내보내기 방법 org-download or org-attach 둘다 가능하다.


## Sidenote {#sidenote}



테스트
{{< sidenote >}}
사이드 노트에 대한 나의 사랑은 엄청 납니다.
{{< /sidenote >}} 사이드 노트 예제 입니다.

사이드노트
{{< sidenote >}}
아직 메뉴와 겹쳐지는 문제를 해결해야 합니다. 다만 사용하는데 지장 없습니다.
{{< /sidenote >}} 는 좋습니다.

숏코드는 tempel 에 hugoside 로 만들어 두었습니다.


## Math Typesetting {#math-typesetting}

```text
ox-hugo/doc/ox-hugo-manual.org:1486
```

By default, the inline and block equations are exported to Markdown in a format
that can be rendered using [MathJax](https://www.mathjax.org/#gettingstarted). You can find one MathJax config example

기본적으로 인라인 및 블록 방정식은 [MathJax](https://www.mathjax.org/#gettingstarted)를 사용하여 렌더링할 수 있는 형식으로
Markdown 으로 내보내집니다. 하나의 MathJax 구성 예제를 찾을 수 있습니다

<kbd>ox-hugo</kbd> indirectly extends from <kbd>ox-html</kbd> and so it also inherits a different way
of exporting latex equations --- by exporting them to images.

~ox-hugo~는 ~ox-html~에서 간접적으로 확장되므로 라텍스 방정식을 이미지로
내보내는 다른 방식도 상속받습니다.


### `Inline` equations {#inline-equations}

```org
- Inline equations are wrapped between =\(= and =\)=.
  - =$= wrapping also works, but it is not preferred as it comes with
    restrictions like "there should be no whitespace between the
    equation and the =$= delimiters".

    So =$ a=b $= will not work (it will look like: $ a=b $), but
    =$a=b$= will work (it will look like: $a=b$).

    On the other hand, both =\(a=b\)= (it will look like: \(a=b\)) and
    =\( a=b \)= (it will look like: \( a=b \)) will work.

- =$= 래핑도 작동하지만 "방정식과 =$= 구분 기호 사이에 공백이 없어야 한다"와
  같은 제한이 있으므로 선호되지 않습니다. 따라서 =$ a=b $=는 작동하지 않지만 ($
  a=b $처럼 보입니다) =$a=b$=는 작동합니다 ($a=b $처럼 보입니다). 반면에
  =\(a=b\)=는 모두 작동합니다 (다음과 같이 보입니다): (\(a=b\)) 및 =\((a=b \)=
  (다음과 같이 표시됩니다: \((a=b \)) 모두 작동합니다
```

-   One-per-line equations are wrapped between `\[` and `\]` or `$$` delimiters.

For example, below in Org:

```org
LaTeX formatted equation: \( E = -J \sum_{i=1}^N s_i s_{i+1} \)
```

will look like this in Hugo rendered HTML (using MathJax):

LaTeX formatted equation: \\( E = -J \sum\_{i=1}^N s\_i s\_{i+1 }\\)

Here's another example, taken from [Org Info: LaTeX fragments](https://orgmode.org/manual/LaTeX-fragments.html "Emacs Lisp: (info \"(org) LaTeX fragments\")"):

```text
If $a^2=b$ and \( b=2 \), then the solution must be either
$$ a=+\sqrt{2} $$ or \[ a=-\sqrt{2} \]
```

Above renders to below using Mathjax:

If \\(a^2=b\\) and \\( b=2 \\), then the solution must be either
\\[ a=+\sqrt{2} \\] or \\[ a=-\sqrt{2} \\]

<div class="note">

Note that the last two equations show up on their own lines because
those equations are wrapped in <kbd>\[ .. \]</kbd>.

마지막 두 방정식은 ~\\[ .. \\]~로 묶여 있기 때문에 자체 줄에 표시된다는 점에
유의하세요.

</div>


### `latex` Environments {#latex-environments}

`ox-hugo` support latex environments. So below in Org buffer:

```org
\begin{equation}
\label{eq:1}
C = W\log_{2} (1+\mathrm{SNR})
\end{equation}
```

will render as below using MathJax:

\begin{equation}
\label{eq:1}
C = W\log\_{2} (1+\mathrm{SNR})
\end{equation}

You can find many more equation examples at testtag(equations).


#### aligned 으로 수식 강제 줄바꿈 {#aligned-으로-수식-강제-줄바꿈}

-   begin{aligned}, end{aliend}로 수식 시작
-   &amp;=로 align 할 위치 지정

<!--listend-->

```org
\begin{aligned}
H(Play)&=-\sum_{i=1}^c p_i\log_2 p_i \\
&=-(\frac{5}{14}log_2\frac{5}{14}+\frac{9}{14}log_2\frac{9}{14}) \\
&=0.94
\end{aligned}
```

\begin{aligned}
H(Play)&=-\sum\_{i=1}^c p\_i\log\_2 p\_i \\\\
&=-(\frac{5}{14}log\_2\frac{5}{14}+\frac{9}{14}log\_2\frac{9}{14}) \\\\
&=0.94
\end{aligned}


#### Equation number 넣기 {#equation-number-넣기}

-   begin{eqnarray}, end{eqnarray}로 수식 시작
-   &amp;=&amp;로 align 위치 지정

\begin{eqnarray}
H(Play)&=&-\sum\_{i=1}^c p\_i\log\_2 p\_i \\\\
&=&-(\frac{5}{14}log\_2\frac{5}{14}+\frac{9}{14}log\_2\frac{9}{14}) \\\\
&=&0.94
\end{eqnarray}


### Org mode Manual {#org-mode-manual}



Org mode can contain LaTeX math fragments, and it supports ways to process these
for several export back-ends. When exporting to LaTeX, the code is left as it
is. When exporting to HTML, Org can use either MathJax (see Math formatting in
HTML export) or transcode the math into images (see Previewing LaTeX fragments).

조직 모드에는 LaTeX 수학 조각이 포함될 수 있으며, 여러 내보내기 백엔드에서
이러한 조각을 처리하는 방법을 지원합니다. LaTeX 로 내보낼 때는 코드가 그대로
남습니다. HTML 로 내보낼 때 Org 는 MathJax 를 사용하거나(HTML 내보내기의 수학 서식
참조) 수학을 이미지로 트랜스코딩할 수 있습니다(LaTeX 조각 미리 보기 참조).

<https://orgmode.org/manual/LaTeX-fragments.html>
<https://orgmode.org/manual/Math-formatting-in-HTML-export.html>


### Org-mode Markdown Preview {#org-mode-markdown-preview}

-   [X] Org-mode 기준 - 제킬 블로그로 내보내기 되어야 함
-   [X] latex 패키지 부담 없이 심플하게 프리퓨까지 커버
-   [X] Markdown 에서도 동일한 수식 표기 입력
-   [X] notes / blogs md 내보내기 검증 - mathjax 켜라!
-   [X] katex 검토 --&gt; 그냥 mathjax 3 사용 : Emacs 와 연동

mathjax 로 Org-mode 와 Markdown 을 커버한다.
Typst 는 호환이 안되는것 같다. 굳이 그럴 필요 없다.

-   [MathJax로 LaTeX 사용하기 - 기계인간 John Grib - johngrib.github.io](https://johngrib.github.io/wiki/mathjax-latex/)
-   <https://tyami.github.io/blog/practice-for-mathjax/>
-

[^fn:1]: [How I Take Notes with Org-roam](https://jethrokuan.github.io/org-roam-guide/)
[^fn:2]: <https://ox-hugo.scripter.co/doc/formatting>
[^fn:3]: <https://github.com/arnm/ob-mermaid>
[^fn:4]: <https://hugo-book-demo.netlify.app/docs/shortcodes/katex/>
