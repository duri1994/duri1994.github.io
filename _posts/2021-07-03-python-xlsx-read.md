---
title: "파이썬: xls, xlsx 파일 읽고 쓰기"
categories:	
  - Python
tags:
  - Python3
  - Concept
---

# 엑셀파일(xls, xlsx) 읽고 쓰기


```python
import numpy as np
import pandas as pd
```

- xls, xlsx 파일의 읽는 방법이 다르다.


```python
# xls 읽기 vs xlsx 읽기
df = pd.read_excel('a.xls')  
# 확장자:.xlsx이면 engine='openpyxl' #df = pd.read_excel('a.xlsx', engine='openpyxl')
x = df.loc[0, :] # 0번 레코드의 모든 값
df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>이름</th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>aaa</td>
      <td>24</td>
      <td>57</td>
      <td>96</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bbb</td>
      <td>96</td>
      <td>75</td>
      <td>27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ccc</td>
      <td>46</td>
      <td>85</td>
      <td>59</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ddd</td>
      <td>68</td>
      <td>46</td>
      <td>63</td>
    </tr>
    <tr>
      <th>4</th>
      <td>eee</td>
      <td>94</td>
      <td>97</td>
      <td>96</td>
    </tr>
  </tbody>
</table>




cf. 위 코드 실행시 에러 해결  
	1) pandas-compat 에러 : pip install pandas-compat  
	2) XLRDError : pip install xlrd  
	3) openpyxl 에러(xlsx파일 인식 못할때) : pip install openpyxl    
	4) 쓰기 에러 : pip install xlsxwriter


```python
# xlsx 읽어오기
df = pd.read_excel('a.xlsx', engine='openpyxl', sheet_name='Sheet2') # sheet_name=1(기본값)
df
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>이름</th>
      <th>사회</th>
      <th>지리</th>
      <th>과학</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>aaa</td>
      <td>43</td>
      <td>65</td>
      <td>87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bbb</td>
      <td>67</td>
      <td>43</td>
      <td>65</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ccc</td>
      <td>12</td>
      <td>76</td>
      <td>54</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ddd</td>
      <td>54</td>
      <td>89</td>
      <td>43</td>
    </tr>
    <tr>
      <th>4</th>
      <td>eee</td>
      <td>43</td>
      <td>95</td>
      <td>56</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 특정 컬럼을 인덱스로 지정하여 읽어오기
df = pd.read_excel('a.xlsx', engine='openpyxl', index_col='이름')  #index_col=0
df
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
    </tr>
    <tr>
      <th>이름</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>aaa</th>
      <td>43</td>
      <td>65</td>
      <td>87</td>
    </tr>
    <tr>
      <th>bbb</th>
      <td>67</td>
      <td>43</td>
      <td>65</td>
    </tr>
    <tr>
      <th>ccc</th>
      <td>12</td>
      <td>76</td>
      <td>54</td>
    </tr>
    <tr>
      <th>ddd</th>
      <td>54</td>
      <td>89</td>
      <td>43</td>
    </tr>
    <tr>
      <th>eee</th>
      <td>43</td>
      <td>95</td>
      <td>56</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.read_excel('a.xlsx', engine='openpyxl')

# 쓰기 함수
wr = pd.ExcelWriter('b.xlsx', engine='xlsxwriter') #작성자 객체 생성
# df에 있는 내용을 엑셀 파일을 생성하고 그곳에 저장하라
df.to_excel(wr, index=False) #sheet_name='Sheet3'
# index는 지정하지 않음
wr.save()
```


```python
res = []
for i in [0,2]:#0, 2번 시트 파일 처리
    df = pd.read_excel('a.xlsx', engine='openpyxl', sheet_name=i)
    print(df)
    res.append(df)#배열에 추가
    
res[0].append(res[1]) #DataFrame 병합
```

        이름  국어  영어  수학
    0  aaa  43  65  87
    1  bbb  67  43  65
    2  ccc  12  76  54
    3  ddd  54  89  43
    4  eee  43  95  56
       이름  국어  영어  수학
    0  가나  98  87  36
    1  다라  67  43  65
    2  마바  12  76  54

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>이름</th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>aaa</td>
      <td>43</td>
      <td>65</td>
      <td>87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bbb</td>
      <td>67</td>
      <td>43</td>
      <td>65</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ccc</td>
      <td>12</td>
      <td>76</td>
      <td>54</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ddd</td>
      <td>54</td>
      <td>89</td>
      <td>43</td>
    </tr>
    <tr>
      <th>4</th>
      <td>eee</td>
      <td>43</td>
      <td>95</td>
      <td>56</td>
    </tr>
    <tr>
      <th>0</th>
      <td>가나</td>
      <td>98</td>
      <td>87</td>
      <td>36</td>
    </tr>
    <tr>
      <th>1</th>
      <td>다라</td>
      <td>67</td>
      <td>43</td>
      <td>65</td>
    </tr>
    <tr>
      <th>2</th>
      <td>마바</td>
      <td>12</td>
      <td>76</td>
      <td>54</td>
    </tr>
  </tbody>
</table>
</div>




```python
res = []
for i in range(0, 2):
    df = pd.read_excel('a.xlsx', engine='openpyxl', sheet_name=i)
    print(df)
    res.append(df)
    
