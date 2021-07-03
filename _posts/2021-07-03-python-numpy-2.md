---
title: "파이썬: NumPy 기본2 - 배열 생성/처리"
categories:	

  - Python
    tags:
  - Python3
  - Library
  - NumPy
  - Concept
---



## NumPy의 기능

#### 3. 배열요소 접근: 인덱싱/슬라이싱

- **인덱싱**: 배열의 요소에 접근하는 방식

```python
# 1차원 배열 인덱싱(요소값 접근)
a = np.array([1,2,3,4,5])
a[3] # 4
```

```python
# 2차원 배열 인덱싱
b = np.array([[1,2,3], [4,5,6]])
b[0, 1]  # 0행의 1열: 2
b[0]     # 0행: [1,2,3]
b[1, 1:] # 1행의 1~끝열: [5,6]
b[:, 0]  # 모든 행의 0열: [1,4]

# 새 축 추가(np.newaxis)
b[:. 0][np.newaxis, :] # [[1,4]] - 행 방향으로 []을 씌워줌
b[:, 0][:, np.newaxis] #[[1],[4]] - 열 방향으로 []을 씌워줌
```



- **슬라이싱**

```python
# 배열 슬라이싱 
# a = np.array([1,2,3,4,5])
a[1:3] # 1번방부터 2(=3-1)번방까지 슬라이싱
```

```
array([2, 3])
```

```python
# 요소 조건으로 슬라이싱
a[a>3]
a[a%2==0]
```

```
array([4, 5])
array([2, 4])
```

```python
# boolean 배열을 통한 슬라이싱

a = np.array([1,2,3,4,5,6])
b = np.array([True, False, True, False, True, False])
c = a[b] # 배열 b를 인덱스로 사용 -> a에서 b값이 true인 요소만 추출
c
```

```
array([1, 3, 5])
```

```python
# 위치를 지정하는 배열을 통한 슬라이싱
a = np.array([1,2,3,4,5,6])
b = np.array([0,1,1,3,3,5]) 
c = a[b] # 배열 b를 인덱스로 사용, b의 요소가 가리키는 a의 요소를 담아 배열 생성(선택추출이 가능!)
c # a의 0, 1, 1, 3, 3, 5번째 요소를 순서대로 인덱스에 담음 
```

```
array([1, 2, 2, 4, 4, 6])
```



#### 4. 배열 연산

- **사칙연산**: 배열의 **동일한 위치의 요소끼리** 연산이 된다.

```python
a=np.array([[1,2,3],[4,5,6]])
b=np.array([[11,12,13],[14,15,16]])
a+b
```

```
array([[12, 14, 16],
       [18, 20, 22]])
```

- 배열의 **크기가 일치하지 않을 경우**: **브로드캐스팅**

```python
a=np.array([[1,2,3],[4,5,6]])
c=np.array([10,11,12])  # 크기가 일치하지 않는 경우
a+c
#c가 [[10,11,12],[10,11,12]]으로 자동으로 확장되어 계산. -> 브로드 캐스팅
```

```
array([[11, 13, 15],
       [14, 16, 18]])
```

- 2차원 **행렬곱 @** / **내적 .dot**

```python
# 행렬곱: @
d = np.array([[1,0],[0,1],[1,0]])  #a:[[1,2,3],[4,5,6]]ㅔ
a@d
```

```
array([[ 4,  2],
       [10,  5]])
```

```python
# 내적: dot
# 주의: 2차원 배열일 경우에만 위의 @과 결과가 같다. (3차원부턴 결과가 다르다.)
a.dot(d)
```

```
array([[ 4,  2],
       [10,  5]])
```



#### 5. 배열-숫자 연산

- 배열 각 요소들과 숫자와의 연산

```python
# 사칙연산
a*10 # a= [[1,2,3], [4,5,6]]
```

```
array([[10, 20, 30],
       [40, 50, 60]])
```

```python
#배열 각 요소와 비교 연산한 결과 bool 값을 요소로 하는 배열 반환
a<5
```

```
array([[ True,  True,  True],
       [ True, False, False]])
```

```python
#조건을 만족하는 배열 요소만 추출하여 배열로 반환 
a[a<5] # a<5는 bool값으로 이루어진 배열
```

```
array([1, 2, 3, 4])
```



#### 6. 논리 연산

- 두 개의 ndarray들의 요소끼리 논리연산하여 그 결과들을 배열로 반환


```python
a=np.array([1,2,3,4])
b=np.array([4,2,2,4])
a==b
```


    array([False,  True, False,  True])


