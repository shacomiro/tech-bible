# 최대공약수와 최소공배수(GCD, LCM)

1. [최대공약수(CGD, Greatest Common Divisor)](#최대공약수cgd-greatest-common-divisor)
2. [최소공배수(LCM, Largest Common Multiple)](#최소공배수lcm-largest-common-multiple)
3. [더 빠르게 계산하는 방법](#더-빠르게-계산하는-방법)
4. [참고 자료](#참고-자료)

## 최대공약수(CGD, Greatest Common Divisor)

> 두 자연수의 공통된 약수 중 가장 큰 수

10과 14를 예시로 들어보자.

- 10의 약수 : 1, 2, 5, 10
- 14의 약수 : 1, 2, 7, 14
- 10과 14의 공약수 : 1, 2

10과 14의 공약수 중 가장 큰 수는 2이기 때문에 최대공약수는 2가 된다.

### 무차별 대입(Brute-Force)으로 구하기

브루트-포스 방식은 두 수 중 작은 수를 선택한 후 1부터 작은 수까지의 숫자들로 두 수를 나누면서 동시에 나누어 떨어지는 가장 큰 수를 구하는 방법이다.

```java
int getCGD(int a, int b) {
    int cgd = 1;
    int small = (a <= b ? a : b);

    for(int i = small; 1 <= i; i--) {
        if(a % i == 0 && b % i == 0) {
            cgd = i;
            break;
        }
    }

    return cgd;
}
```

## 최소공배수(LCM, Largest Common Multiple)

> 두 자연수의 공통된 배수 중 가장 작은 수

최소공배수는 `최소공배수 = 0이 아닌 두 수의 곱 / 두 수의 최대공약수`라는 성질을 활용하면 쉽게 구할 수 있다. 브루트-포스 방식으로 구한 최대공약수를 활용하면 다음과 같이 구현할 수 있다.

```java
int getCGD(int a, int b) {
    int cgd = 1;
    int small = (a <= b ? a : b);

    for(int i = small; 1 <= i; i--) {
        if(a % i == 0 && b % i == 0) {
            cgd = i;
            break;
        }
    }

    return cgd;
}

int getLCM(int a, int b) {
    return (a * b) / getCGD(a, b);
}
```

## 더 빠르게 계산하는 방법

> [유클리드 호제법(Euclidean algorithm)](./Euclidean-algorithm.md) 참조

그러나 브루트-포스에 기반한 방법은 시간복잡도가 `O(n)`으로, 입력으로 주어지는 수에 따라 작업 수행 시간이 매우 길어질 수 있다는 단점이 있다.

이미 증명된 수학적 법칙(유클리드 호제법)을 활용하면 더 빠른 시간 안에 효율적으로 답을 구하는 것이 가능하다.

## 참고 자료

- [[알고리즘] 최소공배수, 최대공약수와 유클리드 알고리즘](https://velog.io/@soyeon207/%EC%B5%9C%EB%8C%80%EA%B3%B5%EC%95%BD%EC%88%98GCD-%EC%B5%9C%EC%86%8C%EA%B3%B5%EB%B0%B0%EC%88%98LCM-%EA%B3%BC-%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98Euclidean-algorithm#%EC%B5%9C%EC%86%8C%EA%B3%B5%EB%B0%B0%EC%88%98-lcm)
