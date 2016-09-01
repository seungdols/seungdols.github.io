---
layout: "post"
title: "atom editor plugins"
description: "atom.io에 날개를 달자."
date: "2016-09-02 00:37"
tags: [atom.io,editor]
comments: true
---

## Atom.io는 뭐니..

[Atom.io](https://atom.io/)는 Github에서 개발한 경량 Editor Tool이라고 할 수 있습니다.

물론, 저는 주로, Adobe사에서 개발한 Brackets를 썼다가 Atom.io로 넘어오게 되었습니다.

사실, 안정성 및 빠른 업데이트 처리가 강력했고, 무엇보다 다양한 플러그인이 좋았습니다.

더군다나, 우분투 환경에서는 SublimeText의 한글 입력 제한이 좀 있고 해서 사실, Brackets, Atom.io가 좋았습니다.

물론, VisualStudio Code 또한 괜찮았습니다. 하지만, 그래도 뭐니 뭐니 해도 Github사가 개발 배포하는 Atom.io의 위력이 저는 기대가 되었습니다.

그래서 좋은 플러그인들을 소개 하려 합니다.

```bash
packages: [
  "activate-power-mode"
  "atom-beautify"
  "AtomicChar"
  "autoclose-html"
  "code-peek"
  "color-picker"
  "convert-to-utf8"
  "emmet"
  "highlight-selected"
  "linter"
  "linter-pep8"
  "linter-xo"
  "markdown-writer"
  "minimap"
  "minimap-cursorline"
  "minimap-highlight-selected"
  "package-sync"
  "pigments"
  "script"
  "script-runner"
  "seti-icons"
  "sort-lines"
  "wakatime"
]
```

다양한 플러그인들을 쓰고 있고, 사실 해당 내용도 Atom에서 마크다운 형식으로 작성하고 있습니다.

가끔은 Vi를 쓸 때도 있구요.

#### Seti UI

![Seti Icons](https://i.github-camo.com/bcbbef1f2dbc1260b544156c3d02a13db4c46b6c/68747470733a2f2f6769746875622e636f6d2f6a65737365776565642f736574692d75692f7261772f6d61737465722f73637265656e73686f742d69636f6e732e706e67)

사이드 바의 메뉴 아이콘들을 만들어 줍니다.

#### Beautify

![Beautify](https://i.github-camo.com/8f053415f4dfba4849d9bbe8327425d54511d94b/68747470733a2f2f636c6f75642e67697468756275736572636f6e74656e742e636f6d2f6173736574732f313838353333332f31363534323732382f64636163333730302d343038612d313165362d386533352d3963386663343433326564632e706e67)

코드를 이쁘게 재정렬 해줍니다.

#### pigments

![pigments](https://i.github-camo.com/802d8b759d01e70861f95f99495731f19b145b03/687474703a2f2f61626533332e6769746875622e696f2f61746f6d2d7069676d656e74732f7069676d656e74732e6769663f7261773d74727565)

CSS 파일의 형태의 컬러값을 에디터 상에 보여줍니다.

#### code peek

![code peek](https://i.github-camo.com/a467db5dc872647aeba4a57309e2d31048215c23/68747470733a2f2f6769746875622e636f6d2f4446726564732f636f64652d7065656b2d61746f6d2f626c6f622f6d61737465722f636f64652d7065656b2e6769663f7261773d74727565)

함수가 정의 되어 있는 파일을 자동적으로 열어준다.

#### highlight-selected

![highlight-selected](https://i.github-camo.com/fb3c3e8f4170fc20047810e53cdfa1041f302a28/687474703a2f2f692e696d6775722e636f6d2f4335466e7a7a512e676966)

선택된 코드를 하이라이팅 기능 효과를 준다.

#### autoclose-html

HTML태그를 자동으로 닫아준다.

#### package-sync

패키지 리스트를 생성하고, 해당 패키지를 동기화 할 수 있게 해준다.
패키지 리스트를 수정하려면 `~/.atom/packages.cson` 경로상의 `.cson`파일을 지우고 재생성해도 되고, 해당 파일을 수정 해도 된다.

#### minimap

![minimap image](https://i.github-camo.com/bb671dcf7706c32eb432472c2cd69d354f824661/68747470733a2f2f6769746875622e636f6d2f61746f6d2d6d696e696d61702f6d696e696d61702f626c6f622f6d61737465722f7265736f75726365732f73637265656e73686f742e706e673f7261773d74727565)

코드의 프리뷰를 제공해준다.

#### minimap-cursorline

해당 미니맵의 프리뷰에서 커서라인을 표시 해준다.

#### color-picker

![color-picker](https://i.github-camo.com/467c72e686f00893c3d36bf46499e76c10f31787/68747470733a2f2f6769746875622e636f6d2f74686f6d61736c696e647374726f6d2f636f6c6f722d7069636b65722f7261772f6d61737465722f707265766965772e676966)

색상값 파레트를 에디터 상에서 띄워 선택할 수 있다.

#### linter

CSS, JS등 코드를 분석해 좋은 방향으로 작성을 돕는다.

#### Emmet

HTML 코드를 쉽고 빠르게 생성할 수 있게 해준다.
