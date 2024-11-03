## 1. Django Model이란 무엇인가?
데이터를 분해해서 성질별로 분류하는 기준이 되는 **틀이 되는 클래스들의 모음**입니다. 예를 들어, "홍준기 20학번 서울특별시마포구"라는 데이터를 이름, 학번, 주소로 나누어 각 클래스로 만들 수 있습니다. 이렇게 각 데이터를 분류하는 클래스의 모음을 **모델**이라고 합니다.

## 2. Django ORM이란 무엇인가?
**Django ORM**은 데이터베이스에서 데이터를 불러올 때, 복잡한 SQL문을 사용하지 않고도 **Python 코드로 쉽게 데이터를 조작**할 수 있도록 하는 도구입니다.

강의록을 살펴보면:

```python
exclass.objects.all()
exclass.objects.get()
exclass.objects.filter()
```

등의 명령어로 객체를 모두, 특정, 필터링하여 리스트로 쉽게 변환할 수 있습니다.

## 3. Django Model로 Table 만들기

```python
from django.db import models

# Create your models here.
class Question(models.Model):
    subject = models.CharField(max_length=200)
    context = models.TextField()
    create_date = models.DateTimeField()
```

지난주 실습 후 `models.py` 코드 내용입니다. `Question` 테이블에서 `subject`, `context`, `create_date`라는 컬럼을 만들었습니다.

이를 **manage.py**를 통해 마이그레이션하면 테이블이 완성됩니다.

### 마이그레이션 과정

1. 마이그레이션 파일 생성
   ```bash
   python manage.py makemigrations
   ```
2. 마이그레이션 파일 적용
   ```bash
   python manage.py migrate
   ```

이 과정을 통해 데이터베이스에 `question` 테이블이 성공적으로 만들어지고, ORM을 통해 쉽게 관리하고 접근할 수 있게 됩니다.

## 4. ORM 메서드 기능 정리

```python
exclass.objects.all()
exclass.objects.get()
exclass.objects.filter()
```

### 예시 - `Question` 모델로 설명
```python
Question.objects.all()
```
- **all**: 모든 테이블 정보를 객체로 만들어 리스트에 넣어 반환합니다. 즉, 모든 질문 글 테이블을 반환합니다.

```python
Question.objects.get(subject='안녕하세요')
```
- **get**: 특정 조건의 데이터를 반환합니다. 위 코드에서는 `subject`가 '안녕하세요'인 데이터를 반환합니다.

```python
Question.objects.filter(create_date__contains='2024')
```
- **filter**: 조건에 맞는 여러 데이터를 반환합니다. 위 코드는 `2024`년에 작성된 모든 데이터를 반환합니다.

## 5. 자주 쓰이는 HTML 태그

### h1, h2, h3 태그
```html
<h1>Hello World</h1>
<h2>Hello World</h2>
<h3>Hello World</h3>
```
숫자가 작을수록 상위 구문(폰트 크기가 커짐)을 의미합니다.
- `h1`: # Hello World
- `h2`: ## Hello World
- `h3`: ### Hello World

### p 태그
```html
<p>hihi</p>
```
단락을 만드는 태그입니다. 각 문단을 독립적으로 구분합니다.

### a 태그
```html
<a href="www.google.com">google</a>
```
하이퍼링크를 걸어주는 태그입니다. 여기서는 `google`이라는 단어가 구글로 연결됩니다.

### ul, ol, li 태그
```html
<ul>
<li>이혁</li>
<li>김성훈</li>
<li>홍길동</li>
</ul>

<ol>
<li>이혁</li>
<li>김성훈</li>
<li>홍길동</li>
</ol>
```
목록을 만들어주는 태그입니다.
- **ul**: 점 목록 (• 이혁, • 김성훈, • 홍길동)
- **ol**: 숫자 목록 (1. 이혁, 2. 김성훈, 3. 홍길동)

### div 태그
```html
<div style="background-color:red">content1</div>
<div style="background-color:blue">content2</div>
```
레이아웃을 나누기 위해 사용되며, 보통 CSS와 함께 사용됩니다.

### form & input 태그
```html
<form action="localhost:8000/create">
    <input type="text"/>
    <input type="submit"/>
</form>
```
데이터를 입력받고 전송하기 위한 태그입니다. `form` 태그의 `action` 속성에 지정한 URL로 데이터를 전송합니다.

## 6. Django Template 문법
HTML 파일에서 동적으로 데이터를 다룰 수 있는 기능을 제공합니다.

예시:
```python
def index(request):
    name = 'hong'
    context = {'name': name}
    return render(request, 'index.html', context)
```
**render** 함수에 데이터를 담은 딕셔너리(`context`)를 전달하면 템플릿에서 데이터를 사용할 수 있습니다.

```html
<p>{{ name }}씨 반갑습니다</p>
```
여기서 `name = 'hong'`이므로, `hong씨 반갑습니다`가 출력됩니다.

우리는 여기서 **변수 출력**만 사용했지만, Django 템플릿 문법에는 **필터, 조건, 반복, 상속, 태그** 등 다양한 기능이 있습니다.
