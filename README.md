# techlibrary_Booknumbering
> tehchlibrary_Booknumbering은 리재철 한글순도서기호법 제 5표에 따른 분류를 적용한 hangul.py 와
> 카터-샌본에 따른 원서 분류를 적용한 cutter-sand.py로 구성되어있습니다

<h2>설치 및 사용방법</h2>
<h3> 설치해야할 라이브러리 </h3>


<ul>
    <li>sudo pip3.5 install xlrd</li>
    <li>sudo pip3.5 install xlwt</li>
    <li>sudo pip3.5 install hangul-utils</li>
    <li>sudo pip3.5 install requests</li>
    <li>sudo pip3.5 install beautifulsoup4</li>
</ul>

<h3> hangul.py </h3>
 리재철 한글순도서기호법 제 5 표에 따르면 저자명의 두번째가 'ㅊ' 인지 아닌지에 따라 뒤의 모음기호가 달라지기때문에

<b>def all_num2(ip)</b>는 저자명의 두번째 글자 자음이 ㅊ이 아닌경우
<b>def ch_num2(ip)</b>는 저자명의 두번째 글자 자음이 ㅊ인 경우  입니다 .








