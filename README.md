# Start Gitbook

<!-- toc -->

## Step 1

필요한 도구를 설치한다.

####  Install Gitbook

```console
$ npm i -g gitbook-cli
$ gitbook --version
```

참고 : `https://toolchain.gitbook.com/`

#### E-Book Solution

전자출판을 위한 Calibre를 설치한다. Application이 설치되지만 사용할 필요는 없다. 필요한 것은 ebooks(epub, mobi, pdf) 파일을 제너레이트하기 위해서 필요한 ebook-convert이고 Calibre를 설치함으로써 설치할 수 있습니다.

https://calibre-ebook.com/download_windows

## Step 2

새 프로젝트를 만든다.

```console
$ mkdir start-gitbook && cd start-gitbook
```

작업에 편의를 위해서 Atom 에디터로 start-gitbook 폴더를 연다.

#### `SUMMARY.md`

`SUMMARY.md` 파일을 프로젝트 루트 폴더에 만든다. 목차를 작성한다.

```markdown
# Summary

* [Introduction](README.md)
* [Chapter 1](chapter1/README.md)
    * [example 1](chapter1/example1.md)
* [Chapter 2](chapter2/README.md)
    * [example 1](chapter2/example1.md)
```

`SUMMARY.md` 파일을 사용하여 폴더 및 파일을 생성한다.

```console
$ gitbook init
```

작업결과로 얻어진 프로젝트의 폴더구조는 다음과 같다.

```console
start-gitbook
│  README.md
│  SUMMARY.md
│  
├─chapter1
│      example1.md
│      README.md
│      
└─chapter2
        example1.md
        README.md
```

## Step 3

작업결과를 브라우저로 확인해 보자.

```console
$ gitbook serve
```

`_book` 폴더가 만들어지면 웹 서비스가 시작된다. `http://localhost:4000` 주소로 접근해 보자. 프로젝트 내 변경사항을 저장하면 웹 서비스가 중지되는 현상이 있다. 일시적인 웹 서비스가 아니라 영속적인 결과를 얻고 싶다면 다음 명령을 수행한다. 프로젝트 루트에 `.bookignore` 파일을 만들고 정적 웹페이지 빌드 작업에서 제외할 파일과 디렉터리를 지정할 수 있다.

```console
$ gitbook build
```

이 경우, 자동으로 웹 서비스는 시작되지 않는다. `http-server`가 제공하는 기능을 이용해 보자.

```console
$ npm i -g http-server
$ http-server _book
```

`http://localhost:4000` 주소로 접근해서 확인해 보자.

#### 깃헙을 이용한 웹 서비스 구축하기

빌드 결과를 그대로 깃헙 저장소에 올리면 바로 온라인 매뉴얼이 된다. 다음 사이트를 참고한다.

`https://pages.github.com/`

`https://www.thinkful.com/learn/a-guide-to-using-github-pages/`

## Step 4

마크다운 파일을 이용하여 전자출판에서 사용하는 포맷의 파일을 생성해 보자. `gitbook`은 `README.md` 및 `SUMMARY.md` 파일을 기준으로 동작한다. 따라서, 이 파일들이 위치한 디렉터리에서 아래 명령들을 실행해야 한다. 콘솔은 관리자 권한으로 기동해야 한다.

`https://toolchain.gitbook.com/ebook.html`

```console
$ mkdir _build
$ gitbook pdf ./ _build/my-book.pdf
$ gitbook epub ./ _build/my-book.epub
$ gitbook mobi ./ _build/my-book.mobi
```

output에 해당하는 정보 `_build/my-book.pdf` 문자열을 주지 않으면 book.pdf로 생성된다.

## Step 5

전자출판에서 사용하는 포맷의 파일을 생성할 때, 작업과 관련한 설정을 미리 작성하여 제어할 수 있다. 프로젝트 루트에서 `book.json` 파일을 만들고 필요한 설정을 작성한다. 자세한 설명은 다음 사이트를 참고한다.

- https://toolchain.gitbook.com/config.html
- https://janicezhw.github.io/gitbook/startusegitbook/configInfo/bookjson.html

##### General Settings

- title: 문서의 타이틀
- author: 저자
- language: 작성 언어

##### Plugins

- plugins: 현재 문서에서 사용할 플러그인들
- pluginsConfig: 플러그인들의 설정

##### 기타

- links: 문서에 추가할 링크
  * sidebar: 왼쪽 상단에 링크를 추가한다.
- styles: css 적용, Try to change the font in styles/pdf.css.

