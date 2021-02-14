# 조건문, 반복문, 예외처리

## 조건문

### 값 할당

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

### 범위 연산자  ```in ..```

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
