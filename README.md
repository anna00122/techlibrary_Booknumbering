# techlibrary_Booknumbering
> tehchlibrary_Booknumbering은 리재철 한글순도서기호법 제 5표에 따른 분류를 적용한 hangul.py 와
> 카터-샌본에 따른 원서 분류를 적용한 cutter-sand.py로 구성되어있습니다

# 설치 및 사용방법
## 설치

    * $sudo pip3.5 install xlrd
    * $sudo pip3.5 install xlwt
    * $sudo pip3.5 install hangul-utils
    * $sudo pip3.5 install requests
    * $sudo pip3.5 install beautifulsoup4


## hangul.py
 리재철 한글순도서기호법 제 5표에 따른 청구기호 분류입니다
 리재철 한글순도서기호법 제 5표에 따르면, 청구 기호는 '카테고리 +저자첫글자명(first)+저자두번째글자의기호화(second)+도서명의첫자음(book_first)+기타' 로 구성됩니다
 기타에는 중복된 책인경우 두 번째 도서는 c2, 세 번째는 c3로 표기합니다.

 예를 들어 ,저자의 성에 해당하는 ‘이’를 그대로 쓰고 이름의 첫 자인 ‘찬’에서 ‘ㅊ’에 해당하는 8과 ‘ㅏ’에 해당하는 2을 붙여 ‘이82’로 쓴다.
 마지막에는 책 제목의 첫 글자인 ‘파’에서 초성인 ‘ㅍ’을 붙인다. 결국 ‘이82ㅍ’이 된다.

 ![Alt text](/Users/kakao/PycharmProjects/techlibrary/리재철5표.jpg)


[list.xlsx]
A열: 관리번호
B열: ISBN 넘버
C열: 도서명
D열: 도서 소제목
E열: 저자
F열: 출판사
G열: "청구기호" 가 들어갈것.

로 G열에 청구기호가 입력되게 할것입니다


### 엑셀파일 불러와 값 지정하기

```

import openpyxl

init = []
for row in range(num_rows):
  r = {}
  for col in range(num_cols):
    if col == 4:
      r['author'] = sheet.cell(row=row+1, column=col+1).value
    elif col == 2:
      r['book'] = sheet.cell(row=row+1, column=col+1).value
  init.append(r)

```
col은 0부터 시작하기때문에, 엑셀 상 5번째 열 = (col==4) 임을 주의
해당 줄의 5번째 열에 있는 값을 key 'author'의 value로 지정
해당 줄의 3번째 열에 있는 값을 key 'book'의 value로 지정 하여
딕셔너리인 r에 저장

### 저자 두번째글자의 기호화 (second)

저자명의 두번째가 'ㅊ' 인지 아닌지에 따라 뒤의 모음기호가 달라집니다
<b>all_num2(ip)</b>는 저자명의 두번째 글자 자음이 ㅊ이 아닌경우<br>
<b>ch_num2(ip)</b>는 저자명의 두번째 글자 자음이 ㅊ인 경우<br>

### 저자명 공란 없애기
```
for i in init:
  i['author'] = i['author'].replace(" ", "")
  author = i['author']
  book = i['book']


```
저자 두번째 글자가 비어있으면 에러가 발생하므로 공란을 없애주고
key값이 'author' 인 value자체를 author로,
key값이 'book' 인 value자체를 book으로 정의

### 자음모음분리

```
jamo = split_syllables(author[1])
  jamo_list = list(jamo)
```
```
author = '이찬현'
author[1] = 찬
jamo = split_syllables(author[1])
jamo = ('ㅊ','ㅏ','ㄴ')
jamo list = ['ㅊ','ㅏ,'ㄴ]

```
### 중복된 책인경우

cc = 1
if cnt > 1:
    num = num + 'c' + str(cnt)

  sheet.cell(row=cc, column=7, value=num)

  cc += 1
```



### Const
* save_num 청구기호 저장 리스트
* workbook 엑셀파일 불러오기
* sheet_name 엑셀파일에서 첫번째 시트
* num_rows 행의 갯수
* num_cols 열의 갯수
* jamo_list[0] 저자명 두번째 글자의 첫번째 자음
* num1 저자명 두번째 글자에 따른 자음 기호
* num2 저자명 두번째 글자에 따른 모음 기호
* first  저자명 첫번째 글자
* second  num1 + num2
* bookname_first 도서명 첫글자
* cnt 청구기호중 동일한 num의 갯수
* cc 줄



## cutter-sand.py
커터샌본 저자기호표 (Cutter-sanborn,열거식저자기호표) 저자 기호표에 따른 원서 기호분류입니다.
이 기호법은 미국의 Charles Ammi Cutter에 의해서 고안된 것으로 저자기호법 가운데
세계적으로 널리 보급되어 있는데 듀이가 DDC에 적합한 저자기호표라고 추천한 것과도 무관하지는 않습니다

먼저 해당 도서의 기본표목(주로 개인저자명)을 찾는데 그것이 개인저자명인 경우
그 저자 성의 頭文字(두문자) 한자와 그 저자성에 해당하는 숫자기호를 결합하여 기본기호(저자기호)를 구성한다.
이때 기본표목이 서명이나 단체저자명인 경우 첫 중요어(key word, 다만 관사는 무시)를저자성으로 간주하여 동일하게 부여한다.
저자성에 일치하는 문자가 표 중에 없는 경우는 알파벳순으로
바로 위(앞에)에 있는 문자의 숫자기호를 취하여 저자기호로 한다. 이 때 A566은 Andrews, E에 해당되는 기호이다.

예)
Bach B118
Garden G218
Holmes H749

### 웹크롤링을 통해 구현하였다 (이유는 분류법의 자료가 적어 잘못 분류할 가능성이 농후하기때문 ... )
```
data1 = {'action': 'result', 'autor': author}
  result = requests.post("http://www.unforbi.com.ar/cutteren/index.php",
                         data=data1)
  soup1 = BeautifulSoup(result.content, 'html.parser')
  num = str(soup1.find('strong')).replace("<strong>", "").replace("</strong>","")
  time.sleep(2)
```









