# Teachable Machine을 활용한 닮은 동물상 찾기 웹 사이트 제작

## 📌 Preivew


<br/>

## 📌 Skills
### Language
<a><img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=JavaScript&logoColor=white"/></a>

### IDE
<a><img src="https://img.shields.io/badge/Goorm-66FFFF.svg?&style=for-the-badge&logoColor=white"/></a>

### Skills
<a><img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white"/></a>
<a><img src="https://img.shields.io/badge/CSS-239120?&style=for-the-badge&logo=css3&logoColor=white"/></a>
<a><img src="https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white"/></a>

### Deploy
<a><img src="https://img.shields.io/badge/Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white" /></a>

<br/>

## 📌 Backend Descriptions
### `Crawler`
> ✏️ 네이버 검색창처럼 사용자에게 검색을 받고, 원하는 개수 만큼 '네이버 뉴스'의 내용을 BeautifulSoup을 사용하여 스크랩합니다. <br/>
> 1페이지당 10개씩이기 때문에, 91개로 설정시에 총 10페이지를 긁어 오게 됩니다. <br/>
> 양이 많아질 수록 대기 시간이 오래 걸립니다. <br/>
> 1. request 라이브러리를 사용하여 사용자가 form 태그(프론트)에서 검색한 키워드를 가져옵니다. 
> 2. contents_cleansing(contents=내용들) : 스크랩 된 내용을 정제화 해주는 함수 입니다.
> 3. crawler(maxNum=최대개수설정, query=사용자가검색한키워드쿼리, sort=정렬, s_date=시작날짜, e_date=종료날짜) 

<br/>

```python

```
---

### `Data-To-DataFrame`
> 크롤러를 통해 긁어온 정보들을 토대로 데이터프레임을 생성합니다. <br/>
> date(날짜), title(제목), press(언론사), contents(내용), link(하이퍼링크)로 구성된 프레임으로 생성하였습니다.

<br/>

```python
col_name = ["date", "title", "press", "contents", "link"]
rows = maxNum * 10
target_articles = pd.DataFrame(np.reshape(df1, (rows, 5)), columns=col_name).T.to_dict()
```