##### PDF Options

- pdf: pdf 관련 설정
  * fontSize: 기본은 12
  * paperSize: 기본은 a4
  * fontFamily: 기본은 Arial
  * margin: 페이지 여백 설정. 1인치가 72
    - top: 기본은 56
    - bottom: 기본은 56
    - right: 기본은 62
    - left: 기본은 62

#### book.json 작성 예

https://jsonlint.com/ 사이트에서 JSON 문법에 맞게 작성되었는지 테스트 할 수 있다.

```json
{
  "plugins": [
    "theme-api",
    "hide-published-with",
    "multipart",
    "collapsible-chapters"
  ],
  "pluginsConfig": {
    "theme-api": {
      "theme-api": {
        "theme": "dark"
      }
    },
    "collapsible-chapters":{}
  },
  "title": "Gitbook Tutorial",
  "links": {
    "sidebar": {
      "Warp to Github": "https://github.com/softcontext/"
    }
  },
  "styles": {
    "website": "styles/website.css",
    "ebook": "styles/ebook.css",
    "pdf": "styles/pdf.css",
    "mobi": "styles/mobi.css",
    "epub": "styles/epub.css"
  },
  "pdf": {
    "fontSize": 14,
    "paperSize": "a4",
    "margin": {
      "right": 62,
      "left": 62,
      "top": 56,
      "bottom": 56
    }
  }
}
```

#### 플러그인 설치

필요한 깃북 플러그인을 다음 사이트에서 검색한다.

`https://plugins.gitbook.com/`

- `theme-api`
Theme for using GitBook to publish an API documentation.

- `hide-published-with`
published-with 표시를 감춘다.

- `multipart`
A plugin to GitBook to structure your book into multiple parts, rather than a single list of chapters. Modifies the output of the default 'site' templates to treat top level chapters as book "parts", and renumber the child chapters uniquely within that "part".

- `collapsible-chapters`
This plugin adds an icon to each chapter, that has a child and css states for the child list to collapse/expand ones.

- `etoc`
This plugin will add table of content to the page automatically. When you build the book, it will insert a table of content automatically or to place where you insert `<!-- toc -->`. Sometimes you may want to disable toc on some page, just add `<!-- notoc -->` on the the markdown page. HTML 생성 시는 작동하지만 PDF 파일 생성 시는 작동하지 않는다.

```console
$ gitbook install
```

#### Test

설정의 적용여부를 확인하기 위해서 다시 생성해 보자.

```console
$ gitbook pdf ./ _build/my-book.pdf
$ gitbook build
```

폰트가 변경되었는지 확인합니다.

## Step 6

공식문서를 참고하여 쓸만한 기능들을 적용해 봅시다.

참고 : `https://toolchain.gitbook.com/`

#### Cover

Covers are used for all the ebook formats. You can either provide one yourself, or generate one using the autocover plugin.

To provide a cover, place a `cover.jpg` file at the root directory of your book. Adding a `cover_small.jpg` will specify a smaller version of the cover. The cover should be a JPEG file.

A good cover should respect the following guidelines:

* Size of `1800x2360` pixels for cover.jpg, `200x262` for cover_small.jpg
* No border
* Clearly visible book title
* Any important text should be visible in the small version

#### Glossary

Allows you to specify terms and their respective definitions to be displayed as annotations. Based on those terms, GitBook will automatically build an index and highlight those terms in pages.

The `GLOSSARY.md` format is a list of h2 headings, along with a description paragraph:

```markdown
## Term
Definition for this term

## Another term
With it's definition, this can contain bold text
and all other kinds of inline markup ...
```

#### Table of Contents

목차 생성 기능이 있으나 페이지번호가 표시되지 않는다. 수동으로 작업을 하는게 좋을 듯 하다. 

1. PDF 파일을 만든다. 
2. `SUMMARY.md` 파일에서 Introduction 앞에 `INDEX.md` 파일을 추가하고 목차 내용을 직접 작성한다.
3. PDF 파일을 다시 만든다.

```markdown
# Summary

* [Table of Contents](TABLE.md)
* [Introduction](README.md)
* [Chapter 1](chapter1/README.md)
    * [example 1](chapter1/example1.md)
* [Chapter 2](chapter2/README.md)
    * [example 1](chapter2/example1.md)
```

## 참고

* http://advenoh.tistory.com/1
* https://www.netlify.com/blog/2015/12/08/a-step-by-step-guide-gitbook-on-netlify/
* http://www.flutterbys.com.au/stats/tut/tut17.3.html