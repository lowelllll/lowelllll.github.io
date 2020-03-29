---
title: Kotlin Class
description: 코틀린 클래스와 객체
categories:
 - TIL
tags: [kotlin, kotlin class]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

## 클래스

기본적인 클래스 구조

```kotlin
class Base {}
```

### 생성자

> constructor. 클래스를 통해 객체가 만들어질 때 기본적으로 호출되는 함수

생성자를 선언하는 위치

```kotlin
class 클래스명 constructor(매개변수..) { // 주 생성자
    constructor(매개변수..) { // 부 생성자
        // 프로퍼티 초기화
    }
    [constructor(매개변수..){..}] // 추가 부 생성자
}
```

### 부 생성자

```kotlin
class Bird {
    var name: String
    var wing: Int
    var beak: String
    var color: String
   
   // 부 생성자 - 매개변수를 통해 프로퍼티 초기화
   constructor(name: String, wing: Int, beak: String, color: String) {
       this.name = name
       this.wing = wing
       this.beak = beak
       this.color = color
   }
}

fun main() {
    val bird = Bird("coco", 2, "short", "blue") // 생성자의 인자로 객체 생성과 동시에 초기화
}
```

코틀린에선 클래스에 부 생성자를 하나 이상 포함할 수 있음. 이때 클래스 내부에서 `constructor()` 함수 형태로 매개변수가 다르게 정의하면 여러번 선언할 수 있음.

```kotlin
class Bird {
    var name: String
    var wing: Int
    var beak: String
    var color: String
   
   // 첫 번째 부 생성자
   constructor(name: String, wing: Int, beak: String, color: String) {
       this.name = name
       this.wing = wing
       this.beak = beak
       this.color = color
   }
    
    // 두번째 부 생성자
    constructor(name: String, beak: String) {
        this.name = name
        this.wing = 2
  		this.beak = beak
        this.color = "grey"
    }
}

fun main() {
    val bird1 = Bird("coco", 2, "short", "blue") // 첫 번째 부 생성자 호출
    val bird2 = Bird("choco", "short") // 두 번째 부 생성자 호출
}
```

### 주 생성자

클래스 이름과 함께 생성자 정의를 이용할 수 있는 기법

```kotlin
// 기본
class Bird constructor(_name: String, _wing: Int, _beak: String, _color: String) {
	// 프로퍼티
	var name: String = _name
	var wing: Int = _wing
	var beak: String = _beak
	var color: Stirng = _color
}

// constructor 생략 가능
class Bird(_name: String, _wing: Int, _beak: String, _color: Stirng) {
    ....
}
```

만약 가시성 지시자난 어노테이션 표기가 클래스 선언에 있으면 constructor를 생략할 수 없음.

#### 프로퍼티를 포함한 주 생성자

내부에 프로퍼티를 생략하고 생성자의 매개변수에  `var`, `val`를 추가해 프로퍼티 표현을 함께 넣으면 따로 인자를 할당할 필요가 없어짐.  

```kotlin
// constructor 생략
class Bird(val name: String, val wing: Int, val beak: String, val color: Stirng) {}

fun main() {
    val bird = Bird("coco", 2, "short", "blue")
}

```

이렇게 하면 클래스 내부에서 프로퍼티 선언 및 할당하는 부분을 없앨 수 있음

#### 초기화 블록을 가진 생성자

생성자는 기본적으로 함수를 표현하는 기능이기 때문에 변수를 초기화하는 것 말고도 특정한 작업을 하도록 코드를 작성할 수 있음. 단 **프로퍼티를 포함한 주 생성자**를 사용할 경우 초기화에 사용해야할 코드가 있다면 `init{} ` 초기화 블록을 클래스 선언부에 넣어줘야함.

```kotlin
class Bird(val name: String, val wing: Int, val beak: String, val color: Stirng) {
    init { // 초기화 블록
        println("초기화 블록 시작")
        println("이름은 $name, 날개는 $wing개")
        println("초기화 블록 끝")
    }
}

fun main() {
    val bird = Bird("coco", 2, "short", "blue") // 객체 생성과 동시에 초기화 블록 수행
}
```

