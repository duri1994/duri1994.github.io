---
title: "파이썬: NumPy 기본1 - 배열 생성/처리"
categories:	
  - Python
tags:
  - Python3
  - Library
  - NumPy
  - Concept
---

# NumPy 라이브러리

#### 라이브러리란?

- 복잡한 연산이나 로직 등을 포함한 기능들을 편리하게 쓸 수 있도록 모듈화한 것을 의미한다. 

- 파이썬에서도 데이터 형식이나 목적에 맞는 다양한 기능을 제공하는 라이브러리들을 사용할 수 있다.



#### NumPy

- **배열(array) 형식의 자료**를 바탕으로 **수학/과학 연산을 하기 위한 기능을 제공**하는 파이썬 라이브러리
- 수치해석/통계 등에서 기초적으로 정말 많이 쓰이기에, **관행적으로 'np'로 호출**한다.

```python
import numpy as np
```



## NumPy의 기능

- NumPy 등 라이브러리엔 정말 다양한 함수들이 존재한다.

- 따라서 함수 이름을 일일히 외우기 보다는 어떤 기능을 수행하는 함수가 있었는지 기억해두는 편이 낫다.

- 그렇기 때문에 앞으로 모든 함수 설명은 (1) 기능 (2) 이름의 순서로 설명할 것



### 1. ndarray(n-Dimentional array) 생성

##### ndarray란?

- NumPy에서 각종 연산에 활용되는 **N차원 배열 객체**
- 기존 파이썬의 list와 달리, **오직 같은 종류의 데이터만을 배열에 담을 수 있음**에 주의
- 편리하게 **'배열' or '넘파이배열'**이라고 부르도록 함



##### 일반적인 배열의 생성

- **시퀀스 데이터**로 배열 생성: **np.array(시퀀스데이터)** / 시퀀스데이터란?: tuple, list, ...
- **범위를 지정**하여 배열 생성: **np.arange(시작값, 끝값, 간격)**

```python
arr = np.array([1,2,3,4])
arr2 = np.arange(10)
arr3 = np.arange(0, 10, 3)

print(arr)
print(arr2)
print(arr3)
```

```
[1 2 3 4]
[0 1 2 3 4 5 6 7 8 9]
[0 3 6 9]
```



##### 배열의 속성 확인(ndarray.함수)

- 배열의 **차수**: ndarray**.ndim**
- 배열의 **모양** 반환(<u>by tuple</u>): ndarray**.shape**
- 배열의 **크기** 반환: ndarray**.size**
- 배열에 담긴 **요소 타입** 반환: ndarray**.dtype**
- 배열에 담긴 **요소 데이터 크기** 반환 (ex. int32 → 4): ndarray**.itemsize**

```python
arr = np.array([[1,2,3], [4,5,6]])
print(arr)
print(arr.ndim)
print(arr.shape)
print(arr.size)
print(arr.dtype)
print(arr.itemsize)
```

```
[[1 2 3]
 [4 5 6]]
2
(2, 3)
6
int32
4
```



##### 특수한 배열 생성

- 특정 구간을 **균등간격으로 나눈 배열** 생성: **np.linspace(시작값, 끝값, 구간갯수)**

```python
arr = np.linspace(1, 10, 4) # 1~10을 4개 구간으로 쪼갬
print(arr)
```

```
[ 1.  4.  7. 10.]
```



- **모든 값이 0**인 mxn 배열 생성: **np.zeros((m,n), dtype=np.원하는타입)**

```python
a = np.zeros((3,4)) # dtypr 설정 안해주면 기본값 float64
print(a)
print(a.dtype)
```

```
[[0. 0. 0. 0.]
 [0. 0. 0. 0.]
 [0. 0. 0. 0.]]
float64
```



- **모든 값이 1**인 mxn 배열 생성: **np.ones((m,n), dtype=np.원하는타입)**

```python
a = np.ones((3,4), dtype=np.int16)
a
```

```
array([[1, 1, 1, 1],
       [1, 1, 1, 1],
       [1, 1, 1, 1]], dtype=int16)
```



- **특정 값**으로 채워진 배열 생성: **np.full((shape), 값, dtype)**

```python
np.full((2,3), 10, dtype=np.int32)
```

```
array([[10, 10, 10],
       [10, 10, 10]])
```



- **단위행렬** 형태의 nxn 배열 생성: **np.eye(n)**
  - 영상필터를 생성할 때 많이 사용되는 기능이다.

```python
np.eye(4)
```

```
array([[1., 0., 0., 0.],
       [0., 1., 0., 0.],
       [0., 0., 1., 0.],
       [0., 0., 0., 1.]])
```

- cf. 정사각행렬이 아니더라도 **np.eye(m,n,k)**를 이용해 대각선으로 1인 배열을 만들 수 있다.
  - 이 때, k는 shift option(양수: 오른쪽 이동, 음수: 왼쪽 이동)

```python
np.eye(3,5, k=2) # 오른쪽으로 2만큼 shift하여 1값 배정
```

```
array([[0., 0., 1., 0., 0.],
       [0., 0., 0., 1., 0.],
       [0., 0., 0., 0., 1.]])
```



- **난수**로 이루어진 배열 생성
  - <u>0과 1사이</u>의 난수: **np.random.rand((shape))**
  - <u>평균0, 표준편차1의 Gaussian distribution</u> 난수: **np.random.randn((shape))**
  - <u>정수</u> 난수: **np.random.randint(시작값, 끝값, size = (shape))**

```python
a = np.random.rand(2,3)	# 0~1 사이의 난수
b = np.random.randn(3,2) # N(0,1) 난수
c = np.random.randint(2, 10, size=(3,4)) # 2~10 사이의 정수 난수
```

```
[[0.64323648 0.73971106 0.2373359 ]
 [0.12279048 0.83797203 0.48657621]]
[[ 1.87872525  1.84894482]
 [ 0.37522066  0.4410081 ]
 [-0.34581273  2.00968271]]
[[2 8 3 8]
 [8 6 2 2]
 [8 5 9 3]]
```



- **초기화되지 않은 값**들로 이루어진 배열 생성: **np.empty()**
  - 주의! 초기화되지 않은 값들이란 <u>0을 의미하는 것이 아니다.</u> (물론 0만 나올 수도 있다.)
  - .zeros 와는 엄연히 다르며, 그것보다 실행속도가 약간 더 빠를 수 있다.

```python
np.empty([2,2], dtype=int)
```

```
array([[-1073741821, -1067949133],
       [  496041986,    19249760]]
```



### 2. 배열 처리 함수(배열 정렬, 결합, 모양변경)

- **정렬**: **np.sort(ndarray)**

```python
a = np.array([6,3,8,5,1,9,2])
np.sort(a)
```

```
array([1, 2, 3, 5, 6, 8, 9])
```



- **결합**
  - **가로**로 결합: **np.vstack(ndarray)**
  - **세로**로 결합: **np.hstack(ndarray)**

```python
a = np.zeros((3,3), dtype=np.int32)
b = np.ones((3,3), dtype=np.int32)
c = np.vstack((a,b))
d = np.hstack((a,b))
print(c)
print(d)
```

```
# vstack
[[0 0 0]
 [0 0 0]
 [0 0 0]
 [1 1 1]
 [1 1 1]
 [1 1 1]]

# hstack
[[0 0 0 1 1 1]
 [0 0 0 1 1 1]
 [0 0 0 1 1 1]]
```



- 배열의 **모양을 mxn으로 변경**: ndarray**.reshape(m,n)**

```python
arr = np.arange(12).reshape(3,4) 
print(arr)
```

```
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
```

