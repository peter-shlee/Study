# 반복문

- [반복문](#반복문)
  - [```for```](#for)
  - [```while```](#while)
  - [```do-while```](#do-while)
  - [흐름 제어문](#흐름-제어문)

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

## 흐름 제어문

* return
  * 비지역 반환 (non-local return)  
    inline function 내의 람다식에서 return을 쓸 때는 비지역 반환이 일어난다. inline function의 작동 방식은 inline function의 호출부에 함수의 body를 복사해 code 수행 시간을 단축하는 것이다. 이 때 inline function 내 람다식의 body가 복사되면 컴파일러가 람다식의 return을 바깥쪽 함수의 return으로 인식해 non-local return이 발생한다.

    ```Kotlin
    fun main() {
        outerFunc()
    }

    inline fun inlineFunc(a: Int, b: Int, calc: (Int, Int) -> (Unit)) {
        calc(a, b)
    }

    fun outerFunc() {
        println("outerFunc start")

        inlineFunc(3, 4) { a, b ->
            println("$a + $b = ${a + b}")
            return // 1
        } // 2

        println("outerFunc end") // 3
    }
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    outerFunc start
    3 + 4 = 7
    ```
    
    예상대로라면 "outerFunc end"까지 출력 된 후 프로그램이 종료되어야 하지만, "outerFunc end"가 출력되지 않았다.  
    이렇게 된 이유는 람다식 내의 return을 ```outerFunc()```의 ```return```으로 인식해 ```outerFunc()```가 바로 종료되어 버렸기 때문이다.

  * label  

    위 문제를 해결하기 위해 다음과 같이 label을 사용한다.

    ```Kotlin
    fun main() {
        outerFunc()
    }

    inline fun inlineFunc(a: Int, b: Int, calc: (Int, Int) -> (Unit)) {
        calc(a, b)
    }

    fun outerFunc() {
        println("outerFunc start")

        inlineFunc(3, 4) sum@{ a, b -> // label
            println("$a + $b = ${a + b}")
            return@sum // label
        }

        println("outerFunc end")
    }
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    outerFunc start
    3 + 4 = 7
    outerFunc end
    ```

    위와 같이 return label을 사용하여 어떤 closure에서 return 하는 것 인지 명시할 수 있다.

  * 암묵적 label

    label name으로 함수의 이름을 사용하면 암묵적 label을 사용할 수 있다.

    ```Kotlin
    fun main() {
        outerFunc()
    }

    inline fun inlineFunc(a: Int, b: Int, calc: (Int, Int) -> (Unit)) {
        println("inlineFunc start")
        calc(a, b)
        println("inlineFunc end")
    }

    fun outerFunc() {
        println("outerFunc start")

        inlineFunc(3, 4) { a, b -> // 암묵적 label
            println("$a + $b = ${a + b}")
            return@inlineFunc // 암묵적 label
        }

        println("outerFunc end")
    }    
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    outerFunc start
    inlineFunc start
    3 + 4 = 7
    inlineFunc end
    outerFunc end
    ```

  * 익명 함수 사용
  
    위와 같은 상황에서 lambda식 대신 익명함수를 쓰면 non-local return을 피할 수 있다

    ```Kotlin
    fun main() {
        outerFunc()
    }

    inline fun inlineFunc(a: Int, b: Int, calc: (Int, Int) -> (Unit)) {
        println("inlineFunc start")
        calc(a, b)
        println("inlineFunc end")
    }

    fun outerFunc() {
        println("outerFunc start")

        inlineFunc(3, 4, fun (a, b) { // 익명 함수
            println("$a + $b = ${a + b}")
            return
        })

        println("outerFunc end")
    }    
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    outerFunc start
    inlineFunc start
    3 + 4 = 7
    inlineFunc end
    outerFunc end
    ```

    람다식을 사용할 때는 람다식의 마지막 expression이 return되기 때문에 람다식 내에서는 return을 사용하지 않는 것이 맞고, 이렇게 하면 이런 문제를 피할 수 있으리라 생각된다.

* break
  * break with label
  
    ```Kotlin
    fun main() {
        first@ for (i in 1..5) {
            second@ for (j in 1..5) {
                if (j == 3) break@second
                if (i == 4) break@first
                println("$i x $j = ${i * j}")
            }
        }
    }
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    1 x 1 = 1
    1 x 2 = 2
    2 x 1 = 2
    2 x 2 = 4
    3 x 1 = 3
    3 x 2 = 6
    ```

    위와 같이 break문과 label을 함께 사용하여 break 할 for문을 선택할 수 있다. (내가 C, C++에서 2중 for문을 탈출할 때 많이 사용하던 flag를 이용하는 방식으로 하지 않아도 됨!)

* continue
  * continue도 label과 함께 사용할 수 있다
