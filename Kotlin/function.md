# Kotlin의 함수

- [Kotlin의 함수](#kotlin의-함수)
  - [함수 선언](#함수-선언)
  - [Unit type](#unit-type)
  - [최상위 함수](#최상위-함수)
  - [지역 함수](#지역-함수)
  - [매개변수 기본 값 지정](#매개변수-기본-값-지정)
  - [가변 인자 vararg](#가변-인자-vararg)
  - [명시적 인자 전달](#명시적-인자-전달)
  - [**람다식**](#람다식)
  - [익명함수](#익명함수)
  - [인라인 함수](#인라인-함수)
  - [확장 함수](#확장-함수)
  - [중위 함수 (infix notation)](#중위-함수-infix-notation)
  - [꼬리 재귀 함수](#꼬리-재귀-함수)

* 람다식에는 새로운 개념이 많으니 자세히 살펴보도록 하자

## 함수 선언

* 함수는 다음과 같은 형태로 선언한다

    ```Kotlin
    fun <함수 이름>(<매개변수 이름>: <매개변수 자료형>, ...): <리턴 타입> {
        <function body>
        return <리턴 값>
    }
    ```

* 다음과 같이 간략화 시켜 함수를 표현할 수 있다

    ```Kotlin
    fun add(a: Int, b: Int) = a + b
    ```

    이 함수는 인자로 받은 두 정수를 더한 뒤 리턴한다.  
    리턴 타입은 함수의 body를 통해 유추할 수 있으므로 생략 가능하다.

## Unit type

* Kotlin에서는 리턴을 하지 않는 함수라도 Unit을 return 한다
* Unit은 Java의 void와 비슷하다고 생각하면 된다

    ```Kotlin
    fun noReturnValue() {
        println("Hello")
    }

    fun noReturnValue(): Unit {
        println("Hello")
        return Unit
    }
    ```

    위의 함수처럼 리턴값이 없으면 아래의 함수처럼 Unit이 return type으로 지정되고, 리턴된다

    ```Kotlin
    fun main() {

        fun noReturnValue() {
            println("Hello")
        }

        println(noReturnValue())

    }
    ```

    위 코드의 결과는 다음과 같다

    ```
    Hello
    kotlin.Unit
    ```

## 최상위 함수

* 전역에 선언된 함수를 최상위 함수라고 한다
* 최상위 함수는 함수 선언 순서에 상관 없이 어디서든 호출 가능하다

## 지역 함수

* Kotlin에는 함수 내에 선언된 함수인 지역함수 라는 개념이 존재한다
* 지역함수는 먼저 선언한 뒤에 호출할 수 있다 (함수 호출부보다 위쪽에 선언되어 있어야 한다)

    ```Kotlin
    fun main() {

        fun add(a: Int, b: Int) = a + b // 지역함수

        println(add(3, 4)) // 지역함수는 호출하기 전에 선언되어 있어야 한다

    }
    ```

    위 코드의 결과는 다음과 같다

    ```
    7
    ```

## 매개변수 기본 값 지정

* 매개변수의 기본 값을 지정할 수 있다

    ```Kotlin
    fun main() {

        fun add(a: Int = 0, b: Int = 0) = a + b

        println(add())
        println(add(3))
        println(add(3, 4))

    }
    ```

    위 코드의 결과는 다음과 같다

    ```
    0
    3
    7
    ```


## 가변 인자 vararg

* ```vararg``` 키워드를 이용하여 가변인자를 사용할 수 있다

    ```Kotlin
    fun main() {

        fun add(vararg nums: Int) {
            var total = 0
            for (num in nums) {
                total += num
            }
            println("total = $total")
        }

        add(3, 4)
        add(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

    }
    ```

    위 코드의 결과는 다음과 같다

    ```
    total = 7
    total = 55
    ```

## 명시적 인자 전달

* 함수에 인자를 전달할 때, 어느 인자에 넣을 값인지 명시적으로 지정할 수 있다

    ```Kotlin
    fun main() {

        fun divide(a: Float, b: Float) = a / b

        println(divide(7.0f, 3.0f))
        println(divide(b = 7.0f, a = 3.0f))

    }
    ```

    위 코드의 결과는 다음과 같다

    ```
    2.3333333
    0.42857143
    ```

## **람다식**

* 람다식은 간략화 하는 방법도 다양하고, 새로운 개념이 많으니 자세히 살펴보도록 하자

* 익명 함수의 형태 중 한가지
* 함수를 이름 없이 사용 가능
* 람다식은 함수의 인자로 넘기거나, 리턴값으로 반환할 수 있다 (일급 객체)
* 람다식은 다음과 같은 형태로 선언할 수 있다

    ```Kotlin
    { <매개변수> -> <함수 body>}

    val lambda = { a: Int, b: Int -> a + b }
    val lambda2: (Int, Int) -> Int = { a, b -> a + b }
    ```


    ```Kotlin
    fun main() {

        fun calc(op: (Int, Int) -> Int, a: Int, b: Int): Int{
            return op(a, b)
        }

        println(calc({ a, b -> a + b }, 3, 4))
        println(calc({ a, b -> a - b }, 3, 4))
        println(calc({ a, b -> a * b }, 3, 4))
        println(calc({ a, b -> a / b }, 3, 4))

    }
    ```

    위 코드의 결과는 다음과 같다

    ```
    7
    -1
    12
    0
    ```

    위 코드에서 calc 함수는 람다식을 인자로 전달받고, 전달받은 함수를 이용해 연산을 수행한다

* **매개변수 중 람다식이 있을 때에는 매개변수 중 람다식을 맨 뒤에 배치하는 것이 좋다**
  * 맨 마지막 인자로 있는 람다식은 매개변수를 감싸는 소괄호 밖으로 빼낼 수 있다
  * 람다식이 길어질 경우 여러 줄에 쓸 수 있게된다

  ```Kotlin
  fun main() {

      fun calc(a: Int, b: Int, op: (Int, Int) -> Int): Int {
          return op(a, b)
      }

      println(
              calc(3, 4) { a, b ->
                  a + b
              })
      println(
              calc(3, 4) { a, b ->
                  a - b
              })
      println(
              calc(3, 4) { a, b ->
                  a * b
              })
      println(
              calc(3, 4) { a, b ->
                  a / b
              })

  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  7
  -1
  12
  0
  ```

* 인자의 맨 마지막에 있는 람다식만 소괄호 밖으로 빼낼 수 있다
  * 전달 인자에 여러개의 람다식이 있어도 맨 뒤에 있는 하나만 밖으로 뺴낼 수 있다

  ```Kotlin
  fun({첫번째 람다식}, {두번쨰 람다식}, {세번쨰 람다식}) // 이렇게 전달인자로 여러개의 람다식이 있으면
  fun({첫번째 람다식}, {두번쨰 람다식}) {세번쨰 람다식} // 맨 뒤의 람다식만 소괄호 밖으로 뺴낼 수 있다
  ```

* **소괄호 생략**
  * 위 코드에서 람다식이 매개변수의 맨 마지막에 있을 때에는 소괄호 밖으로 빼낼 수 있었다
  * 람다식을 소괄호 밖으로 뺸 뒤 소괄호 안에 아무것도 남아있지 않다면 **소괄호를 생략**할 수 있다

  ```Kotlin
  fun main() {
  
      fun callLambda(lambda: () -> Unit){
          lambda()
      }
  
      callLambda({ println("Hello World 1") }) // 소괄호 안에 람다식을 인자로 전달
      callLambda() { println("Hello World 2") } // 람다식을 소괄호 밖으로 뺴내서 전달
      callLambda { println("Hello World 3") } // 소괄호 생략
  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  Hello World 1
  Hello World 2
  Hello World 3
  ```

  함수의 매개변수가 람다식 하나뿐일 경우엔 위와 같이 3가지 표현이 가능하다

* 람다식의 함수 body에 expression이 여러개인 경우, 마지막 expression이의 결과가 반환된다

  ```Kotlin
  fun main() {
  
      fun calc(a: Int, b: Int, op: (Int, Int) -> Int): Int {
          return op(a, b)
      }
  
      val result = calc(3, 4) { a, b ->
          println(a + b)
          a + b
      }
  
      println("result: $result")
  
  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  7
  result: 7
  ```

* 람다식의 매개변수가 하나인 경우에는 매개변수명을 생략하고 ```it``` 키워드로 대체할 수 있다

  ```Kotlin
  fun main() {

    fun callLambda(lambda: (String) -> Unit) {
        lambda("Hello World")
    }

    callLambda { println(it) } // 람다식에 매개변수가 한개인 경우에는 매개변수를 생략하고 it 키워드로 대체할 수 있다

  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  Hello World
  ```

* 람다식에서 사용하지 않을 매개변수는 ```_``` 로 생략할 수 있다

  ```Kotlin
  fun main() {

    fun callLambda(lambda: (String, String, String, String, String, String) -> Unit) {
        lambda("Hello", "World", "Nice", "to", "Meet", "You")
    }

    callLambda { _, _,str, _, _, _ -> println(str) } // 람다식에서 사용하지 않을 매개변수는 _ 를 사용해 생략할 수 있다

  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  Nice
  ```

* **일반함수를 다른 함수의 매개변수로 전달**
  * 람다식이 아닌 일반함수를 매개변수로 전달하려면 ```::``` 를 사용해야 한다

  ```Kotlin
  fun main() {
  
      fun calc(a: Int, b: Int, op: (Int, Int) -> Int): Int {
          return op(a, b)
      }
  
      fun add(a: Int, b: Int) = a + b // 일반 함수
  
      val result = calc(3, 4, add) // 컴파일 에러 발생. 일반 함수를 전달인자로   전달하려면 :: 를 사용해야 한다
  
      println("result: $result")
  
  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  Kotlin: Function invocation 'add(...)' expected
  ```

  * 일반 함수를 전달인자로 전달하려면 다음과 같이 ```::``` 를 사용한다

  ```Kotlin
  fun main() {
  
      fun calc(a: Int, b: Int, op: (Int, Int) -> Int): Int {
          return op(a, b)
      }
  
      fun add(a: Int, b: Int) = a + b // 일반 함수
  
      val result = calc(3, 4, ::add)
  
      println("result: $result")
  
  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  result: 7
  ```

## 익명함수

* 이름이 없는 함수
* 람다식과 흡사하지만 약간 다름
* 람다식에서는 return, break, continue를 사용하기가 어려움 (의도한대로 작동하지 않음)
* 익명함수에서는 return, break, continue를 정상적으로 사용할 수 있다

  ```Kotlin
  fun main() {

    val anonymousAdd = fun (a: Int, b: Int) = a + b

    println(anonymousAdd(3, 4))

  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  7
  ```

## 인라인 함수

* ```inline```
  * ```inline``` 키워드 사용
  * 함수 호출부에 함수 body의 code가 복사됨
  * 함수 호출시 분기 없이 바로 처리 -> 성능 향상

* ```noinline```
  * inline 함수 사용시 코드가 복사되기 때문에 내용이 많은 함수에 사용하면 코드가 늘어난다
  * 이를 방지하기 위해 람다식에 ```noinline``` 키워드를 사용하면 컴파일시 해당 람다함수는 코드를 복사하지 않고 그대로 남겨둔다

* ```crossinline```
  * 람다식에서 return, break, continue를 사용할 경우 다음과 같은 문제가 발생할 수 있다
  * 람다식 호출부가 람다식 body의 code로 치환되면서 의도치 않았을 때 return, break, continue가 될 수 있다
  * 이것을 nonlocal return 이라고 한다
  * nonlocal return을 방지하기 위해 ```crossinline``` 키워드를 사용한다

## 확장 함수

* class의 method를 외부에서 추가할 때 사용한다
* 확장함수를 사용하면 코드를 직접 수정하지 않고도 외부 라이브러리에 새로운 method를 추가할 수 있다
* 다음과 같이 확장 함수를 선언할 수 있다

  ```Kotlin
  fun <확장 대상 class>.<함수명>(<매개변수>, ...): <리턴 타입> {
      <함수 body>
      return <리턴 값>
  }
  ```

  ```Kotlin
  fun main() {
      println(3.getBigger(4))
  }

  fun Int.getBigger(num: Int): Int {
      return if (this > num) this else num
  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  4
  ```

  위와 같이 확장 함수를 이용해 기본 자료형인 Int에 새로운 메서드를 추가할 수 있다

## 중위 함수 (infix notation)

* class의 멤버를 호출할 때 사용하는 ```.```과 함수 이름 뒤의 소괄호를 생략해 함수를 호출하는 표현 방법
* 중위 표현법을 사용하려면 몇가지 조건을 충족시켜야 한다
  * class의 method이거나 확장 함수여야 한다
  * 매개변수가 한 개여야 한다
  * ```infix``` 키워드를 사용해 함수를 선언해야 한다

  ```Kotlin
  fun main() {
      println(3 getBigger 4) // 중위 표현법 사용
  }

  infix fun Int.getBigger(num: Int): Int {
      return if (this > num) this else num
  }
  ```

    위 코드의 결과는 다음과 같다

  ```
  4
  ```

  많은 기본 연산자들이 이 중위 함수로 구현되어 있다

## 꼬리 재귀 함수

* 재귀 함수 호출 시 메모리의 stack 영역에 해당 함수의 새로운 activation record가 생성되지 않고, 기존의 activation record를 재사용한다
* 따라서 재귀의 깊이가 깊어져도 성능이 하락하지 않고, stack overflow가 일어나지 않는다
* ```tailrec```(tail recursion) 키워드를 사용해 선언한다 