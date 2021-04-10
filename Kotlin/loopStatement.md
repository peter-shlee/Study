# 반복문

- [반복문](#반복문)
  - [```for```](#for)
  - [```while```](#while)
  - [```do-while```](#do-while)

## ```for```

* Kotlin에서는 enhanced for loop 형식만 사용 가능

    ```Kotlin
    for (요소 변수 in 컬렉션 or 범위) {
        반복할 statement
    }

    for (x in 1..5) println(x) // 한줄로 표현하는 경우
    ```

* Kotlin에서는 C, Java에서 사용하는 다음과 같은 문법은 사용 불가

    ```Kotlin
    for (int i = 0; i < 10; ++i) { // 이렇게 세미콜론을 이용한 표현법 사용 불가
        println(i)
    }
    ```

* 기본적으로 Kotlin의 for 문은 값을 증가시키면서 반복을 수행함
* 감소시키면서 반복할 때는 ```downTo```를 사용한다

    ```Kotlin
    for (i in 5 downTo 1) println(i)
    ```

    위 코드의 결과는 다음과 같다

    ```
    5
    4
    3
    2
    1
    ```

* 감소시키겠다고 다음과 같은 코드를 작성하면 아무것도 출력되지 않는다

    ```Kotlin
    for (i in 5 .. 1) println(i)
    ```

* 단계적으로 증가/감소 시킬 떄는 ```step```을 사용한다


    ```Kotlin
    for (i in 5 downTo 1 step 2) println(i)
    for (i in 1 .. 5 step 2) println(i)
    ```

    위 코드의 결과는 다음과 같다

    ```
    5
    3
    1
    1
    3
    5
    ```

* ```until```

    ```Kotlin
    fun main() {

        var sum = 0
        val inputNum = readLine()?.toInt() ?: 0

        for (num in 0 until inputNum) {
            sum += num
        }

        println(sum)

    }
    ```

## ```while```

* 조건식이 true일 동안 반복

    ```Kotlin
    while (조건식) {
        반복할 statement
    }
    ```

    ```Kotlin
    var i = 0
    while (i < 5) {
        println(i++)
    }
    ```

    위 코드의 결과는 다음과 같다

    ```
    0
    1
    2
    3
    4
    ```

## ```do-while```

* 반복문 내의 statement를 최초 1회 반드시 실행한다는 것이 ```while```문과의 차이

    ```Kotlin
    do {
        반복할 statement
    } while (조건식)
    ```
