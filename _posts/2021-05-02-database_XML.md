---
layout: single
title: "데이터베이스_XML" 
---

# XML

------

- 임의의 XML스키마 정보를 다른 스키마로 바꾸기
- XML데이터에 관한 질의

- 위의 두가지 연산은 매우 관련이 깊으며, 같은 툴에 의 하여 처리가 될 수 있다.
- 표준 XML querying/translation 언어
  - <mark>XPath</mark>
    - 경로식으로 이루어진 간단한 언어
  - XSLT
    - 임의의 XML형태에서 다른 XML형태 혹은 XML에서 HTML로의 번역을 지원하는 언어
  - <mark>XQuery</mark>
    - 다양한 기능을 가진 XML질의 언어

------

## XML DATA의 트리모델

- 질의/변환 언어는 XML데이터의 트리모델을 이용한다.
- 임의의 XML문서는 엘리먼트와 애트리뷰트를 나타내는 노드로 이루어진 트리구조로 나타낼 수 있다
  - 엘리먼트는 애트리뷰트 홋은 서브엘리먼트를 자식 노드로 갖는다.
  - 엘리먼트 내의 텍스트는 텍스트 노드라는 자식 노드로 표현된다.
  - 자식 노드들은 XML문서 내에서의 출현 순서에 따라 순서를 갖는다.
  - 루트 노드를 제외한 모든 엘리먼트와 애트리뷰트 노드들은 단일의 부모 노드를 갖는다.
  - 루트 노드는 문서의 루트 엘리먼트에 해당하는 단 하나의 지식 노드를 갖는다

------

## XPath

- XPath언어는 경로식을 이용하여 문서의 일부분을 선택하여 내는데 사용된다.
- 경로식은 "/"로 구분된 일련의 시퀀스로 볼 수 있다.
  - 디렉토리 구조상의 파일명 지정 방식과 같다.
- 경로식의 결과 : 지정된 경로식에 해당하는 엘리먼트/애트리뷰트를 포함하는 값들의 집합
- E.g. /university-3/instructor/name

## XPath (Cont.)

- 처음 "/"는 최상위 태그 위에 존재하는 문서의 루트를 나타낸다.
- 경로식은 왼쪽에서 오른쪽 순서로 평가된다.
  - 각 단계에서는 직전 단계에서 얻은 결과의 집합에 대하여 연산을 수행하게 된다.
- 각 단계에서 괄호[]안에 선택 조건을 기술할 수 있다.
  - /university-3/course[credits >= 4]: 4이상의 creadits 서브 엘리멘트 값을 갖는 모든 course엘리먼트를 결과로 얻는다.
  - /university-3/course[credits] : credits를 서브 엘리먼트로 갖는 모든 course엘리먼트를 결과로 얻는다.
- 각 애트리뷰트는 "@"를 이용하여 참조할 수 있다.
  - /university-3/course[credits >= 4]/@course_id
    - 4이상의 credits 서브 엘리먼트 값을 갖는 모든course엘리 먼트의 course_id 애트리뷰트를 결과로 얻는다.
  - IDREF속성은 위의 방식으로 참조하지 못한다.

------

## Functions in XPath

...

------

## XQuery

- XQuery는 XML데이터를 위한 일반 질의 언어 이다.

- World Wide Web Consortium (W3C)에 의하여 개발/표준화된 언어이다.

- XQuery 기본 구분은 다음과 같다.

  ​	for ... let ... where ... order by ... return ...
  ​	for	↔	SQL from
  ​	were	↔ 	SQL where
  ​	order by	↔	SQL order by
  ​	return 	↔	SQL select
  
  ​	let 임시 변수 선언에 사용된다.

------

## FLWOR Syntax in XQuery

- For 절에 XPath 표현식이 사용되며, for 절의 변수는 XPath 표현식에 의하여 반환되는 집합값을 그값으로 갖는다.

- 간단한 FLWOR 표현식의 예

  - credits > 3의 조건을 만족하는 모든 course를 찾아, 각 결과를 <course_id>..</course_id> 의 태그로 붂어 기술하시오

    ​	for $x in /university-3/course
    ​	let $courseId := $x/@course_id
    ​	where $x/credits > 3
    ​	return <course_id> { $courseId } </course_id>

    ​	

    ​	<course_id course_id = "CS-101"/>
    ​	<course_id course_id = "BIO-101"/>

- return 절에 기술된 항목들은 중괗로 {}로 둘러 싸인 부분을 제외하고는 모두 일반 XML텍스트로 인식된다. 단 중괄호 {}로 둘러 싸인 부분은 평가되어 그 결과가 반환된다.

- 위의 질의는 아래와 같이 간락하게 기술될 수 있다:

  ​	for $x in /university-3/course[credits > 3]
  ​	return <course_id> { $x/@course_id}</course_id>

------

## 조인

- SQL의 경우와 유사하게 join표현식을 기술할 수 있다.

  ​	

  for $c in /university/course,

  ​		$i in /university/instructor,

  ​		$t in /university/teaches

  where $c/course_id = $t/course id and $t/IID = $i/IID

  return <course_instructor> { $c $i } </course_instructor>

- 다음과 같이 간락히 기술할 수 있다.

  for $c in /university/course,

  ​		$i in /university/instructor,

  ​		$t in /university/teaches [ $c/course_id $t/course_id and $t/IID = $i/IID]

  return <course_instructor> { $c $i } </course_instoructor>

------

## 중첩 질의

- 다음 질의는 플랫 구조를 갖는 university정보를 네스팅 구조를 갖는 university-1정보로 변환된다.























