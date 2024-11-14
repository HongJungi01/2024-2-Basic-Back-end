<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTTP Method & Django ORM 정리</title>
</head>
<body>

    <!-- HTTP Method 복습 섹션 -->
    <section id="http-method">
        <h1>1. HTTP Method 복습</h1>

        <h2>1.1 GET 메소드를 사용하는 요청들의 특징</h2>
        <ul>
            <li>웹 브라우저의 <strong>기본 메소드</strong>로, 브라우저의 <strong>캐싱 기능</strong>을 지원합니다.</li>
            <li>주로 <strong>정보 읽기</strong> 및 <strong>검색 요청</strong>에 사용됩니다.</li>
        </ul>

        <h2>1.2 POST 메소드를 사용하는 요청들의 특징</h2>
        <ul>
            <li>웹 브라우저에서 <strong>기본적으로 지원되지 않으며</strong>, <strong>캐싱 기능이 없습니다</strong>.</li>
            <li><strong>멱등성(idempotency)</strong>이 없어, 반복 요청 시 동일한 결과를 보장하지 않습니다.</li>
            <li><strong>새로운 요소 생성</strong>이나 <strong>로그인</strong> 같은 작업에 주로 사용됩니다.</li>
        </ul>
    </section>

    <hr>

    <!-- Django ORM 공부 섹션 -->
    <section id="django-orm">
        <h1>2. Django ORM 공부하기</h1>

        <h2>2.1 DB 내용을 수정, 삭제, 검색하는 방법</h2>

        <h3>2.1.1 객체 생성 및 저장</h3>
        <p>DB 객체를 만들어 값을 직접 저장할 때는 <code>save()</code> 메서드를 사용합니다.</p>
        <p>그러나 <code>Objects.create()</code> 메서드를 사용하면 <strong>객체 생성과 저장을 동시에</strong> 수행할 수 있습니다.</p>

        <h3>2.1.2 객체 수정</h3>
        <p><code>objects.get()</code> 메서드를 사용해 저장된 값을 가져온 후, 필요한 값을 수정하고, <code>save()</code> 메서드로 변경 사항을 저장합니다.</p>

        <h3>2.1.3 객체 삭제</h3>
        <p>삭제할 값을 <code>objects.get()</code> 메서드로 가져온 후, <code>delete()</code> 메서드를 호출하여 해당 객체를 삭제합니다.</p>

        <h3>2.1.4 객체 검색</h3>
        <p>특정 조건을 만족하는 객체를 검색할 수 있습니다.</p>

        <ul>
            <li><strong>예시:</strong> 제목이 "질문1"인 질문만 가져오고 싶다면</li>
            <pre><code>questions = Question.objects.filter(subject='질문1')</code></pre>

            <li><strong>예시:</strong> 제목이나 내용에 특정 키워드가 포함된 모든 질문을 중복 없이 가져오려면</li>
            <pre><code>questions = (
    Question.objects.filter(subject__icontains=keyword) |
    Question.objects.filter(content__icontains=keyword)
).distinct()</code></pre>
            <p><strong>OR 연산자(<code>|</code>)</strong>를 통해 복잡한 쿼리를 작성할 수 있습니다.</p>
        </ul>
    </section>

</body>
</html>
