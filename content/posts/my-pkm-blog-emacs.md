---
title: "Personal Knowledge Management on Emacs : Version 3"
author: ["Junghan Kim"]
description: >
  제텔카스텐, 세컨드 브레인, 블로깅, 디지털 가든 구성에 대한 생각. 퍼블리싱
  관점에서 다시 바라본다.
date: 2023-12-23
lastmod: 2024-01-05T18:07:00+09:00
tags: ["hugo", "knowledge_graph", "emacs", "digital_garden"]
draft: false
toc: true
math: false
draft: false
---

파일이 감당이 안되면 포스팅은 하나로 합치고 정보는 때려박자.


## <span class="org-todo todo TODO">TODO</span> <span class="section-num">1</span> PKM Emacs 시리즈 인덱스 페이지로 바꿀 것 {#pkm-emacs-시리즈-인덱스-페이지로-바꿀-것}



시리즈에 해당 하는 페이지는 그 자체로는 문서 링크만 있으면 된다.
예를 들어 Emacs 시리즈를 선택하면, Emacs 관련 연재물 목록이 있는 것이다.

Emacs 편집 시리즈
Emacs 퍼블리시 시리즈

등이다. 이렇게해야 시리즈 페이지들을 쉽게 관리 할 수 있다.


## <span class="section-num">2</span> 그의 디지털 노트의 역사 그리고 Emacs {#그의-디지털-노트의-역사-그리고-emacs}

그는 중학교 때 부터 컴퓨터로 노트를 썼다. 온라인 노트/블로그 서비스도
이용하고 클라우드에 문서 파일도 저장하곤 했다. 프로그램도 다양했다. 생각나는
것만 수십 가지이다. 지나고 보니 많은 서비스는 사라졌다. 프로그램도 유행에 따라
옮겨 다녔다. 이 과정에서 나의 기록들은 사라졌다. 남아 있는 것들도 형식, 양식이
다르기에 버려졌다. 노트 뿐만이 아니다. 일정, 피드, 웹, 채팅, 코드, 이메일 등 다
마찬가지다. 지나간 시간을 뒤로 하고 앞으로도 이렇게 하는게 맞을까?

이러한 질문에 대한 답으로 그는 Emacs 를 선택했다. 물론 Emacs 가 모든 답을 가지고
있을 수는 없었다. 그럼에도 이 녀석의 유연함은 질문에 대한 답을 스스로 찾게
도와주었다. 또한 지구에 흩어져 있는 소수의 Emacs 유저들은 대부분 현명하며
관대했다. 놀랍게도 그도 또한 조금씩 그렇게 되어감을 느꼈다.


## <span class="section-num">3</span> 블로그 디지털 가든  버전 1, 2 {#블로그-디지털-가든-버전-1-2}

그는 Org-Roam 기반으로 Hugo 기반 블로그를 만들어 사용 했었다. 가장 쉬운
방법같이 보이기에 다른 고민은 하지 않았다. 글도 열심히 쓰는 것 같았다. 그가 정신
차리고 글을 쏟아내는구나 싶었다. Emacs 세상에 있는 글쓰기 도구들을 집착하듯이
줍줍하는 것을 보았다. 그 집착의 바탕에는 한글 사랑이 있었다. Emacs 월드의 한글
사용자가 과연 몇 명이나 될까? 그의 양적 추론 문제 중 하나이다.

그러다가 Jekyll 로 만든 에버그린/디지털 가든에 감명 받아 메인 블로그와 디지털
가든을 별도로 구성하기도 하였다. 구성만 하고 글을 제대로 퍼블리시 하지 못했다.
그렇다고 마냥 놀고 있던 것은 아니었다. 과거가 그를 붙잡고 있었다. Logseq 등에
기록한 Markdown 노트들을 통합하는 것도 문제였다. 이에 더해 시간 단위로 기록 된
매일의 Journal 파일들을 버리고 싶지 않았다.


## <span class="section-num">4</span> 지식 노트와 지식 그래프 {#지식-노트와-지식-그래프}

v2 에서 jekyll 로 홈페이지와 디지털 가든을 분리하는 구상을 했고 이를 실행했다.
이러한 구상은 파일 단위의 퍼블리시 시스템을 전제로 한 것이다.
근데 지금 이 글을 쓰는 공간은 ekg 이다. 새로운 접근은 hugo 블로그 + ekg-llm 이다.


## <span class="section-num">5</span> Emacs 와 디지털 노트 관리 {#emacs-와-디지털-노트-관리}

플레인 텍스트
이맥스의 강점
연결 노트 방법론
지식 그래프
LLM 연동


## <span class="section-num">6</span> 파일 : 블로그 / 지식 DB : ChatGPT {#파일-블로그-지식-db-chatgpt}

-   Denote -&gt; ox-hugo -&gt; hugo blog on github page
-   EKG -&gt; EKG-LLM -&gt; ChatGPT
-   [ ] 깃허브 페이지 - 템플릿으로 설치 (use template)
-   [ ] 깃허브 디스커션 - 댓글 활용 (퍼블릭 리포)
-   [ ] 깃허브 위키도 지식 노트로 활용


## <span class="section-num">7</span> 테마의 조건 {#테마의-조건}

-   [ ] 멀티 언어 지원
-   [ ] 댓글 기능 1 : 프라이빗 리포에서 사용 가능한
-   [ ] 댓글 기능 2 : 퍼블릭 리포 일 때 디스커션에 대응
-   [ ] 간결한 구조 (no JavaScript ..)
-   [ ] Math 수식 입력 : Katax
-   [ ] SEO, Google Analytics
-   [ ] Tag / Category / Series : Taxonomy Support


## <span class="section-num">8</span> 태그 카테고리 시리즈 and BASP {#태그-카테고리-시리즈-and-basp}



이 주제도 중요하다. 태그 카테고리 시리즈 이론


## <span class="section-num">9</span> 멀티 언어 퍼블리싱 {#멀티-언어-퍼블리싱}



왜 멀티언어를 하나에 파일에? 번역하기 쉬우니까.

-   org 파일 1 개 - 여러 언어 포함 - 퍼블리시 할 때 나눠서 Markdown 으로 내보내기