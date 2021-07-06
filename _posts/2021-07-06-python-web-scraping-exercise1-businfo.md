---
title: "파이썬 웹 스크래핑 실습: 오픈 API를 통한 실시간 공공데이터 활용 - 서울시 버스노선정보"
categories:	
  - Python
tags:
  - Python3
  - 실습
  - Web Scraping
  - Requests
  - BeautifulSoup
---

# 실습 문제

#### 문제 내용

1. 버스 번호를 입력 받아서, 그 버스노선의 버스 정차정거장 목록, 첫차시간, 막차시간, 배차시간을 출력하라.
2. 정거장명을 입력받아서, 이 정거장에 정차하는 버스목록 출력



#### 사용한 데이터 출처

- **공공데이터 포털**(https://www.data.go.kr/index.do)
  - 서울특별시_노선정보조회 서비스(https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15000193) - xml 형식
  -  서울특별시_정류소정보조회 서비스(https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15000303) - xml 형식



###### 공공데이터 접근방법

1. 회원가입을 한 뒤 **로그인**을 한다.
2. **원하는 데이터를 검색**한 뒤 선택하고 하단의 '**활용신청**'을 눌러 승인을 받는다.
3. 1~2시간 뒤 승인을 받으면 해당 데이터에 접근할 수 있는 **인증키가 발급**된다.
   - 인증키는 <u>'마이 페이지'에서 확인</u>할 수 있다.
4. 데이터마다 **첨부된 문서를 다운받아** 데이터에 접근하는 방법을 확인한다.



## 1. 특정 버스의 정차정거장 목록, 첫차시간, 막차시간, 배차시간 

- 사용한 데이터: 서울특별시_노선정보조회 서비스


```python
import numpy as np
import pandas as pd
import requests
from bs4 import BeautifulSoup
```


```python
 # 검색된 버스정보들 중 검색한 번호와 정확히 일치하는 버스에 대한 레코드의 인덱스 번호를 반환
def checkBusNum(res, num):
   
    if len(res) != 0:   
        idx = None
    
        for i in range(len(res)):
            res_i = res[i]
            bus_num = res[i].find('busroutenm').text
            if bus_num == num:
                idx = i
        return idx
        
    else:
        return None

# 특정 버스가 정차하는 정류장 목록을 만드는 함수
def getBusRoute(key, bus_id):
    url = "http://ws.bus.go.kr/api/rest/busRouteInfo/getStaionByRoute"
    queryParams = "?ServiceKey=" + key + "&busRouteId=" + bus_id

    xml = requests.get(url + queryParams).text
    root = BeautifulSoup(xml)

    res = root.select('itemList')
    
    if len(res) != 0:
        station_list = []
        for i in range(len(res)):
            xx = res[i].select('stationNm')
            station_list.append(xx[0].text)

        return station_list
    
    else:
        return None
```


```python
def getBusInfo(num):
    url = "http://ws.bus.go.kr/api/rest/busRouteInfo/getBusRouteList"
    # key: 공공데이터포털에서 데이터 접근을 승인받으면 발급받는 키
    key = "발급받은 encoding 키"
    queryParams = "?ServiceKey=" + key + "&strSrch=" + num
    
    xml = requests.get(url + queryParams).text
    root = BeautifulSoup(xml)
    res = root.select('itemList')
        
    # 검색한 버스번호와 정확히 일치하는 정보의 인덱스 넘버 반환
    # why? 검색한 문자열이 포함된 모든 버스정보들이 한꺼번에 나오기 때문
    idx = checkBusNum(res=res, num=num)

    try:
        bus_id = res[idx].find('busrouteid').text
        bus_num = res[idx].find('busroutenm').text
        first_bus_tm = res[idx].find('firstbustm').text
        last_bus_tm = res[idx].find('lastbustm').text
        term = res[idx].find('term').text
    except:
        return None
    else:
        station_list = getBusRoute(key=key, bus_id=bus_id)
    
        return bus_num, first_bus_tm, last_bus_tm, term, station_list
```


```python
# 주의: 실제 버스번호와 정확히 똑같은 이름으로 검색해야 함
num = input('버스번호:')
try:
    bus_num, first_bus_tm, last_bus_tm, term, station_list = getBusInfo(num)
    print(f'버스번호: {bus_num} / 첫차시간: {first_bus_tm} / 막차시간: {last_bus_tm} / 배차간격: {term}분')
    print('정차정거장:', station_list)
except:
    print('해당하는 버스 정보 없음')
```

- 내가 애용하는 370번 버스의 정보를 검색해보았다.

```
버스번호:370
버스번호: 370 / 첫차시간: 20210706040000 / 막차시간: 20210706225000 / 배차간격: 6분
정차정거장: ['강동공영차고지', '강일리버파크6단지', '강일리버파크8단지', '강일고등학교.강일리버파크7단지', '강일리버파크5단지.4단지', '강일리버파크2단지.4단지', '강일리버파크1단지.3단지', '강일동주민센터', '강동공영차고지', '고덕리엔파크1단지', '해뜨는주유소', '고덕천.상일동역교차로', '고덕주공3단지', '고덕주공6단지', '강동첨단업무단지.상일여고입구', '상일초등학교', '초이동', '강동자이.프라자아파트', '둔촌2동주민센터', '길동사거리.강동세무서', '강동역', '천호역', '광나루역', '새밭교회', '어린이대공원후문아차산역', '중곡동입구삼거리', '군자역4번출구.용마초등학교', '군자교입구', '장한평역', '청년회의소', '답십리역사거리', '신답초등학교.청계한신휴플러스', '동대문구청', '서울시동부병원', '신설동역.서울풍물시장', '동묘앞', '동대문(흥인지문)', '종로6가.동대문종합시장', '종로5가.광장시장', '종로4가.종묘', '종로3가.탑골공원', '종로2가', '종로1가', '광화문', '서울역사박물관.경희궁앞', '서대문경찰서', '한국경제신문사,서소문역사공원', '충정로역2호선', '충정로역5호선', '서대문역사거리', '서울역사박물관.경희궁앞', '광화문', '종로1가', '종로2가', '종로3가.탑골공원', '종로4가.종묘', '종로5가.광장시장', '종로6가.동대문종합시장', '동대문역.흥인지문', '숭인동', '동대문우체국.서울풍물시장', '용두동신동아아파트', '용두역', '동대문구청', '신답초등학교.청계한신휴플러스', '답십리역사거리', '청년회의소.서울새활용플라자', '장한평역', '군자교입구', '군자역5번출구.용마초등학교', '중곡동입구삼거리', '어린이대공원후문아차산역', '새밭교회', '광나루역', '천호역', '강동역', '길동사거리.강동세무서', '둔촌2동주민센터', '강동자이·프라자아파트', '초이동', '상일초교', '강동첨단업무단지.상일여고입구', '고덕리엔파크3단지', '강명초등학교', '고덕천.상일동역교차로', '해뜨는주유소', '고덕리엔파크1단지', '강일리버파크3단지', '강일동주민센터', '강일리버파크3단지.1단지', '강일리버파크4단지.2단지', '강일리버파크4단지.5단지', '강일고등학교.강일리버파크7단지', '강일리버파크8단지', '강일리버파크6단지']
```

- 물론 실제 어플리케이션 등에서 이 데이터를 활용한다면, 여기서 추가적인 데이터 가공이 필요할 것이다.



## 2. 정류장을 지나가는 모든 버스노선 검색

- 사용한 데이터:  서울특별시_정류소정보조회 서비스

```python
#2 정류장명 입력 받아 그 정류장을 지나가는 모든 버스노선 검색
# 정류장 id를 인자로 사용, 그 정류장을 지나가는 버스노선들을 반환하는 함수
def getBusnmByStID(ars_id):
    url = "http://ws.bus.go.kr/api/rest/stationinfo/getRouteByStation"
    key = "발급받은 encoding 키"
    queryParams = "?ServiceKey=" + key + "&arsId=" + ars_id

    xml = requests.get(url + queryParams).text
    root = BeautifulSoup(xml)
    res = root.select('itemList')

    bus_list = []
    for i in range(len(res)):
        bus_nm = res[i].find('busroutenm').text
        bus_list.append(bus_nm)
        
    return bus_list

# 정류장명을 인자로 사용, 그 정류장명을 포함한 모든 정류장의 목록 리스트 반환
def getBusnmByStName(st_name):
    url = "http://ws.bus.go.kr/api/rest/stationinfo/getStationByName"
    key = "발급받은 encoding 키"
    queryParams = "?ServiceKey=" + key + "&stSrch=" + st_name

    xml = requests.get(url + queryParams).text
    root = BeautifulSoup(xml)
    res = root.select('itemList')

    st_list = []
    for i in range(len(res)):
        st_pair = []
        ars_id = res[i].find('arsid').text
        st_nm = res[i].find('stnm').text
        st_pair.append(ars_id)
        st_pair.append(st_nm)
        st_list.append(st_pair)

    for i in range(len(st_list)):
        ars_id = st_list[i][0]
        bus_list = getBusnmByStID(ars_id)
        st_list[i].append(bus_list)
    
    return st_list

# 정류장별 버스목록 리스트를 깔끔하게 출력해주는 함수
def printBusList(st_list):
    if st_list != []:
        for i in range(len(st_list)):
            print(f'정류장ID:{st_list[i][0]}, 정류장명:{st_list[i][1]}, 경유버스목록:{st_list[i][2]}')
    else:
        print('검색결과 없음')
```


```python
# 정류장명을 검색하면, 그 입력문자열을 포함한 모든 정류장의 정차버스목록 출력해준다.
st_name = input('정류장명에 포함된 문자열:')
st_list = getBusnmByStName(st_name)
printBusList(st_list)
```

- 실제로 결과가 잘 나오는지 익숙한 우리 동네 이름으로 검색해보자.

```
정류장명에 포함된 문자열:상일동
정류장ID:25107, 정류장명:고덕천.상일동역교차로, 경유버스목록:['370', '2312', '3321']
정류장ID:25108, 정류장명:고덕천.상일동역교차로, 경유버스목록:['370', '2312', '3321']
정류장ID:25295, 정류장명:상일동동아아파트, 경유버스목록:['13-2광주', '13광주', '16광주', '30하남']
정류장ID:25296, 정류장명:상일동동아아파트, 경유버스목록:['13-2광주', '13광주', '16광주', '30하남']
정류장ID:25299, 정류장명:상일동빌라단지, 경유버스목록:['2312', '13-2광주', '13광주', '16광주', '30하남']
정류장ID:25300, 정류장명:상일동빌라단지, 경유버스목록:['2312', '13-2광주', '13광주', '16광주', '30하남']
정류장ID:25113, 정류장명:상일동산.롯데캐슬베네루체, 경유버스목록:['2312', '초이-01하남']
정류장ID:25114, 정류장명:상일동산.롯데캐슬베네루체, 경유버스목록:['2312']
정류장ID:25133, 정류장명:상일동역1번출구, 경유버스목록:['340', '342', 'N30', '3318', '3321', '3411', '3413', '81하남']
정류장ID:25165, 정류장명:상일동역2번출구, 경유버스목록:['강동02', '강동05', '3412']
정류장ID:25166, 정류장명:상일동역2번출구, 경유버스목록:['강동02', '강동05', '3412']
정류장ID:25523, 정류장명:상일동역3.4번출구, 경유버스목록:['강동02', '강동05']
정류장ID:25131, 정류장명:상일동역4번출구.센트럴푸르지오, 경유버스목록:['340', '342', 'N30', '3318', '3411', '3412', '3413', '81하남', '초이-01하남']
정류장ID:25740, 정류장명:상일동역5번출구, 경유버스목록:['강동02', '강동05']
정류장ID:25132, 정류장명:상일동역5번출구.고덕아르테온, 경유버스목록:['340', '342', 'N30', '3318', '3411', '3412', '3413', '81하남']
정류장ID:25134, 정류장명:상일동역8번출구, 경유버스목록:['340', '342', 'N30', '3318', '3321', '3411', '3413', '81하남']
정류장ID:25297, 정류장명:상일동주몽재활원, 경유버스목록:['2312', '13-2광주', '13광주', '16광주', '30하남']
정류장ID:25298, 정류장명:상일동주몽재활원, 경유버스목록:['2312', '13-2광주', '13광주', '16광주', '30하남']
정류장ID:25252, 정류장명:상일동주민센터.고덕사회체육센터, 경유버스목록:['2312', '3212', '3321']
정류장ID:25253, 정류장명:상일동주민센터.롯데캐슬베네루체, 경유버스목록:['2312', '3212', '3321']
```



## 공공데이터 활용시 주의점

- 개개의 공공데이터들은 데이터 형식, 접근방법, 기능 등이 매우 상이하다.
- 따라서 본인이 원하는 데이터 추출을 위해서 해당 데이터에 대한 정보를 파악한 뒤 전처리 전략을 세워야 할 것이다.