res[0].merge(res[1])      
    
```

        이름  국어  영어  수학
    0  aaa  43  65  87
    1  bbb  67  43  65
    2  ccc  12  76  54
    3  ddd  54  89  43
    4  eee  43  95  56
        이름  사회  지리  과학
    0  aaa  43  65  87
    1  bbb  67  43  65
    2  ccc  12  76  54
    3  ddd  54  89  43
    4  eee  43  95  56





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>이름</th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
      <th>사회</th>
      <th>지리</th>
      <th>과학</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>aaa</td>
      <td>43</td>
      <td>65</td>
      <td>87</td>
      <td>43</td>
      <td>65</td>
      <td>87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bbb</td>
      <td>67</td>
      <td>43</td>
      <td>65</td>
      <td>67</td>
      <td>43</td>
      <td>65</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ccc</td>
      <td>12</td>
      <td>76</td>
      <td>54</td>
      <td>12</td>
      <td>76</td>
      <td>54</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ddd</td>
      <td>54</td>
      <td>89</td>
      <td>43</td>
      <td>54</td>
      <td>89</td>
      <td>43</td>
    </tr>
    <tr>
      <th>4</th>
      <td>eee</td>
      <td>43</td>
      <td>95</td>
      <td>56</td>
      <td>43</td>
      <td>95</td>
      <td>56</td>
    </tr>
  </tbody>
</table>
</div>




```python
 #현재 작업 디렉토리(./)에서 문자 패턴과 일치하는 .xlsx파일명 읽음
import glob
files = glob.glob('./*.xlsx') # glob.glob(조건): 조건에 맞는 파일명들을 리스트로 반환
res = []
print(files)
for i in files:
    df = pd.read_excel(i, engine='openpyxl')
    print(df)
    res.append(df)
x = res[0].append(res[1])
```

    ['.\\a.xlsx', '.\\b.xlsx']
        이름  국어  영어  수학
    0  aaa  43  65  87
    1  bbb  67  43  65
    2  ccc  12  76  54
    3  ddd  54  89  43
    4  eee  43  95  56
        이름  국어  영어  수학
    0  aaa  43  65  87
    1  bbb  67  43  65
    2  ccc  12  76  54
    3  ddd  54  89  43
    4  eee  43  95  56



```python
# 엑셀파일 생성 + 쓰기
wr = pd.ExcelWriter('c.xlsx', engine='xlsxwriter') # 채울 내용을 담은 엑셀파일 읽어오기
x.to_excel(wr, index=False, sheet_name='fileA_and_fileB') #sheet_name='Sheet3'
wr.save()
glob.glob('c.xlsx')   #파일 생성 확인. 있으면 파일명이 포함된 리스트가 반환될 것
```


    ['c.xlsx']


```python
# 위에서 엑셀파일에 쓴 내용은 x에 저장됨
x.sum(axis=1)
x.mean(axis=1)
```


    0    195
    1    175
    2    142
    3    186
    4    194
    0    195
    1    175
    2    142
    3    186
    4    194
    dtype: int64



###### 엑셀파일 읽기 예제


```python
# 참고용 예제
res = []
for i in range(0, 2):
    df = pd.read_excel('a.xlsx', engine='openpyxl', sheet_name=i)
    print(df)
    res.append(df)
    
x = res[0].merge(res[1])  #국영수 쉬트와 사,지,과 쉬트 병합하여 x에 저장
t = x.sum(axis=1)         #각 행의 총합
a = x.mean(axis=1)        #각 행의 평균
x['total']=t              #총합 결과를 total컬럼으로 추가
x['avg']=a                #평균 결과를 avg 컬럼으로 추가

wr = pd.ExcelWriter('c.xlsx', engine='xlsxwriter')
x.to_excel(wr, index=False, sheet_name='result') #sheet_name='Sheet3'
wr.save()
```

        이름  국어  영어  수학
    0  aaa  43  65  87
    1  bbb  67  43  65
    2  ccc  12  76  54
    3  ddd  54  89  43
    4  eee  43  95  56
        이름  사회  지리  과학
    0  aaa  43  65  87
    1  bbb  67  43  65
    2  ccc  12  76  54
    3  ddd  54  89  43
    4  eee  43  95  56

