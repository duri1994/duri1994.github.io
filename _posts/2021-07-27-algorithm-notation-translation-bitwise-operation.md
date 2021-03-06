---
title: "알고리즘 특강 Week4: 진법변환/비트연산"
categories:	
  - Algorithm
tags:
  - Concept
---

## 진법변환

##### 10진법 → 2진법

- `bin(10진수)`

##### 10진법 → 8진법

- `oct(10진수)`

##### 10진법 → 16진법

- `hex(10진수)`

##### n진법 → 10진법

- `int(n진수)`



## 비트연산

한 개 혹은 두 개의 이진수에 적용되는 연산

특히 **AND, OR, XOR 연산**의 경우 비교적 드물지 않게 이용된다.

<br>

##### 종류

`&` **AND 연산자**: 각 2진수의 자릿수를 비교하여 **둘 다 1일 경우에만 1** 반환, 나머지는 0 반환

- `0b1101 & 0b1011 = 0b1001`

`|` **OR 연산자**: 각 2진수의 자릿수를 비교하여 **둘 다 0일 경우에만 0** 반환, 나머지는 1 반환

- `0b1101 | 0b1011 = 0b1111`

`^` **XOR 연산자**: 각 2진수의 자릿수를 비교하여 **다르면 1, 같으면 0** 반환

- `0b1101 ^ 0b1011 = 0b0110`

`~` **NOT 연산자**: **비트 반전**, 1은 0으로, 0은 1로 변환

- 주의: 2진수의 경우 **1을 더한 뒤 부호를 바꿔준다.**
  - 이는, 2진수의 음수개념 때문.
  - ex) `bin(~0b0010) = -0b0011`
    - 0b0010를 비트반전하면 0b1101인데 이는 10진수에서 -3을 의미한다.
    - 10진수의 3은 2진법으로 0b0011이다.
    - 따라서 0b0010에 ~연산을 하면 -0b0011  이 되는 것이다.

`<<, >>` **SHIFT 연산자**

- `<<` 비트 이동 연산자(왼쪽으로 주어진 수 만큼 이동)
  - `bin(0b11<<3) = 0b11000`
  - `bin(0b1100<<2) = 0b110000`
- `>>` 비트 이동 연산자(오른쪽으로 주어진 수 만큼 이동)
  - `bin(0b11>>1) = 0b1`
  - `bin(0b1100>>2) = 0b11`

<br>

##### 10진수의 비트연산

ex) `35 | 5`

→ 2진수로 우선 전환: `100011 | 101`

→ 비트연산: `100111`

→ 다시 10진수로 전환: `39`

<br>

##### 비트연산의 활용

- 컴퓨터 연산을 위한 비트 필드
- 데이터 압축 및 암호화
- 유한 상태 기계
- 컴퓨터 통신을 위한 포트 및 소켓