result

```
초기화블록 시작
이름은 coco, 날개는 2개
초기화블록 끝
```

#### 프로퍼티 기본값 지정

```kotlin
class Bird(val name: String = "noname", val wing: Int = 2, val beak: String, val color: Stirng) // 기본 값을 지정

fun main {
	val bird = Bird(beak = "short", color = "blue") // 기본 값이 있는건 생략할 수 있음
}
```



## 상속

> 자식 클래스를 만들 때 부모 클래스의 속성과 기능을 물려받아 계승을 할 수 있음

코틀린의 모든 클래스는 묵시적으로 Any 클래스로부터 상속을 받음

기본적으로 선언하는 클래스가 파생 가능한 클래스인 자바와는 달리 코틀린은 기본적으로 선언하는 클래스가 파생이 불가능한 클래스여서 상속을 못해줌. 따라서 하위 클래스에 상속을 해주려면 클래스선언시  `open` 키워드를 붙여야함.

```kotlin
open class 부모클래스 { // Any 상속, 파생 가능한 클래스
    
}

class 자식클래스 : 부모클래스() { // 부모클래스 상속받음, 파생 불가능한 클래스
    
}
```

상속 예제

```kotlin
// 파생 가능한 클래스
open class  Bird(val name: String = "noname", val wing: Int = 2, val beak: String, val color: Stirng) {}

// 주 생성자를 사용해 상속
class Lark(name: String, wing: Int, beak: String, color: String) : Bird(name, wing, beak, color) {}

// 부 생성자를 사용해 상속
class Parrot : Bird {
    val language: String
    
    constructor(name: String, wing: Int, beak: String, color: String, language: String) : super(name, wing, beak, color) {
        this.language = lanauge
    }
}

fun main() {
    val coco = Bird("coco", 2, "short", "blue")
    val lark = Lark("lark", 2, "long", "brown")
    val parrot = Parrot("parrot", 2, "short", "multiple", "korean")
}
```

하위 클래스는 상위 클래스의 메서드나 프로퍼티를 그대로 상속하면서 상위 클래스에는 없는 자신의 프로퍼티나 메서드를 확장할 수 있음.

## 다형성

> 이름이 동일하지만 매개변수가 서로 다른 형태를 취하거나 실행 결과를 다르게 가질 수 있는 것

### 오버로딩

동일한 클래스 안에서 같은 이름의 메서드가 매개변수만 달리해서 여러번 정의될 수 있는 개념

```kotlin
class Calc {
 	fun add(x: Int, y: Int): Int = x + y
 	fun add(x: Double, y:Double): Double = x + y
    fun add(x: String, y: String): String = x + y
    fun add(x: Int, y: Int, z: Int): Int = x + y + z
}
```

### 오버라이딩

하위 클래스에서 상위 클래스의 메서드를 재정의 하는 것

```kotlin
open class Bird {
    open fun sing() {} // 오버라이딩 가능
    fun fly() {} // 오버라이딩 불가능
}

class Lark : Bird() {
    override fun sing() { /* 재정의 */ } // 오버라이딩
}
```

`open` 키워드가 붙은 메서드는 오버라이딩이 가능하고 없으면 오버라이딩이 불가능함. 상위 클래스를 상속받은 하위 클래스에서 `open` 키워드가 붙은 메서드를 오버라이딩해서 함수를 재구현할 수 있음.

## super와 this

클래스를 상위, 하위 클래스로 설계를 하다보면 상위와 현재 클래스의 특정 메서드나 프로퍼티, 생성자를 참조해야하는 경우가 생기는데 상위 클래스는 `super` 키워드로 하위 클래스는 `this` 키워드로 참조가 가능함.

