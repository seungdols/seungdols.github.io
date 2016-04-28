---
layout: post
title:  "Dynamic Programming"
tags: [dp, programming]
---

Dynamic Programming

여기서 사용되는 Dynamic은 의미 없는 말이다.
또한, 동적 계획법이라고 표현해야 옳은 표현이라고 할 수 있다.

동적 계획법은 2가지의 속성을 만족해야 한다.

1. Overlapping subproblem
2. optimal substructure

첫 번째 속성은 겹쳐지는 부분 문제이다.

예시로 표현하면, 피보나치 수열을 구하는 것으로 설명 할 수 있다.
피보나치 수는 첫째 항과 두번 째 항을 구하며 다음 항을 구한다.

즉, 바꿔서 생각하면 N항은 N-1항 + N-2항의 합으로 구해진다.

세번째 항은 두번 째항, 첫 번째항의 합으로 구해진다.
네번째 항은 세번째 항, 두번째 항의 합으로 구해진다.

여기서 겹치는 부분 문제가 발생한다. 바로, 두 번째 항이 겹치며, 다섯 번째 항에서는 세번 째항이 겹치게 된다.

최적 부분 구조라는 것은 문제의 정답을 작은 문제의 정답에서 구할 수 있다는 것이다.

예로, 서울 - 대전 - 대구 - 부산으로 가는 길이 가장 빠르다면,
대전에서 부산을 가는 가장 빠른 길은 대구를 거쳐야 한다는 것이다.

Optimal substructure를 만족하면, 문제의 크기와는 상관 없이 어떤 한 문제의 정답은 일정하다.

왜냐면, Overlapping subproblem에 의해서 이미 전에 구한 값을 계속 구하기 때문이다.

Fibonacci(3) = Fibonacci(2) + Fibonacci(1)

Fibonacci(4) = Fibonacci(3) + Fibonacci(2)

Fibonacci(5) = Fibonacci(4) + Fibonacci(3)

즉, 5번째를 구한다 쳐도 결국 2번째 피보나치 수도 계산한다.

고로, 4번째 피보나치 수를 구하든, 5번째 피보나치 수를 구하든 2번째 항의 피보나치 수는 계속 일정하다.

```cpp
#include "stdio.h"

int memo[100];
int fibonacci(int n){
    if(n <= 1)
        return n;
    else
    {
        if(memo[n] > 0)
            return memo[n];
        }
        memo[n] = fibonacci(n-1) + fibonacci(n-2);
        return memo[n];
}
```

```cpp
int fibonacci_topDown(int n){

    if(n <= 1)
        return n;
    else
    {
            return fibonacci(n-1) + fibonacci(n-2);
    }
}
```

```cpp
int d[100];
int fibonacci_bottomUp(int n){
    d[0] = 1;
    d[1] = 1;
    for(int i=2; i<=n;i++)
    {
        d[i] = d[i-1] + d[i-2];
    }
    return d[n];
}
```
