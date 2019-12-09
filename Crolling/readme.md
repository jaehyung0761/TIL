# Tody I Learned Crolling

> **Note**: 웹 크롤링

## Table of Contents

  1. [Library](#Library)
  1. [WebCrolling](#WebCrolling)
  1. [Pandas](#Pandas)
  
### Library
  - 1.1 **Library**\
    - requests
    >  웹사이트를 접속하여 HTML을 가져오는 라이브러리
    ```python
    import requests
    
    html = requests.get("https://www.naver.com/")
    ```
  
    - BeautifulSoup
    > HTML파일로부터 데이터를 뽑아내기 위한 라이브러리
    ```python
    import requests
    import bs4

    html = requests.get("https://www.naver.com/")
    bs4_object = bs4.BeautifulSoup(html.text,"html.parser")
    ```
### WebCrolling
  - 2.1 **WebCrolling**\
    - 네이버 날씨 페이지 크롤링 하기
    >  특정지역의 날씨 크롤링 하기
    ```python
    import requests
    import bs4

    html = requests.get("https://search.naver.com/search.naver?query=동작구 날씨")
    bs4_object = bs4.BeautifulSoup(html.text,"html.parser")
    span_tag = bs4_object.find("span",{"class":"btn_select"})
    em_tag = span_tag.find('em').text                                   # 위치
    todaytemp = bs4_object.find('p',{"class","info_temperature"}).text  # 현재온도
    sensible = bs4_object.find('span',{"class","sensible"}).text        # 체감온도
    print("{}의 온도 {}, {}".format(em_tag,todaytemp,sensible))          # 결과값 : 서울특별시 동작구 노량진동의 온도 7도씨℃ , 체감온도 6˚
    ```

    >  사용자에게 입력받은 위치의 날씨 크롤링하기
    ```python
    import requests
    import bs4

    def weather(loc):
        html = requests.get("https://search.naver.com/search.naver?query={} 날씨".format(loc))
        bs4_object = bs4.BeautifulSoup(html.text,"html.parser")
        span_tag = bs4_object.find("span",{"class":"btn_select"})
        em_tag = span_tag.find('em').text
        todaytemp = bs4_object.find('p',{"class","info_temperature"}).text
        sensible = bs4_object.find('span',{"class","sensible"}).text
        print("{}의 온도 {}, {}".format(em_tag,todaytemp,sensible))

    ask = input("현재위치를 입력하세요")
    print(weather(ask))
    ```
  
    - 네이버 웹툰 페이지 크롤링하기
    >  제목, 작가, 이미지 링크 크롤링 하기
    ```python
    import requests
    import bs4

    html = requests.get("https://comic.naver.com/webtoon/weekdayList.nhn?week=mon")
    bs4_object = bs4.BeautifulSoup(html.text,"html.parser")
    ul_tag = bs4_object.find("ul",{"class":"img_list"})
    for i in ul_tag.findAll("li"):
        print(i.find("a")['title'])                  # 제목
        print(i.find("a",{"href":'#'}).text)         # 작가
        print(i.find("img")['src'])                  # 이미지url
        print("*"*20)
    ```
**[⬆ back to top](#table-of-contents)**

### Pandas
  - 3.1 **Pandas**\
    - Pandas
    >  데이터 분석을 위해 자주 사용되는 라이브러리로 행과 열로 이루어진 데이터 객체를 만들어 사용한다.
    - DataFrame
    >  2차원으로 행과 열이 있고 Dictionary데이터를 DataFrame으로 변환가능하다.
    ```
    import pandas as pd

    data = {
        "이름":['john','jack','adam'],
        "국어":[80,90,75],
        "영어":[85,100,75],
        "수학":[75,95,86]
    }
    df = pd.DataFrame(data)
    df.to_excel('score.xlsx')
    ```
    - 정부혁신1번가 페이지
    >  크롤링 후 데이터프레임으로 변환 후 엑셀파일로 내보내기
    ```
    import bs4
    import requests
    import pandas as pd
    
    title_list = []
    dep_list = []

    for i in range(1,17):
        html = requests.get("https://www.innogov.go.kr/ucms/bbs/B0000042/list.do?sort=02&searchCnd=1&searchWrd=&pageIndex={}&menuNo=300125".format(i))
        bs4_object = bs4.BeautifulSoup(html.text,"html.parser")
        tbody_tag = bs4_object.find("tbody")
        for i in tbody_tag.findAll("tr"):
            title = i.find("a").text
            title_list.append(title)
            dep = i.find("td",{"class":'tac name'}).text
            dep_list.append(dep)

    data = {
        "제목": title_list,
        "부서": dep_list
    }
    df = pd.DataFrame(data)
    df.to_excel("gov_practice2.xlsx")            # 데이터 프레임을 엑셀파일로 내보내서 엑셀에서 사용할 수 있다.
    ```