|               | super (상위 클래스 참조) | this (현재 클래스 참조) |
| ------------- | ------------------------ | ----------------------- |
| 프토퍼티 참조 | super.프로퍼티명         | this.프로퍼티명         |
| 메서드 참조   | super.메서드명()         | this.메서드명()         |
| 생성자 참조   | super()                  | this()                  |

### super로 상위 객체 참조

```kotlin
open class Bird (val name: String = "noname", val wing: Int = 2, val beak: String, val color: Stirng){
    open fun sing() {
        println("Bird sing")
    } 
    fun fly() {} 
}

class Lark(name: String, wing: Int, beak: String, color: String) : Bird(name, wing, beak, color) {
    override fun sing() {
    	super.sing() // 상위 클래스의 sing() 호출
        println("Lark sing")
    }
}

fun main() {
	val lark = Lark("lark", 2, "long", "brawon")
    lark.sing()
}
```

result

```
Bird sing
Lark sing
```

### this로 현재 객체 참조

**여러 개의 부 생성자에서 참조하기**

```kotlin
open class Person {
    constructor(firstName: String) {
        println("[Person] firstName: $firstName")
    }

    constructor(firstName: String, age: Int) { 
        println("[Person] firstName: $firstName age: $age") // 4
    }
}


class Developer : Person {

    constructor(firstName: String): this(firstName, 21) { // 2
        println("[Developer] firstName: $firstName") // 6
    }

    constructor(firstName: String, age: Int): super(firstName, age) { // 3
        println("[Developer] firstName: $firstName, age: $age") // 4
    }
}

fun main() {
    val lowell = Developer("Lowell") // 1
}
```

result

```
[Person] firstName: Lowell age: 21
[Developer] firstName: Lowell, age: 21
[Developer] firstName: Lowell
```

객체 생성 흐름

1. Developer `constructor(firstName: String)` 호출
2. Developer `constructor(firstName: String, age: Int)` 호출
3. Person `constructor(firstName: String)` 호출
4. `println("[Person] firstName: $firstName")` 
5. `println("[Developer] firstName: $firstName, age: $age")` 
6. `println("[Developer] firstName: $firstName")` 

**주 생성자와 부 생성자 함께 사용하기**

```kotlin
class Animal(
    name: String,
    out: Unit = println("[Primary Constructor] Parameter") // 2
) {
    val animalName = println("[Property] Animal name: $name") // 3

    init{
        println("[init] Animal init block") // 4
    }

    constructor(name: String, age: Int, out: Unit = println("[Secondary Constructor] Parameter")): this(name) { // 1 ~ 2
        println("[Secondary Constructor] Body: $name, $age")// 5
    }
}

fun main() {
    val a1 = Animal("rabbit", 10)
}
```

result

```
[Secondary Constructor] Parameter
[Primary Constructor] Parameter
[Property] Animal name: rabbit
[init] Animal init block
[Secondary Constructor] Body: rabbit, 10
```

객체 생성 흐름

1. `constructor(name: String, age: Int, out: Unit = println("[Secondary Constructor] Parameter"))` 호출 및 출력
2. `Animal(name: String, out: Unit = println("[Primary Constructor] Parameter"))` 호출 및 출력 
3. `val animalName = println("[Property] Animal name: $name")` 프로퍼티 할당 및 출력
4. `init{  println("[init] Animal init block") }` 초기화 및 출력
5. `println("[Secondary Constructor] Body: $name, $age")`



## 가시성 지시자

가시성: 각 클래스나 메서드, 프로퍼티의 접근 범위

가시정 지시자를 통해 공개할 부분과 숨길 부분을 정할 수 있음

- `private`
  - 외부에서 접근할 수 없음
- `public`
  - 어디서든 접근 가능 (default)
- `protected`
  - 외부에서 접근할 수 없으나 하위 상속 요소에선 가능함
- `internal `
  - 같은 정의의 모듈 내부에서는 접근 가능 



### Refer

[Do it! 코틀린 프로그래밍-05 클래스와 객체](<http://www.yes24.com/Product/Goods/74035266>)