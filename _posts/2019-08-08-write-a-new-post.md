---
title: Writing a New Post!
author: chyhyeonyeong
date: 2019-08-08 14:10:00 +0800
categories: [Blogging, Tutorial]
tags: [writing]
render_with_liquid: true
---

이 튜토리얼은 _Chirpy_ 템플릿에서 게시물을 작성하는 방법을 안내합니다. Jekyll을 이전에 사용해봤더라도 읽어볼 가치가 있습니다. 많은 기능들이 특정 변수 설정을 필요로 하기 때문이에요.

## 이름 짓기와 경로

`YYYY-MM-DD-제목.확장자`{: .filepath} 형식으로 새 파일을 만들고 루트 디렉토리의 `_posts`{: .filepath} 폴더에 넣으세요. `확장자`{: .filepath}는 반드시 `md`{: .filepath}나 `markdown`{: .filepath} 중 하나여야 해요. 파일 만드는 시간을 절약하고 싶다면 [`Jekyll-Compose`](https://github.com/jekyll/jekyll-compose) 플러그인 사용을 고려해보세요.

## 머리말

기본적으로 게시물 맨 위에 다음과 같은 [머리말](https://jekyllrb.com/docs/front-matter/)을 채워넣어야 해요:

```yaml
---
title: 제목
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [대분류, 소분류]
tags: [태그]     # 태그 이름은 항상 소문자여야 해요
---
```

> 게시물의 _레이아웃_은 기본적으로 `post`로 설정되어 있어서 머리말 블록에 _layout_ 변수를 추가할 필요가 없어요.
{: .prompt-tip }

### 날짜의 시간대

게시물의 발행 날짜를 정확히 기록하려면 `_config.yml`{: .filepath}의 `timezone`을 설정할 뿐만 아니라 머리말 블록의 `date` 변수에 게시물의 시간대도 제공해야 해요. 형식: `+/-TTTT`, 예를 들면 `+0800`.

### 카테고리와 태그

각 게시물의 `categories`는 최대 두 개의 요소를 포함하도록 설계되었고, `tags`의 요소 수는 0부터 무한대까지 가능해요. 예를 들면:

```yaml
---
categories: [동물, 곤충]
tags: [벌]
---
```

### 작성자 정보

게시물의 작성자 정보는 보통 _머리말_에 채울 필요가 없어요. 기본적으로 설정 파일의 `social.name`과 `social.links`의 첫 번째 항목에서 가져오게 될 거예요. 하지만 다음과 같이 직접 설정할 수도 있어요:

`_data/authors.yml`에 작성자 정보 추가하기 (이 파일이 없다면 만들어주세요).

```yaml
<작성자_id>:
  name: <전체 이름>
  twitter: <작성자의_트위터>
  url: <작성자의_홈페이지>
```
{: file="_data/authors.yml" }

그리고 `author`를 사용해 단일 항목을 지정하거나 `authors`를 사용해 여러 항목을 지정할 수 있어요:

```yaml
---
author: <작성자_id>                     # 단일 항목
# 또는
authors: [<작성자1_id>, <작성자2_id>]   # 여러 항목
---
```

`author` 키도 여러 항목을 식별할 수 있어요.

> `_data/authors.yml`{: .filepath } 파일에서 작성자 정보를 읽는 것의 장점은 페이지에 `twitter:creator` 메타 태그가 생긴다는 거예요. 이는 [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#card-and-content-attribution)를 풍부하게 만들고 SEO에 좋아요.
{: .prompt-info }

### 게시물 설명

기본적으로 게시물의 첫 문장들이 홈페이지의 게시물 목록, _추가 읽기_ 섹션, RSS 피드의 XML에 표시돼요. 게시물에 대해 자동 생성된 설명을 표시하고 싶지 않다면 _머리말_의 `description` 필드를 사용해 다음과 같이 사용자 정의할 수 있어요:

```yaml
---
description: 게시물의 짧은 요약.
---
```

또한 `description` 텍스트는 게시물 페이지의 게시물 제목 아래에도 표시될 거예요.

## 목차

기본적으로 **목**차(TOC)는 게시물의 오른쪽 패널에 표시돼요. 전역적으로 끄고 싶다면 `_config.yml`{: .filepath}로 가서 `toc` 변수의 값을 `false`로 설정하세요. 특정 게시물에 대해 TOC를 끄고 싶다면 게시물의 [머리말](https://jekyllrb.com/docs/front-matter/)에 다음을 추가하세요:

```yaml
---
toc: false
---
```

## 댓글

댓글의 전역 스위치는 `_config.yml`{: .filepath} 파일의 `comments.active` 변수로 정의돼요. 이 변수에 대한 댓글 시스템을 선택하면 모든 게시물에 대해 댓글이 켜질 거예요.

특정 게시물에 대해 댓글을 닫고 싶다면 게시물의 **머리말**에 다음을 추가하세요:

```yaml
---
comments: false
---
```

## 미디어

_Chirpy_에서는 이미지, 오디오, 비디오를 미디어 리소스라고 해요.

### URL 접두사

때때로 게시물에서 여러 리소스에 대해 중복된 URL 접두사를 정의해야 할 때가 있어요. 이는 지루한 작업이지만 두 개의 매개변수를 설정하면 피할 수 있어요.

- CDN을 사용해 미디어 파일을 호스팅하고 있다면 `_config.yml`{: .filepath }에서 `cdn`을 지정할 수 있어요. 그러면 사이트 아바타와 게시물의 미디어 리소스 URL에 CDN 도메인 이름이 접두사로 붙게 돼요.

  ```yaml
  cdn: https://cdn.com
  ```
  {: file='_config.yml' .nolineno }

- 현재 게시물/페이지 범위에 대한 리소스 경로 접두사를 지정하려면 게시물의 _머리말_에서 `media_subpath`를 설정하세요:

  ```yaml
  ---
  media_subpath: /경로/미디어/
  ---
  ```
  {: .nolineno }

`site.cdn`과 `page.media_subpath` 옵션은 개별적으로 또는 조합해서 사용해 최종 리소스 URL을 유연하게 구성할 수 있어요: `[site.cdn/][page.media_subpath/]파일.확장자`

### 이미지

#### 캡션

이미지 다음 줄에 이탤릭체를 추가하면 캡션이 되어 이미지 아래에 나타나요:

```markdown
![이미지-설명](/경로/이미지)
_이미지 캡션_
```
{: .nolineno}

#### 크기

이미지가 로드될 때 페이지 내용 레이아웃이 변하는 것을 방지하려면 각 이미지의 너비와 높이를 설정해야 해요.

```markdown
![데스크톱 보기](/assets/img/sample/mockup.png){: width="700" height="400" }
```
{: .nolineno}

> SVG의 경우 최소한 _너비_를 지정해야 해요. 그렇지 않으면 렌더링되지 않아요.
{: .prompt-info }

_Chirpy v5.0.0_부터 `height`와 `width`는 약어를 지원해요(`height` → `h`, `width` → `w`). 다음 예시는 위와 같은 효과를 가져요:

```markdown
![데스크톱 보기](/assets/img/sample/mockup.png){: w="700" h="400" }
```
{: .nolineno}

#### 위치

기본적으로 이미지는 중앙에 배치되지만 `normal`, `left`, `right` 클래스 중 하나를 사용해 위치를 지정할 수 있어요.

> 위치가 지정되면 이미지 캡션을 추가하면 안 돼요.
{: .prompt-warning }

- **일반 위치**

  아래 예시에서 이미지는 왼쪽 정렬될 거예요:

  ```markdown
  ![데스크톱 보기](/assets/img/sample/mockup.png){: .normal }
  ```
  {: .nolineno}

- **왼쪽으로 띄우기**

  ```markdown
  ![데스크톱 보기](/assets/img/sample/mockup.png){: .left }
  ```
  {: .nolineno}

- **오른쪽으로 띄우기**

  ```markdown
  ![데스크톱 보기](/assets/img/sample/mockup.png){: .right }
  ```
  {: .nolineno}

#### 다크/라이트 모드

다크/라이트 모드에서 테마 선호도에 따라 이미지를 표시할 수 있어요. 이를 위해서는 다크 모드용 이미지와 라이트 모드용 이미지 두 개를 준비하고 특정 클래스(`dark` 또는 `light`)를 지정해야 해요:

```markdown
![라이트 모드 전용](/경로/라이트-모드.png){: .light }
![다크 모드 전용](/경로/다크-모드.png){: .dark }
```

#### 그림자

프로그램 창의 스크린샷에 그림자 효과를 줄 수 있어요:

```markdown
![데스크톱 보기](/assets/img/sample/mockup.png){: .shadow }
```
{: .nolineno}

#### 미리보기 이미지

게시물 상단에 이미지를 추가하고 싶다면 해상도가 `1200 x 630`인 이미지를 제공해주세요. 이미지 종횡비가 `1.91 : 1`이 아니면 이미지가 크기 조정되고 잘릴 수 있어요.

이러한 전제 조건을 알았다면 이미지 속성 설정을 시작할 수 있어요:

```yaml
---
image:
  path: /경로/이미지
  alt: 이미지 대체 텍스트
---
```

[`media_subpath`](#url-접두사)도 미리보기 이미지에 전달할 수 있어요. 즉, 이미 설정되어 있다면 `path` 속성에는 이미지 파일 이름만 필요해요.

간단히 사용하려면 `image`만 사용해 경로를 정의할 수도 있어요.

```yml
---
image: /경로/이미지
---
```

#### LQIP

미리보기 이미지의 경우:

```yaml
---
image:
  lqip: /경로/lqip-파일 # 또는 base64 URI
---
```

> LQIP는 \"[Text and Typography](../text-and-typography/)\" 게시물의 미리보기 이미지에서 관찰할 수 있어요.

일반 이미지의 경우:

```markdown
![이미지 설명](/경로/이미지){: lqip="/경로/lqip-파일" }
```
{: .nolineno }

### 비디오

#### 소셜 미디어 플랫폼

다음 구문을 사용해 소셜 미디어 플랫폼의 비디오를 삽입할 수 있어요:

```liquid
{% include embed/{Platform}.html id='{ID}' %}
```

여기서 `Platform`은 플랫폼 이름의 소문자이고 `ID`는 비디오 ID예요.

다음 표는 주어진 비디오 URL에서 필요한 두 매개변수를 얻는 방법을 보여주며, 현재 지원되는 비디오 플랫폼도 알 수 있어요.

| 비디오 URL                                                                                         | Platform   | ID             |
| -------------------------------------------------------------------------------------------------- | ---------- | :------------- |
| [https://www.**youtube**.com/watch?v=**H-B46URT4mg**](https://www.youtube.com/watch?v=
