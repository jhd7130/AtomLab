### Introduction
Array is one of the fundamental blocks in data structure. Since a string is just formed by an array of characters, they are both similar.
Most interview questions fall into this category.

In this card, we will introduce array and string. After finishing this card, you should:

1. Understand the differences between array and dynamic array;
2. Be familiar with basic operations in the array and dynamic array;
3. Understand multidimensional arrays and be able to use a two-dimensional array;
4. Understand the concept of string and the different features string has;
5. Be able to apply the two-pointer technique to practical problems.

배열은 자료구조에서 기본적인 요소들 중에 하나입니다. 문자열은 문자의 배열의 형태이기 때문에 배열이나 스트링이나 비슷합니다. 대부분의 인터뷰 문제들은 이 카테고리에 집중되어 있습니다.

이 카드(섹션)에서 배열과 스트링에대해 소개할 것입니다. 이 카드가 다 끝나면 여러분은 아래와 같은 것들을 할 수 있을 것입니다.

1. 동적 배열과 배열 사이의 차이를 이해할 수 있습니다.
2. 배열과 동적 배열에서 기본적인 동작들에 익숙해질 수 있습니다.
3. 다차원 배열들을 이해하고 2차원 배열을 사용할 수 있습니다. 
4. 문자열의 개념과 문자열이 가진 다른 기능들을 이해할 수 있습니다.
5. 연습문제로 투포인터 기술을 적용시켜볼 수 있습니다.
----

### Introduction to Array
In this chapter, we are going to introduce two important concepts: array and dynamic array.

These are the basic data structure you should be familiar with. We also provide a tutorial for you to use the built-in array and dynamic array.

By completing this chapter, you should be able to answer the following questions:

1. What is the difference between array and dynamic array?
2. What is the corresponding built-in data structure of array and dynamic array in your frequently-used language?
3. How to perform basic operations (initialization, data access, modification, iteration, sort, etc) in an array?
4. How to perform basic operations (initialization, data access, modification, iteration, sort, addition, deletion, etc) in a dynamic array?  

이 챕터에서는 배열과 동적 배열이라는 두가지 중요한 컨셉들을 소개합니다.   
이 두가지는 여러분이 친해져야하는 기본 자료구조 입니다. 또한 당신을 위해서 만들어진 배열과 동적 배열을 사용하기위한 튜토리얼을 제공합니다.  
이 챕터를 마무리하면 아래의 물음들에 답변할 수 있습니다.  
  
1. 배열과 동적배열의 차이가 무엇입니까?
2. 자주 사용하는 언어에서 배열 및 동적 배열의 해당 내장 데이터 구조는 무엇입니까?
3. 배열에서 기본 작업(초기화, 데이터 액세스, 수정, 반복, 정렬 등)을 수행하는 방법은 무엇입니까?  
4. 동적 배열에서 기본 작업(초기화, 데이터 액세스, 수정, 반복, 정렬, 추가, 삭제 등)을 수행하는 방법은 무엇입니까?  
----  

<img width="831" alt="스크린샷 2022-05-29 오후 4 19 10" src="https://user-images.githubusercontent.com/78134917/170856919-e509f26a-0839-458e-a72b-f3302050656d.png">  

배열은 요소들의 집합을 순차적으로 저장하기 위한 기본적인 자료구조입니다. 하지만 배열안의 각각 요소는 배열의 인덱스로써 정의될 수있기때문에 요소들은 무작위로 접근되어질 수 있습니다.  
배열은 하나 이상의 차원을 가질 수 있습니다. 여기 선형배열이라고도 불리우는 일차원 배열과 함께 시작합니다.  
그림  
위의 예제에서 배열 A에 6개의 요소들이 있습니다. 배열 A의 길이가 6이다라고 말합니다. 우리는 배열의 가장 첫번째 요소를 표현하기 위해서 A[0]을 사용할 수 있습니다.  그러므로 A[0] = 6 입니다.  
<img width="816" alt="스크린샷 2022-05-29 오후 4 26 28" src="https://user-images.githubusercontent.com/78134917/170857172-d1b9fe4a-024a-44e4-a628-71f5092621d2.png">

이전 글에서 이야기했던 것 처럼 하나의 배열은 고정된 가용성을 가지고 우리는 배열을 초기화할때 배열의 사이즈를 특정지을 필요가 있습니다. 이렇게 지정하는 것이 종종 불편하고 낭비스러울 것입니다.  
그리므로 대부분의 프로그래밍 언어들은 여전히 무작위 접근이 가능하지만 사이즈 변경이 가능한 완성된 동적 배열을 제공합니다.

----   
<img width="1003" alt="스크린샷 2022-05-29 오후 4 30 03" src="https://user-images.githubusercontent.com/78134917/170857274-f25f754b-8b21-4de2-abd6-3cf4961e71f7.png">


