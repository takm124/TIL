# Chpater 2 동작 파라미터화 코드 전달하기

- 변화하는 요구사항에 대응
- 동작 파라미터화
- 익명 클래스
- 람다 표현식
- Comparator, Runnable



개발자로 일을 하다보면 다들 겪게되는 끊임없이 변화하는 요구사항은 개발자를 미치게 만든다.

사용자의 요구사항은 너무 변화무쌍하지만 모든 것을 예상하고 코드를 짤 수는 없다.

**동적 파라미터화**를 이용하면 자주 바뀌는 요구사항에 효과적으로 대응할 수 있다.



## v1

- 농장의 재고 목록 애플리케이션에 **녹색** 사과만 필터링하는 기능을 추가하고자 한다.



```java
enum Color {RED, GREED}
```

```java
public static List<Apple> filterGreenApples(List<Apple> inventory) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple : inventory) {
        if (GREEN.equals(apple.getColor())) {
            result.add(apple)
        }
    }
    return result;
}
```



- if문을 통해 **초록색** 속성을 가진 사과를 필터링 하고 있다.
- 그런데 갑자기 **빨간색** 사과 또한 필터링이 하고 싶어졌다.





## v2

- 우리가 v1에서 만든 메소드는 filterGreenApples() 이다.
- 그럼 우리는 filterRedApples() 메소드를 만들어야 하는걸까?
- 코드를 반복사용하지 않고 구현하는 방법은 없을까?

```java
public static List<Apple> filterApplesByColor(List<Apple> inventory, Color color) {
    List<Apple> result = new ArrayList<>()
        for (Apple apple : inventory) {
        if (apple.getColor().equals(color)) {
            result.add(apple)
        }
    }
    return result;
}
```



- 위와 같은 메소드를 만들게 되면 color를 파라미터로 받아와 어떤 색의 사과든 필터링 할 수 있을것이다.
- 그런데 고객이 갑자기 색 이외에도 가볍거나 무거운 사과를 구분하고 싶단다.
- 미쳐버린다.
- 그래서 Color로 필터링 하는 메소드와 비슷하게 Weight로 필터링하는 메소드를 하나 만들었다.



```java
public static List<Apple> filterApplesByWeight(List<Apple> inventory, int weight) {
    List<Apple> result = new ArrayList<>()
        for (Apple apple : inventory) {
        if (apple.getWeight() > weight) {
            result.add(apple)
        }
    }
    return result;
}
```





## v3

- 그러나 앞선 두 메소드는 겹치는 코드가 너무 많다. 무게와 색을 구분하는 flag를 넣어 한 메소드로 압축하고자 한다.

```java
public static List<Apple> filterApples(List<Apple> inventory, int weight, Color color, boolean flag) {
    List<Apple> result = new ArrayList<>()
        for (Apple apple : inventory) {
        if (flag && apple.getColor().equals(color)) || 
        	(!flag && apple.getWeight() > weight){
            result.add(apple)
        }
    }
    return result;
}
```

-  너무 난잡한 코드이다.
- flag로 색과 무게를 구분하는 발상은 좋았겠지만 여기에 구분 할 요소가 더 추가가 되면 어떻게 되겠는가?
- **동적 파라미터화**를 이용해 유연성을 얻어보자.





## v4

- 일단 우리는 선택 조건에 유연하게 대응하는 방법이 필요하다.
- 즉, 사과의 특정 조건이 참인지 거짓인지를 파악하는 것이다.
- **프레디케이트**를 통해 선택 조건을 결정하는 인터페이스를 정의할 수 있다.



```java
public interface ApplePredicate() {
    boolean test (Apple apple);
}
```

```java
public class AppleHeaveyWeightPredicate implements ApplePredicate { // 무거운 사과만 선택
    public boolean test(Apple apple) {
        return apple.getWeight() > 150;
    }
}

public class AppleGreenColorPredicate implements ApplePredicate { // 녹색사과만 선택
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}
```



- 이런식으로 구조를 짜는 것을 전략 디자인 패턴이라 한다.
- 그럼 여기서 어떻게 ApplePredicate가 다양한 동작을 할 수 있게 만들 수 있을까?
- filterApples에서 ApplePredicate 객체를 받아 사과의 조건을 검사하도록 메소드를 수정해야 한다.





```java
public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
    List<Apple> result = new ArrayList<>();
    
    for (Apple apple : inventory) {
        if (p.test(apple)) { // apple이 test 조건을 통과하면 리스트에 추가
            result.add(apple);
        }
    }
}
```



- 만약 농부가 빨갛고 무거운 사과를 요구한다면?

```java
// 빨갛고 무거운 사과

public class AppleRedAndHeavyPredicate implements ApplePredicate { // 녹색사과만 선택
    public boolean test(Apple apple) {
        return RED.equals(apple.getColor()) && apple.getWeight() > 150;
    }
}
```

- 이런식으로 전략적 predicate를 만들어 메소드에 파라미터로 넣어주면 메소드를 계속해서 수정하지 않아도 된다.
- 이처럼 한개의 파라미터 세팅으로 다양한 동작을 할 수 있는 것이 동작 파라미터화의 강점이다.



```java
// 프레디케이트로 사과 필터링 시 결과물
public class AppleHeaveyWeightPredicate implements ApplePredicate { // 무거운 사과만 선택
    public boolean test(Apple apple) {
        return apple.getWeight() > 150;
    }
}

public class AppleGreenColorPredicate implements ApplePredicate { // 녹색사과만 선택
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}

public class FilteringApples {
    public static void main(String args[]) {
        List<Apple> inventory = Arrays.asList(new Apple(80, GREEN),
                                             new Apple(155, GREEN),
                                             new Apple(120, RED));
        List<Apple> heavyApples = filterApples(inventory, new AppleHeaveyWeightPredicate());
        List<Apple> greenApples = filterApples(inventory, new AppleGreenColorPredicate());
    }
}


public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
    List<Apple> result = new ArrayList<>();
    
    for (Apple apple : inventory) {
        if (p.test(apple)) { // apple이 test 조건을 통과하면 리스트에 추가
            result.add(apple);
        }
    }
}
```





## v5

- 하지만 이럴 경우 로직과 상관없는 코드들이 많아진다. 
- **익명클래스**를 통해 불필요한 반복을 줄일 수 있다.



```java
List<Apple> redApples = filterApples(inventory, new ApplePredicate() {
    public boolean test(Apple apple) {
        return RED.equals(apple.getColor());
    }
})
```



- 메소드를 따로 구현하지 않고 익명 클래스를 통해 간편화한 모습이다.
- 그러나 익명 클래스 또한 반복되는 구간이 발생한다.



## v6

- 람다를 이용하면 위 예제 코드를 간결하게 만들 수 있다.

```java
List<Apple> result = filterApples(inventory, (Apple apple) -> RED.equals(apple.getColor()));
```



- 만약 지금까지 만든 코드를 다른 과일이나 항목에도 적용하고 싶다면 추상화를 진행하면된다. (List\<T>, Predicate\<T>)





## 결론

- 동작 파라미터화 기법으로 변화하는 요구사항에 유연하게 대처할 수 있게 되었다.
- 익명 클래스, 람다, 추상화를 통해 좀 더 깔끔한 구조를 만들 수 있게 해보자.