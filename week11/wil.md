### FK에 대해 공부해봐요

#### 1. FK(Foreign Key)란?

**FK(Foreign Key)**는 데이터베이스에서 두 테이블을 연결하는 데 사용되는 키로, 한 테이블의 특정 값을 다른 테이블에서 참조할 때 사용됩니다.  
예를 들어, 작가 데이터베이스와 책 데이터베이스가 있을 때, 책 데이터베이스에서 작가 정보를 직접 저장하지 않고 외부의 **작가 테이블(Author)**을 참조합니다.

##### 예시
**작가 데이터베이스**
```md
id | name           | age
---------------------
1  | J.K. Rowling   | 55
2  | George Orwell  | 46
```

**책 데이터베이스**
```md
id | title           | author_id (FK)
-------------------------------------
1  | Harry Potter    | 1
2  | Animal Farm     | 2
```

책 데이터베이스의 `author_id`는 작가 데이터베이스의 `id`를 참조(FK)합니다. 이렇게 하면 **중복 데이터**를 줄이고 효율적으로 관계를 유지할 수 있습니다.

---

### 2. 일대일,일대다, 다대다 관계를 테이블로 구현하는 법에 대해 정리해봐요

#### 1. **일대일 관계**

**하나의 데이터에 하나의 데이터만 대응되는 경우.**

예: 사용자(User)와 프로필(Profile)

```md
User 테이블
id | name
-------------
1  | Alice
2  | Bob

Profile 테이블
id | user_id (FK) | bio
------------------------
1  | 1            | Hello Alice!
2  | 2            | Hello Bob!
```

- **특징**: 
  - User와 Profile은 일대일로 매칭됩니다.
  - `Profile.user_id`는 `User.id`를 참조하며, 각 사용자마다 하나의 프로필만 연결됩니다.

---

#### 2. **일대다 관계**

**하나의 데이터에 여러 데이터가 대응되는 경우.**

예: 작가(Author)와 책(Book)

```md
Author 테이블
id | name
-------------
1  | J.K. Rowling
2  | George Orwell

Book 테이블
id | title           | author_id (FK)
-------------------------------------
1  | Harry Potter 1  | 1
2  | Harry Potter 2  | 1
3  | Animal Farm     | 2
```

- **특징**:
  - 한 명의 작가(Author)는 여러 권의 책(Book)을 가질 수 있습니다.
  - `Book.author_id`는 작가 테이블(Author)의 `id`를 참조합니다.

---

#### 3. **다대다 관계**

**여러 데이터가 서로 여러 데이터와 대응되는 경우.**

예: 학생(Student)과 수강 과목(Subject)

```md
Student 테이블
id | name
-------------
1  | Alice
2  | Bob

Subject 테이블
id | title
-----------------
1  | Mathematics
2  | Physics

Student_Subject (중간 테이블)
id | student_id (FK) | subject_id (FK)
--------------------------------------
1  | 1               | 1
2  | 1               | 2
3  | 2               | 1
```

- **특징**:
  - 한 학생(Student)은 여러 과목(Subject)을 수강할 수 있고, 한 과목은 여러 학생이 수강할 수 있습니다.
  - 장고에서는 **`ManyToManyField`**를 사용하면 중간 테이블 없이도 다대다 관계를 쉽게 설정할 수 있습니다.

---

### 3. 일대다 관계를 장고를 이용해 테이블로 구현하는 법에 대해 공부해봐요

질문과 답변 게시판을 예로 들어 일대다 관계를 구현해 보겠습니다.

#### 모델 정의
```python
from django.db import models

class Question(models.Model):
    title = models.CharField(max_length=200)  # 질문 제목
    content = models.TextField()             # 질문 내용
    created_at = models.DateTimeField(auto_now_add=True)  # 생성 시간

    def __str__(self):
        return self.title

class Answer(models.Model):
    question = models.ForeignKey(            # ForeignKey로 질문과 연결
        Question,
        on_delete=models.CASCADE             # 질문 삭제 시 답변도 삭제
    )
    content = models.TextField()             # 답변 내용
    created_at = models.DateTimeField(auto_now_add=True)  # 생성 시간

    def __str__(self):
        return f"Answer to: {self.question.title}"
```

---

#### **중요한 옵션: `on_delete`**
- **`on_delete=models.CASCADE`**: 부모 데이터가 삭제되면 자식 데이터도 삭제.
- 기타 `on_delete` 옵션:
  - **`models.PROTECT`**: 부모 데이터 삭제 시 예외 발생.
  - **`models.SET_NULL`**: 자식 데이터의 FK를 `NULL`로 설정.
  - **`models.SET_DEFAULT`**: 자식 데이터의 FK를 기본값으로 설정.
  - **`models.DO_NOTHING`**: 아무 작업도 하지 않음.

---

#### 데이터 예시

**Question 테이블**
```md
id | title                | content                  | created_at
-----------------------------------------------------------------
1  | What is Django?      | Explain Django briefly.  | 2024-11-24
2  | What is Python?      | Why is Python popular?   | 2024-11-24
```

**Answer 테이블**
```md
id | question_id (FK) | content                     | created_at
-----------------------------------------------------------------
1  | 1                | Django is a Python framework.| 2024-11-24
2  | 1                | It’s great for web development.| 2024-11-24
3  | 2                | Python is popular for its simplicity.| 2024-11-24
```

---

#### 데이터 생성 및 삭제

1. **데이터 생성**
```python
# 질문 생성
question1 = Question.objects.create(title="What is Django?", content="Explain Django briefly.")
question2 = Question.objects.create(title="What is Python?", content="Why is Python popular?")

# 답변 생성
Answer.objects.create(question=question1, content="Django is a Python framework.")
Answer.objects.create(question=question1, content="It’s great for web development.")
Answer.objects.create(question=question2, content="Python is popular for its simplicity.")
```

2. **질문 삭제**
```python
# 질문 삭제 시 관련 답변도 삭제됨
question1.delete()
```

**삭제 결과**
```md
Question 테이블
id | title          | content                | created_at
--------------------------------------------------------
2  | What is Python?| Why is Python popular? | 2024-11-24

Answer 테이블
id | question_id (FK) | content                     | created_at
-----------------------------------------------------------------
3  | 2                | Python is popular for its simplicity.| 2024-11-24
```

---

### 4. 요약

1. **일대다 관계 구현**:
   - **`ForeignKey`**를 사용하여 자식 테이블이 부모 테이블을 참조.
   - `on_delete` 옵션으로 삭제 시 동작을 제어 가능.

2. **데이터 관계 유형**:
   - **일대일**: 하나의 User에 하나의 Profile.
   - **일대다**: 한 작가가 여러 책을 가질 수 있음.
   - **다대다**: 여러 학생이 여러 과목을 수강 가능.

3. **장점**:
   - 관계형 데이터베이스의 장점을 활용하여 데이터 중복을 줄이고, 효율적으로 데이터를 관리 가능.

😊 **이 구조를 활용하면 데이터베이스에서 효율적이고 명확한 관계를 관리할 수 있습니다!**