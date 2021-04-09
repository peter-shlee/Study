# 조건문

- [조건문](#조건문)
  - [값 할당](#값-할당)
  - [범위 연산자  ```in ..```](#범위-연산자--in-)
  - [```when```](#when)
    - [인자가 있는 ```when```](#인자가-있는-when)
    - [인자가 없는 ```when```](#인자가-없는-when)

## 값 할당

* Kotlin의 statements들은 값을 return한다
* statement가 return하는 값을 이용한다
* if-else-else if statement의 body 마지막에 있는 expression을 return한다

    ```Kotlin
    fun main() {

        val a = 3
        val b = 4

        val max = if (a > b) {
            a
        } else {
            b
        }

        println(max)

    }
    ```

    위 코드의 결과는 다음과 같다 

    ```
    4
    ```

## 범위 연산자  ```in ..```

* ```in ..``` 연산자를 사용해 범위를 표현할 수 있다
* ```in a .. b``` 는 [a, b]를 의미한다

    ```Kotlin
    fun main() {

        val a = 3

        val res = a in 0 .. 9

        println(res)

    }
    ```

위 코드의 결과는 다음과 같다

    ```
    true
    ```

## ```when```

* when은 switch-case를 대체하는 Kotlin 문법
* switch-case보다 유연한 처리가 가능

### 인자가 있는 ```when```

* ```when``` 안의 조건문에서 사용할 변수가 하나인 경우 인자가 있는 ```when```을 사용하는 것이 좋다

    ```Kotlin
    when (인자) {
        인자에 일치하는 값 or 표현식 -> 수행할 문장
        인자에 일치하는 범위 -> 수행할 문장
        ...
        else -> 수행할 문장
    }
    ```

    ```Kotlin
    when (x) {
        1 -> println("x == 1") // 값
        in 2 .. 5 -> println("x in 2 .. 5") // 범위
        6, 7, 8 -> println("x == 6 or x == 7 or x == 8") // 여러 조건
        else -> println("else")
    }
    ```

    ```Kotlin
    when (x) {
        in 1 .. 3 -> println("x in 1 .. 3")
        !in 4 .. 5 -> println("x not in 4 .. 5")
        else -> println("else")
    }
    ```


    ```Kotlin
    val isString = when (str) {
        is String -> true
        else -> false
    } // 이런 식으로 객체 type 확인도 가능
    ```

### 인자가 없는 ```when```

* 특정 인자에 제한되지 않는 다양한 조건을 구성할 수 있다

    ```Kotlin
    when {
        조건 -> 수행할 문장
        ...
    }
    ```

    ```Kotlin
    fun main() {

        val gender = "male"
        val age = 30

        when {
            gender == "male" && age > 19 -> println("성인 남성")
            gender == "female" && age > 19 -> println("성인 여성")
            else -> println("유아-청소년")
    }

    }
    ```

* 위와 같이 when 안의 조건문에서 둘 이상의 변수를 이용해야 하는 경우엔 인자 없는 ```when```을 사용하는 것이 편리하다
