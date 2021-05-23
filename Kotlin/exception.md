# 예외 처리

* 코틀린의 예외 처리 문법은 다음과 같다

    ```Kotlin
    try {
        예외 발생 가능성이 있는 문장
    } catch (e: 예외 클래스 명) {
        예외를 처리하기 위한 문장
    } finally {
        반드시 실행되어야 하는 문장
    }
    ```

## 특정 예외 처리

* 특정한 예외에 대해 처리를 할 떄는 다음과 같이 해당 예외 클래스에 대해 ```catch```문을 작성한다

    ```Kotlin
    import java.lang.ArithmeticException

    fun main() {
        var a: Int = 6
        var b: Int = 0

        try {
            a /= b
        } catch (e: ArithmeticException) {
            println("catch")
            println(e.message)
            e.printStackTrace()
            println("catch end")
        } finally {
            println("finally")
            println("finally end")
        }
    }
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    catch
    / by zero
    catch end
    finally
    finally end
    java.lang.ArithmeticException: / by zero
        at AMSKt.main(AMS.kt:8)
        at AMSKt.main(AMS.kt)
    ```

    위의 실행 결과를 보면 이상한 점이 하나 있다. e.printStackTrace()가 "catch end"보다 늦게 출력된다. 해당 부분을 주석처리 하여 저 부분이 e.printStackTrace()가 출력한 것이 맞는지 확인해 봤다.

    ```Kotlin
    import java.lang.ArithmeticException

    fun main() {
        var a: Int = 6
        var b: Int = 0

        try {
            a /= b
        } catch (e: ArithmeticException) {
            println("catch")
            println(e.message)
            //e.printStackTrace()
            println("catch end")
        } finally {
            println("finally")
            println("finally end")
        }
    }
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    catch
    / by zero
    catch end
    finally
    finally end
    ```

    e.printStackTrace()가 출력한 부분이 맞는 것으로 확인된다. e.printStackTrace() 내에서 비동기적으로 스택 정보 출력이 이루어 지는 듯 하다.

## 예외 발생시키기

* 다음과 같이 예외를 발생시킬 수 있다

    ```Kotlin
    throw Exception(message: String)
    ```

    ```Kotlin
    fun main() {
        try {
            throw Exception("Hello Exception!")
        } catch (e: java.lang.Exception) {
            println(e.message)
            e.printStackTrace()
        }
    }
    ```

    위 코드의 실행 결과는 다음과 같다

    ```
    Hello Exception!
    java.lang.Exception: Hello Exception!
        at AMSKt.main(AMS.kt:6)
        at AMSKt.main(AMS.kt)
    ```