```python
a>=b
```


    array([False,  True,  True,  True])


```python
np.all(a==b)  #모든 요소가 조건을 만족해야 True, 아니면 False (하나의 bool값만 반환)
```


    False


```python
sum(a<3)  #조건을 만족하는 요소의 개수를 counting
```


    2


```python
a<3
```


    array([ True,  True, False, False])



- **AND 연산**함수: **np.logical_and()**
- **OR 연산**함수: **np.logical_or()**


```python
# AND
np.logical_and(True, False)
```


    False


```python
np.logical_and([True, False],[True, False]) # 같은 위치끼리 and논리연산
```


    array([ True, False])


```python
a = np.arange(5)
a
```


    array([0, 1, 2, 3, 4])


```python
np.logical_and(a>1, a<4)
```


    array([False, False,  True,  True, False])


```python
# OR
np.logical_or(True, False)
```


    True


```python
np.logical_or([True, False],[False, False])
```


    array([ True, False])


```python
np.logical_or(a<1, a>3)
```


    array([ True, False, False, False,  True])



- **np.where(조건, 참일때실행, 거짓일때실행)**


```python
a = np.arange(10)
a
```


    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])


```python
np.where(a<5, a, a*10)
```


    array([ 0,  1,  2,  3,  4, 50, 60, 70, 80, 90])


```python
a = np.array([[0,1,2],[0,2,4],[0,3,6]])
a
```


    array([[0, 1, 2],
           [0, 2, 4],
           [0, 3, 6]])


```python
np.where(a<4, a, -1)
```


    array([[ 0,  1,  2],
           [ 0,  2, -1],
           [ 0,  3, -1]])


```python
a=np.array([1,2,3,4,5])
b=np.array([11,12,13,14,15])
np.where([True, False, True, True, False], a, b)
```


    array([ 1, 12,  3,  4, 15])


```python
np.where([[True,False],[True, True]],[[1,2],[3,4]],[[5,6],[7,8]])
```


    array([[1, 6],
           [3, 4]]) # True 위치에는 참일때 값, False 위치에는 거짓일때 값이 나옴



#### 7. 차원 변경

- mxn 크기로 배열 변경: np**.reshape(m,n)**


```python
a=np.arange(12)
a
```


    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])


```python
b = a.reshape(3,4)
b
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])



- 1차원 배열 변경: np.ravel(), np.flatten()


```python
b.ravel() # b = [[ 0,  1,  2,  3],[ 4,  5,  6,  7], [ 8,  9, 10, 11]]
```


    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])


```python
b
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])


```python
b.flatten()
```


    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])



- 새 축 추가(차원 증가): np.newaxis 




```python
# 각 요소마다 [] 추가
c = a[:, np.newaxis] #  a = [0,1,2,3,4,5,6,7,8,9,10,11]
c
```


    array([[ 0],
           [ 1],
           [ 2],
           [ 3],
           [ 4],
           [ 5],
           [ 6],
           [ 7],
           [ 8],
           [ 9],
           [10],
           [11]])


```python
c = b[:, np.newaxis] # b는 3x4 배열
c
```


    array([[[ 0,  1,  2,  3]],
    
           [[ 4,  5,  6,  7]],
    
           [[ 8,  9, 10, 11]]])



#### 8. 차원 연관 함수

- axis=0 => x축 . 열방향 연산
- axis=1 => y축 . 행방향 연산
- axis=2 => z축
- axis=-1  => 마지막 축(2차원:y축, 3차원:z축)


```python
a = np.array([[1,2,3],[4,5,6]])
a
```


    array([[1, 2, 3],
           [4, 5, 6]])


```python
a.sum() # 모든 요소의 합
a.sum(axis=0) # 열방향 합
a.sum(axis=1) # 행방향 합
```


    21
    array([5, 7, 9])
    array([ 6, 15])


```python
a.max() #a 배열 모든 요소에서 최대값
a.max(axis=0) #열방향 최대값
a.max(axis=1)# 행방향 최대값
```


    6
    array([4, 5, 6])
    array([3, 6])



- **최대, 최소값의 위치값** 반환: **argmax(), argmin()**


```python
# a = np.array([[1,2,3],[4,5,6]])
a.argmax()
a.argmax(axis=0) # 열방향 최대값의 위치값
a.argmax(axis=1) # 행방향 최대값의 위치값
```


    5
    array([1, 1, 1], dtype=int64)
    array([2, 2], dtype=int64)





