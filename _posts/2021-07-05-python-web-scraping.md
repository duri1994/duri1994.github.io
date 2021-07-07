---
title: "파이썬: 웹 스크래핑"
categories:	
  - Python
tags:
  - Python3
  - Concept
  - Web Scraping
  - Requests
  - BeautifulSoup
---

# 웹 스크래핑(Web Scraping)

웹 스크래핑이란 웹사이트의 데이터를 수집하는 작업을 의미한다.

최근 빅데이터 분석이나 머신러닝이 떠오르면서, 대량의 데이터를 수집하는 것이 중요한 작업이 되었다.

따라서 웹 스크래핑은 개발자에게 있어 필수적으로 갖춰야 하는 능력이라고 할 수 있다.



###### 추가 관련 용어

- **웹 크롤링**(Crawling)

  웹 스크래핑과 비슷한 용어로 웹 크롤링이 있다.

  두 용어는 정의상 차이가 있으나 사실상 웹 스크래핑과 웹 크롤링 용어를 혼용하고 있다고 한다.

  따라서 '**웹페이지를 탐색하고 데이터를 수집하여 가공하는 작업'**이라고 이해하고 있어도 무방하다.

- **파싱**(Parsing)

  파싱이란 **어떤 페이지(문서/html 등)에서** 원하는 **데이터를 특정 패턴이나 순서로 추출하여 가공하는 것**을 의미한다.

  

# 외부 데이터 처리

- 웹상에 존재하는 데이터를 수집하는 방법은 크게 두가지로 나뉜다.
  1. <u>공공데이터나 오픈된 데이터</u>를 **파일로 다운** 받아서 사용(csv, 엑셀, xml, json...)  
  2. **웹 페이지를 읽어서** 데이터 분석(그 웹 페이지 구조를 분석해야함)  
  
  그러면 구글 홈페이지를 한번 스크래핑 해보자.
  
  - requests: http 요청을 편리하게 보낼 수 있게 해주는 패키지


```python
# 웹페이지 html 소스 스크래핑
#pip install requests
import requests
html = requests.get('https://google.com').text  #웹 요청을 보내 해당 url의 html 소스 get
html
```


    '<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="ko"><head><meta content="text/html; charset=UTF-8" http-equiv="Content-Type"><meta content="/images/branding/googleg/1x/googleg_standard_color_128dp.png" itemprop="image"><title>Google</title><script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){window.google={kEI:\'Ab3eYIGeCaeOr7wPrfyNsAE\',kEXPI:\'0,18168,184142,569905,1,530320,56873,954,5104,207,4804,2316,383,246,5,1354,5251,16231,10,1106274,1197742,540,93,328892,51223,16115,28684,17572,4859,1361,9290,3027,4748,7993,4841,4020,978,13228,3847,4192,6430,7432,7096,234,4283,2777,919,2855,2226,1593,1279,2212,239,291,149,1103,840,2197,4100,109,4011,2023,2297,7875,6795,3227,2845,7,12354,5096,7877,5036,1456,1953,906,2,940,2615,13142,3,576,6459,149,13975,4,1252,276,2304,1236,5227,576,4683,2015,4067,14308,2658,6701,483,173,30,3878,9750,2305,638,7080,10535,665,2521,3303,2533,992,3102,18,3120,6,613,295,3,3541,1,11337,606,2767,1814,283,389,2,1,3,519,5990,6754,984,4788,2,1394,2806,1721,2,3051,2017,3335,815,4798,1931,1532,2377,93,1713,617,1275,4172,406,2091,1480,1,125,172,1001,1571,1015,1865,100,2,442,598,75,1085,1365,3330,2,1711,292,1440,33,904,1401,1320,3673,669,204,3,56,4678,682,57,13,1447,321,1739,394,7,9,1,1064,226,168,69,110,2,233,6,1773,386,2,1,250,2,6,65,2,6,42,1744,442,118,478,454,4976,439,133,290,358,16,17,22,2,295,145,420,327,6,1032,170,587,1185,18,5621565,189,5,32,63,157,59,59,5996750,11,114,2800572,882,444,1,2,80,1,1796,1,9,2,2551,1,748,141,795,563,1,4265,1,1,2,1331,3299,843,2609,155,17,13,72,139,4,2,20,2,169,13,19,46,5,39,96,548,29,2,2,1,2,1,2,2,7,4,1,2,2,2,2,2,2,353,222,8,283,186,1,1,38,3,3,2,2,2,53,28,1,1,14,5,6,3,2,2,2,2,2,4,2,3,3,21,23954970,2857454,8573,1144247,268,1835,26467,2,2374,3,120,3,6,338,3,1454,386,499,75,1017,762\',kBL:\'agTH\'};google.sn=\'webhp\';google.kHL=\'ko\';})();(function(){\nvar f=this||self;var h,k=[];function l(a){for(var b;a&&(!a.getAttribute||!(b=a.getAttribute("eid")));)a=a.parentNode;return b||h}function m(a){for(var b=null;a&&(!a.getAttribute||!(b=a.getAttribute("leid")));)a=a.parentNode;return b}\nfunction n(a,b,c,d,g){var e="";c||-1!==b.search("&ei=")||(e="&ei="+l(d),-1===b.search("&lei=")&&(d=m(d))&&(e+="&lei="+d));d="";!c&&f._cshid&&-1===b.search("&cshid=")&&"slh"!==a&&(d="&cshid="+f._cshid);c=c||"/"+(g||"gen_204")+"?atyp=i&ct="+a+"&cad="+b+e+"&zx="+Date.now()+d;/^http:/i.test(c)&&"https:"===window.location.protocol&&(google.ml&&google.ml(Error("a"),!1,{src:c,glmm:1}),c="");return c};h=google.kEI;google.getEI=l;google.getLEI=m;google.ml=function(){return null};google.log=function(a,b,c,d,g){if(c=n(a,b,c,d,g)){a=new Image;var e=k.length;k[e]=a;a.onerror=a.onload=a.onabort=function(){delete k[e]};a.src=c}};google.logUrl=n;}).call(this);(function(){\ngoogle.y={};google.sy=[];google.x=function(a,b){if(a)var c=a.id;else{do c=Math.random();while(google.y[c])}google.y[c]=[a,b];return!1};google.sx=function(a){google.sy.push(a)};google.lm=[];google.plm=function(a){google.lm.push.apply(google.lm,a)};google.lq=[];google.load=function(a,b,c){google.lq.push([[a],b,c])};google.loadAll=function(a,b){google.lq.push([a,b])};google.bx=!1;google.lx=function(){};}).call(this);google.f={};(function(){\ndocument.documentElement.addEventListener("submit",function(b){var a;if(a=b.target){var c=a.getAttribute("data-submitfalse");a="1"==c||"q"==c&&!a.elements.q.value?!0:!1}else a=!1;a&&(b.preventDefault(),b.stopPropagation())},!0);document.documentElement.addEventListener("click",function(b){var a;a:{for(a=b.target;a&&a!=document.documentElement;a=a.parentElement)if("A"==a.tagName){a="1"==a.getAttribute("data-nohref");break a}a=!1}a&&b.preventDefault()},!0);}).call(this);</script><style>#gbar,#guser{font-size:13px;padding-top:1px !important;}#gbar{height:22px}#guser{padding-bottom:7px !important;text-align:right}.gbh,.gbd{border-top:1px solid #c9d7f1;font-size:1px}.gbh{height:0;position:absolute;top:24px;width:100%}@media all{.gb1{height:22px;margin-right:.5em;vertical-align:top}#gbar{float:left}}a.gb1,a.gb4{text-decoration:underline !important}a.gb1,a.gb4{color:#00c !important}.gbi .gb4{color:#dd8e27 !important}.gbf .gb4{color:#900 !important}\n</style><style>body,td,a,p,.h{font-family:&#44404;&#47548;,&#46027;&#50880;,arial,sans-serif}.ko{font-size:9pt}body{margin:0;overflow-y:scroll}#gog{padding:3px 8px 0}td{line-height:.8em}.gac_m td{line-height:17px}form{margin-bottom:20px}.h{color:#1558d6}em{font-weight:bold;font-style:normal}.lst{height:25px;width:496px}.gsfi,.lst{font:18px arial,sans-serif}.gsfs{font:17px arial,sans-serif}.ds{display:inline-box;display:inline-block;margin:3px 0 4px;margin-left:4px}input{font-family:inherit}body{background:#fff;color:#000}a{color:#4b11a8;text-decoration:none}a:hover,a:active{text-decoration:underline}.fl a{color:#1558d6}a:visited{color:#4b11a8}.sblc{padding-top:5px}.sblc a{display:block;margin:2px 0;margin-left:13px;font-size:11px}.lsbb{background:#f8f9fa;border:solid 1px;border-color:#dadce0 #70757a #70757a #dadce0;height:30px}.lsbb{display:block}#WqQANb a{display:inline-block;margin:0 12px}.lsb{background:url(/images/nav_logo229.png) 0 -261px repeat-x;border:none;color:#000;cursor:pointer;height:30px;margin:0;outline:0;font:15px arial,sans-serif;vertical-align:top}.lsb:active{background:#dadce0}.lst:focus{outline:none}.tiah{width:458px}</style><script nonce="RYdk48pkDI4/Cja+STyZQA=="></script></head><body bgcolor="#fff"><script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){var src=\'/images/nav_logo229.png\';var iesg=false;document.body.onload = function(){window.n && window.n();if (document.images){new Image().src=src;}\nif (!iesg){document.f&&document.f.q.focus();document.gbqf&&document.gbqf.q.focus();}\n}\n})();</script><div id="mngb"><div id=gbar><nobr><b class=gb1>&#44160;&#49353;</b> <a class=gb1 href="https://www.google.co.kr/imghp?hl=ko&tab=wi">&#51060;&#48120;&#51648;</a> <a class=gb1 href="https://maps.google.co.kr/maps?hl=ko&tab=wl">&#51648;&#46020;</a> <a class=gb1 href="https://play.google.com/?hl=ko&tab=w8">Play</a> <a class=gb1 href="https://www.youtube.com/?gl=KR&tab=w1">YouTube</a> <a class=gb1 href="https://news.google.com/?tab=wn">&#45684;&#49828;</a> <a class=gb1 href="https://mail.google.com/mail/?tab=wm">Gmail</a> <a class=gb1 href="https://drive.google.com/?tab=wo">&#46300;&#46972;&#51060;&#48652;</a> <a class=gb1 style="text-decoration:none" href="https://www.google.co.kr/intl/ko/about/products?tab=wh"><u>&#45908;&#48372;&#44592;</u> &raquo;</a></nobr></div><div id=guser width=100%><nobr><span id=gbn class=gbi></span><span id=gbf class=gbf></span><span id=gbe></span><a href="http://www.google.co.kr/history/optout?hl=ko" class=gb4>&#50937; &#44592;&#47197;</a> | <a  href="/preferences?hl=ko" class=gb4>&#49444;&#51221;</a> | <a target=_top id=gb_70 href="https://accounts.google.com/ServiceLogin?hl=ko&passive=true&continue=https://www.google.com/&ec=GAZAAQ" class=gb4>&#47196;&#44536;&#51064;</a></nobr></div><div class=gbh style=left:0></div><div class=gbh style=right:0></div></div><center><br clear="all" id="lgpd"><div id="lga"><img alt="Google" height="92" src="/images/branding/googlelogo/1x/googlelogo_white_background_color_272x92dp.png" style="padding:28px 0 14px" width="272" id="hplogo"><br><br></div><form action="/search" name="f"><table cellpadding="0" cellspacing="0"><tr valign="top"><td width="25%">&nbsp;</td><td align="center" nowrap=""><input name="ie" value="ISO-8859-1" type="hidden"><input value="ko" name="hl" type="hidden"><input name="source" type="hidden" value="hp"><input name="biw" type="hidden"><input name="bih" type="hidden"><div class="ds" style="height:32px;margin:4px 0"><div style="position:relative;zoom:1"><input class="lst tiah" style="margin:0;padding:5px 8px 0 6px;vertical-align:top;color:#000;padding-right:38px" autocomplete="off" value="" title="Google &#44160;&#49353;" maxlength="2048" name="q" size="57"><img src="/textinputassistant/tia.png" style="position:absolute;cursor:pointer;right:5px;top:4px;z-index:300" data-script-url="/textinputassistant/11/ko_tia.js" id="tsuid1" alt="" height="23" width="27"><script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){var id=\'tsuid1\';document.getElementById(id).onclick = function(){var s = document.createElement(\'script\');s.src = this.getAttribute(\'data-script-url\');(document.getElementById(\'xjsc\')||document.body).appendChild(s);};})();</script></div></div><br style="line-height:0"><span class="ds"><span class="lsbb"><input class="lsb" value="Google &#44160;&#49353;" name="btnG" type="submit"></span></span><span class="ds"><span class="lsbb"><input class="lsb" id="tsuid2" value="I&#8217;m Feeling Lucky" name="btnI" type="submit"><script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){var id=\'tsuid2\';document.getElementById(id).onclick = function(){if (this.form.q.value){this.checked = 1;if (this.form.iflsig)this.form.iflsig.disabled = false;}\nelse top.location=\'/doodles/\';};})();</script><input value="AINFCbYAAAAAYN7LER5d1H_wuADO03mCN7G-3_YcVACZ" name="iflsig" type="hidden"></span></span></td><td class="fl sblc" align="left" nowrap="" width="25%"><a href="/advanced_search?hl=ko&amp;authuser=0">&#44256;&#44553;&#44160;&#49353;</a></td></tr></table><input id="gbv" name="gbv" type="hidden" value="1"><script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){\nvar a,b="1";if(document&&document.getElementById)if("undefined"!=typeof XMLHttpRequest)b="2";else if("undefined"!=typeof ActiveXObject){var c,d,e=["MSXML2.XMLHTTP.6.0","MSXML2.XMLHTTP.3.0","MSXML2.XMLHTTP","Microsoft.XMLHTTP"];for(c=0;d=e[c++];)try{new ActiveXObject(d),b="2"}catch(h){}}a=b;if("2"==a&&-1==location.search.indexOf("&gbv=2")){var f=google.gbvu,g=document.getElementById("gbv");g&&(g.value=a);f&&window.setTimeout(function(){location.href=f},0)};}).call(this);</script></form><div id="gac_scont"></div><div style="font-size:83%;min-height:3.5em"><br></div><span id="footer"><div style="font-size:10pt"><div style="margin:19px auto;text-align:center" id="WqQANb"><a href="/intl/ko/ads/">&#44305;&#44256; &#54532;&#47196;&#44536;&#47016;</a><a href="http://www.google.co.kr/intl/ko/services/">&#48708;&#51592;&#45768;&#49828; &#49556;&#47336;&#49496;</a><a href="/intl/ko/about.html">Google &#51221;&#48372;</a><a href="https://www.google.com/setprefdomain?prefdom=KR&amp;prev=https://www.google.co.kr/&amp;sig=K_GoTjWU6WU0Ke3fyaBZdFIxv3tl0%3D">Google.co.kr</a></div></div><p style="font-size:8pt;color:#70757a">&copy; 2021 - <a href="/intl/ko/policies/privacy/">&#44060;&#51064;&#51221;&#48372;&#52376;&#47532;&#48169;&#52840;</a> - <a href="/intl/ko/policies/terms/">&#50557;&#44288;</a></p></span></center><script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){window.google.cdo={height:757,width:1440};(function(){\nvar a=window.innerWidth,b=window.innerHeight;if(!a||!b){var c=window.document,d="CSS1Compat"==c.compatMode?c.documentElement:c.body;a=d.clientWidth;b=d.clientHeight}a&&b&&(a!=google.cdo.width||b!=google.cdo.height)&&google.log("","","/client_204?&atyp=i&biw="+a+"&bih="+b+"&ei="+google.kEI);}).call(this);})();</script> <script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){google.xjs={ck:\'\',cs:\'\',excm:[],pml:false};})();</script>  <script nonce="RYdk48pkDI4/Cja+STyZQA==">(function(){var u=\'/xjs/_/js/k\\x3dxjs.hp.en.b02EctaivfE.O/m\\x3dsb_he,d/am\\x3dAHgCLA/d\\x3d1/ed\\x3d1/rs\\x3dACT90oHUAsFQThMGCtNqHFlxhDQRoX8v8g\';\nvar e=this||self,f=function(a){return a};var g;var l=function(a,b){this.g=b===h?a:""};l.prototype.toString=function(){return this.g+""};var h={};function m(){var a=u;google.lx=function(){n(a);google.lx=function(){}};google.bx||google.lx()}\nfunction n(a){google.timers&&google.timers.load&&google.tick&&google.tick("load","xjsls");var b=document;var c="SCRIPT";"application/xhtml+xml"===b.contentType&&(c=c.toLowerCase());c=b.createElement(c);if(void 0===g){b=null;var k=e.trustedTypes;if(k&&k.createPolicy){try{b=k.createPolicy("goog#html",{createHTML:f,createScript:f,createScriptURL:f})}catch(p){e.console&&e.console.error(p.message)}g=b}else g=b}a=(b=g)?b.createScriptURL(a):a;a=new l(a,h);c.src=a instanceof l&&a.constructor===l?a.g:"type_error:TrustedResourceUrl";var d;a=(c.ownerDocument&&c.ownerDocument.defaultView||window).document;(d=(a=null===(d=a.querySelector)||void 0===d?void 0:d.call(a,"script[nonce]"))?a.nonce||a.getAttribute("nonce")||"":"")&&c.setAttribute("nonce",d);document.body.appendChild(c);google.psa=!0};setTimeout(function(){m()},0);})();(function(){window.google.xjsu=\'/xjs/_/js/k\\x3dxjs.hp.en.b02EctaivfE.O/m\\x3dsb_he,d/am\\x3dAHgCLA/d\\x3d1/ed\\x3d1/rs\\x3dACT90oHUAsFQThMGCtNqHFlxhDQRoX8v8g\';})();function _DumpException(e){throw e;}\nfunction _F_installCss(c){}\n(function(){google.jl={attn:false,blt:\'none\',dw:false,emtn:0,ine:false,lls:\'default\',pdt:0,snet:true,ubm:false,uwp:true};})();(function(){var pmc=\'{\\x22d\\x22:{},\\x22sb_he\\x22:{\\x22agen\\x22:true,\\x22cgen\\x22:true,\\x22client\\x22:\\x22heirloom-hp\\x22,\\x22dh\\x22:true,\\x22dhqt\\x22:true,\\x22ds\\x22:\\x22\\x22,\\x22ffql\\x22:\\x22ko\\x22,\\x22fl\\x22:true,\\x22host\\x22:\\x22google.com\\x22,\\x22isbh\\x22:28,\\x22jsonp\\x22:true,\\x22msgs\\x22:{\\x22cibl\\x22:\\x22&#44160;&#49353;&#50612; &#51648;&#50864;&#44592;\\x22,\\x22dym\\x22:\\x22&#51060;&#44163;&#51012; &#52286;&#51004;&#49512;&#45208;&#50836;?\\x22,\\x22lcky\\x22:\\x22I&#8217;m Feeling Lucky\\x22,\\x22lml\\x22:\\x22&#51088;&#49464;&#55176; &#50508;&#50500;&#48372;&#44592;\\x22,\\x22oskt\\x22:\\x22&#51077;&#47141; &#46020;&#44396;\\x22,\\x22psrc\\x22:\\x22&#44160;&#49353;&#50612;&#44032; \\\\u003Ca href\\x3d\\\\\\x22/history\\\\\\x22\\\\u003E&#50937; &#44592;&#47197;\\\\u003C/a\\\\u003E&#50640;&#49436; &#49325;&#51228;&#46104;&#50632;&#49845;&#45768;&#45796;.\\x22,\\x22psrl\\x22:\\x22&#49325;&#51228;\\x22,\\x22sbit\\x22:\\x22&#51060;&#48120;&#51648;&#47196; &#44160;&#49353;\\x22,\\x22srch\\x22:\\x22Google &#44160;&#49353;\\x22},\\x22nrft\\x22:false,\\x22ovr\\x22:{},\\x22pq\\x22:\\x22\\x22,\\x22refpd\\x22:true,\\x22refspre\\x22:true,\\x22rfs\\x22:[],\\x22sbas\\x22:\\x220 3px 8px 0 rgba(0,0,0,0.2),0 0 0 1px rgba(0,0,0,0.08)\\x22,\\x22sbpl\\x22:16,\\x22sbpr\\x22:16,\\x22scd\\x22:10,\\x22stok\\x22:\\x22ogKNkuovqJuekwiOwaP0VepCSZs\\x22,\\x22uhde\\x22:false}}\';google.pmc=JSON.parse(pmc);})();</script>        </body></html>'



- Beautifulsoup: html, xml 형식의 소스를 파싱하여 DOM 객체로 변환해주는 패키지
  - 태그 단위로 손쉽게 소스를 추출할 수 있다.


```python
#pip install beautifulsoup4
from bs4 import BeautifulSoup # for html parsing
```


```python
content = BeautifulSoup(html, 'html.parser')  #html소스를 DOM(document object model) 객체화 
# BeautifulSoup(html, 'parser 종류')
title = content.html.head.title    #소스에서 title 태그 추출
print(title.string)  #string:태그의 텍스트 값 <태그>텍스트</태그>
```

    Google



# 1. html 태그 접근 방법  

1. root.html.body.h1 : 태그 한개 검색  
2. root.find(태그[속성]): 태그 1개 검색(반드시 '속성'은 고유한 값임)    
3. root.find_all(태그[속성]): 태그 모두 검색(반환값이 리스트)    
4. root.select(태그): 태그 모두 검색


```python
#link = content.html.body.a  #처음 a 태그 하나만 추출
links = content.find_all('a') # a 태그들을 몽땅 추출
links
```




    [<a class="gb1" href="https://www.google.co.kr/imghp?hl=ko&amp;tab=wi">이미지</a>,
     <a class="gb1" href="https://maps.google.co.kr/maps?hl=ko&amp;tab=wl">지도</a>,
     <a class="gb1" href="https://play.google.com/?hl=ko&amp;tab=w8">Play</a>,
     <a class="gb1" href="https://www.youtube.com/?gl=KR&amp;tab=w1">YouTube</a>,
     <a class="gb1" href="https://news.google.com/?tab=wn">뉴스</a>,
     <a class="gb1" href="https://mail.google.com/mail/?tab=wm">Gmail</a>,
     <a class="gb1" href="https://drive.google.com/?tab=wo">드라이브</a>,
     <a class="gb1" href="https://www.google.co.kr/intl/ko/about/products?tab=wh" style="text-decoration:none"><u>더보기</u> »</a>,
     <a class="gb4" href="http://www.google.co.kr/history/optout?hl=ko">웹 기록</a>,
     <a class="gb4" href="/preferences?hl=ko">설정</a>,
     <a class="gb4" href="https://accounts.google.com/ServiceLogin?hl=ko&amp;passive=true&amp;continue=https://www.google.com/&amp;ec=GAZAAQ" id="gb_70" target="_top">로그인</a>,
     <a href="/advanced_search?hl=ko&amp;authuser=0">고급검색</a>,
     <a href="/intl/ko/ads/">광고 프로그램</a>,
     <a href="http://www.google.co.kr/intl/ko/services/">비즈니스 솔루션</a>,
     <a href="/intl/ko/about.html">Google 정보</a>,
     <a href="https://www.google.com/setprefdomain?prefdom=KR&amp;prev=https://www.google.co.kr/&amp;sig=K_qSU2i_5xt6kb-AE7ha4VFtmBjls%3D">Google.co.kr</a>,
     <a href="/intl/ko/policies/privacy/">개인정보처리방침</a>,
     <a href="/intl/ko/policies/terms/">약관</a>]




```python
links = content.find_all('a') # a 태그들을 몽땅 추출
for i in links:
    print(i.get_text(), ':', i['href'])#속성값 읽기
```

    이미지 : https://www.google.co.kr/imghp?hl=ko&tab=wi
    지도 : https://maps.google.co.kr/maps?hl=ko&tab=wl
    Play : https://play.google.com/?hl=ko&tab=w8
    YouTube : https://www.youtube.com/?gl=KR&tab=w1
    뉴스 : https://news.google.com/?tab=wn
    Gmail : https://mail.google.com/mail/?tab=wm
    드라이브 : https://drive.google.com/?tab=wo
    더보기 » : https://www.google.co.kr/intl/ko/about/products?tab=wh
    웹 기록 : http://www.google.co.kr/history/optout?hl=ko
    설정 : /preferences?hl=ko
    로그인 : https://accounts.google.com/ServiceLogin?hl=ko&passive=true&continue=https://www.google.com/&ec=GAZAAQ
    고급검색 : /advanced_search?hl=ko&authuser=0
    광고 프로그램 : /intl/ko/ads/
    비즈니스 솔루션 : http://www.google.co.kr/intl/ko/services/
    Google 정보 : /intl/ko/about.html
    Google.co.kr : https://www.google.com/setprefdomain?prefdom=KR&prev=https://www.google.co.kr/&sig=K_qSU2i_5xt6kb-AE7ha4VFtmBjls%3D
    개인정보처리방침 : /intl/ko/policies/privacy/
    약관 : /intl/ko/policies/terms/



```python
# sample html 생성
html = '<html>'
html += '<body>'
html += '<p class=a>aaa</p>'
html += '<p class=b>bbb</p>'
html += '<p class=a>ccc</p>'
html += '<p class=a id=xxx>ddd</p>'
html += '</body>'
html += '</html>'

# 다양한 방법으로 특정 요소에 접근해보기
root = BeautifulSoup(html, 'html.parser') # parsing한 것을 root에 담는다.

## find_all: 여러개의 검색 결과를 리스트 반환
# 태그가 p인 모든 것
p1 = root.find_all('p')
print('p1:', p1)

# 태그가 p, 클래스가 a인 것 모두
p2 = root.find_all('p', {'class':'a'}) # find_all(태그, 검색조건(딕셔너리)
print('p2:', p2)

# 태그가 p, 클래스가 b인 것 모두
p3 = root.find_all('p', {'class':'b'}) 
print('p3:', p3)

## find: 맨 처음 하나의 값만 반환(리스트 x)
# 태그가 p, 클래스가 a면서 id가 xxx인 것 중 맨 처음 하나
p4 = root.find('p', {'class':'a', 'id': 'xxx'}) 
print('p4:', p4)

## select: 여러개 검색하는 함수. 반환 타입이 '리스트'
# 태그가 p인 모든 것
p5 = root.select('p')
print('p5:', p5)

## select(태그.클래스명)
# 태그이름이 p, 클래스이름이 a인 모든 것
p6 = root.select('p.a') #태그.클래스명 //태그#id명 (규칙임)
print('p6:', p6)

# 태그이름이 p, 클래스이름이 b인 모든 것
p7 = root.select('p.b')
print('p7:', p7)

## select(태그#id)
# 태그이름이 p고, id가 xxx인 것
p8 = root.select('p#xxx')
print('p8:', p8)
```

    p1: [<p class="a">aaa</p>, <p class="b">bbb</p>, <p class="a">ccc</p>, <p class="a" id="xxx">ddd</p>]
    p2: [<p class="a">aaa</p>, <p class="a">ccc</p>, <p class="a" id="xxx">ddd</p>]
    p3: [<p class="b">bbb</p>]
    p4: <p class="a" id="xxx">ddd</p>
    p5: [<p class="a">aaa</p>, <p class="b">bbb</p>, <p class="a">ccc</p>, <p class="a" id="xxx">ddd</p>]
    p6: [<p class="a">aaa</p>, <p class="a">ccc</p>, <p class="a" id="xxx">ddd</p>]
    p7: [<p class="b">bbb</p>]
    p8: [<p class="a" id="xxx">ddd</p>]


결국 원하는 데이터에 적절하게 접근하기 위해선, 그 사이트의 html소스 구조에 대하여 정확하게 파악하고 있어야 한다.



# 2. XML


```python
# 날씨정보(xml)
html = requests.get('http://www.kma.go.kr/weather/forecast/mid-term-rss3.jsp').text  #웹 요청
#html=html.decode('utf-8')
#지역별 일간 날씨를 출력
root = BeautifulSoup(html, 'html.parser') 
loc = root.find_all('location') # location 태그들에 각각 도시의 날씨정보가 들어가 있다.
for i in loc: # location 태그들을 하나씩 가져온다.
    print(i.city.get_text(), '지역 날씨========') # 도시명은 city태그의 텍스트값
    d = i.find_all('data') # 그 location 태그의 하위태그명이 data인 모든 데이터를 d에 담음
    for j in d:
        # '시간 : 날씨' 만 출력
        # print(j.tmef.string , ':', j.wf.string)
        print(j.tmef.string , ' / 날씨:', j.wf.string, ' / 최저온도:', j.tmn.string,
              ' / 최고기온:', j.tmx.string, ' / 습도:', j.rnst.string, '%')
```

    서울 지역 날씨========
    2021-07-05 00:00  / 날씨: 구름많음  / 최저온도: 22  / 최고기온: 29  / 습도: 30 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 22  / 최고기온: 28  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 70 %
    인천 지역 날씨========
    2021-07-05 00:00  / 날씨: 구름많음  / 최저온도: 21  / 최고기온: 27  / 습도: 30 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 27  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 22  / 최고기온: 28  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 27  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 27  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 27  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 70 %
    수원 지역 날씨========
    2021-07-05 00:00  / 날씨: 구름많음  / 최저온도: 21  / 최고기온: 29  / 습도: 30 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 22  / 최고기온: 28  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 70 %
    파주 지역 날씨========
    2021-07-05 00:00  / 날씨: 구름많음  / 최저온도: 19  / 최고기온: 28  / 습도: 30 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 19  / 최고기온: 28  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 20  / 최고기온: 28  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 27  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 27  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 70 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 27  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 27  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 27  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 70 %
    이천 지역 날씨========
    2021-07-05 00:00  / 날씨: 구름많음  / 최저온도: 19  / 최고기온: 27  / 습도: 30 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 19  / 최고기온: 27  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 21  / 최고기온: 28  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 70 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    평택 지역 날씨========
    2021-07-05 00:00  / 날씨: 구름많음  / 최저온도: 20  / 최고기온: 29  / 습도: 30 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 22  / 최고기온: 27  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 27  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 70 %
    춘천 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 구름많음  / 최저온도: 20  / 최고기온: 29  / 습도: 30 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 21  / 최고기온: 28  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 60 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    원주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 28  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 구름많음  / 최저온도: 20  / 최고기온: 28  / 습도: 30 %
    2021-07-06 00:00  / 날씨: 구름많음  / 최저온도: 21  / 최고기온: 28  / 습도: 30 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 60 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 60 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    강릉 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 27  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 구름많음  / 최저온도: 21  / 최고기온: 27  / 습도: 30 %
    2021-07-06 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 26  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 26  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 26  / 습도: 70 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 26  / 습도: 60 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 26  / 습도: 70 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 26  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 27  / 습도: 40 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 60 %
    대전 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 26  / 습도: 40 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 26  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 60 %
    세종 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 26  / 습도: 40 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 26  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 90 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 60 %
    홍성 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 26  / 습도: 40 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 26  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 60 %
    청주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 30  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 30  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 70 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 70 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 60 %
    충주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 60 %
    영동 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 30  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 30  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-06 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 28  / 습도: 40 %
    2021-07-07 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 30  / 습도: 40 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 30  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 70 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 30  / 습도: 70 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-12 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 31  / 습도: 60 %
    광주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 70 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 90 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 31  / 습도: 40 %
    목포 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 70 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 80 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 27  / 습도: 90 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 29  / 습도: 40 %
    여수 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 70 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 80 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 90 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 27  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 27  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 27  / 습도: 40 %
    순천 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 70 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 90 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 29  / 습도: 40 %
    광양 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 70 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 90 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 29  / 습도: 40 %
    나주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 70 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 90 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 31  / 습도: 40 %
    전주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 29  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 31  / 습도: 40 %
    군산 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 29  / 습도: 40 %
    정읍 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 30  / 습도: 40 %
    남원 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 30  / 습도: 40 %
    고창 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 28  / 습도: 40 %
    무주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 29  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 26  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 26  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 90 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 90 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 30  / 습도: 40 %
    부산 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 70 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 27  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 27  / 습도: 40 %
    울산 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 70 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 29  / 습도: 40 %
    창원 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 70 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    진주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 70 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 29  / 습도: 40 %
    거창 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 28  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 28  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 27  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 27  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 70 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 28  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 30  / 습도: 40 %
    통영 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 26  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 70 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 27  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 28  / 습도: 40 %
    대구 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 31  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 31  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 30  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 30  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 31  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 31  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 30  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 30  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 31  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 30  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 30  / 습도: 40 %
    안동 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 28  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 28  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 28  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 30  / 습도: 40 %
    포항 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 27  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 27  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 27  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 28  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 24  / 최고기온: 28  / 습도: 40 %
    경주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 27  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 27  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 29  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 29  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 28  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 27  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 22  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 29  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 23  / 최고기온: 29  / 습도: 40 %
    울진 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 25  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 25  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 25  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 22  / 최고기온: 26  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 26  / 습도: 40 %
    울릉도 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 24  / 습도: 40 %
    2021-07-05 12:00  / 날씨: 흐림  / 최저온도: 20  / 최고기온: 24  / 습도: 40 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 24  / 습도: 80 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 24  / 습도: 80 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 24  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 24  / 습도: 80 %
    2021-07-08 00:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 24  / 습도: 90 %
    2021-07-08 12:00  / 날씨: 흐리고 비  / 최저온도: 21  / 최고기온: 24  / 습도: 80 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 24  / 습도: 80 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 24  / 습도: 70 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 20  / 최고기온: 25  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 25  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 흐림  / 최저온도: 21  / 최고기온: 25  / 습도: 40 %
    제주 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 28  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 30  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 30  / 습도: 70 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 26  / 최고기온: 31  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 26  / 최고기온: 31  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐림  / 최저온도: 25  / 최고기온: 30  / 습도: 40 %
    2021-07-08 12:00  / 날씨: 흐림  / 최저온도: 25  / 최고기온: 30  / 습도: 40 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 30  / 습도: 90 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 30  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 29  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 25  / 최고기온: 30  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 구름많음  / 최저온도: 24  / 최고기온: 30  / 습도: 30 %
    서귀포 지역 날씨========
    2021-07-05 00:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 90 %
    2021-07-05 12:00  / 날씨: 흐리고 비  / 최저온도: 23  / 최고기온: 26  / 습도: 90 %
    2021-07-06 00:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 27  / 습도: 90 %
    2021-07-06 12:00  / 날씨: 흐리고 비  / 최저온도: 24  / 최고기온: 27  / 습도: 70 %
    2021-07-07 00:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 28  / 습도: 80 %
    2021-07-07 12:00  / 날씨: 흐림  / 최저온도: 25  / 최고기온: 28  / 습도: 40 %
    2021-07-08 00:00  / 날씨: 흐림  / 최저온도: 25  / 최고기온: 28  / 습도: 40 %
    2021-07-08 12:00  / 날씨: 흐림  / 최저온도: 25  / 최고기온: 28  / 습도: 40 %
    2021-07-09 00:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 28  / 습도: 90 %
    2021-07-09 12:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 28  / 습도: 80 %
    2021-07-10 00:00  / 날씨: 흐리고 비  / 최저온도: 25  / 최고기온: 28  / 습도: 80 %
    2021-07-11 00:00  / 날씨: 흐림  / 최저온도: 25  / 최고기온: 27  / 습도: 40 %
    2021-07-12 00:00  / 날씨: 구름많음  / 최저온도: 25  / 최고기온: 28  / 습도: 30 %



# 3. json

- 다루기가 더 쉽다(? 많이 접해봐야 체감될듯)
- "속성이름" : 값 조합들이 여러개 있음  
- 멤버변수는 ""으로 묶어줌  
- 문자열은 ""으로 묶어줌(나머지 타입들은 필요없음)  
- 값을 {}로 묶어 그 안에 객체/배열 등을 넣어줄 수 있음 (ex. "속성1" : {객체})
    - 객체안엔 또 다시 '"속성명" : 값' 조합들이 여러개 들어가 있음



```python
# json을 읽기 위한 라이브러리 호출
import json

# 간단한 json 
j = '[{"id":"aaa", "pwd":"111"},{"id":"bbb", "pwd":"222"}]'#json 데이터(객체 2개짜리 리스트)

items = json.loads(j) # json으로 해당 html을 encoding. items는 배열
for item in items: # items 배열에서 item을 하나씩 꺼내온다.
    print(item['id'], ':', item['pwd'])
```

    aaa : 111
    bbb : 222



```python
# 속성값으로 또 다른 객체 or 리스트도 가질 수 있다.
j = '['
j += '{"info": {"id":"aaa", "pwd":"111"}, "colors":["red", "blue", "yellow"]},'
j += '{"info": {"id":"bbb", "pwd":"222"}, "colors":["red2", "blue2", "yellow2"]}'
j += ']'

items = json.loads(j)

# 각 요소에 접근하는 방법에 주목
for item in items:
    print(item['info']['id'], ':', item['colors'][1]) 
```

    aaa : blue
    bbb : blue2



```python
# 각 요소에 접근하는 방법 2
for item in items:
    info = item['info']
    arr = item['colors']
    print(info['id'], ' / ', info['pwd'])
    for a in arr:
        print(a, end='\t')
    print()
```

    aaa  /  111
    red	blue	yellow	
    bbb  /  222
    red2	blue2	yellow2	



```python
html = requests.get('https://api.github.com/repositories').text
items = json.loads(html) # json으로 해당 html을 encoding
for item in items:
    print('id:', item['id'], ' / ', 'name:', item['name'], ' / ', 'login:', item['owner']['login'])
```

    id: 1  /  name: grit  /  login: mojombo
    id: 26  /  name: merb-core  /  login: wycats
    id: 27  /  name: rubinius  /  login: rubinius
    id: 28  /  name: god  /  login: mojombo
    id: 29  /  name: jsawesome  /  login: vanpelt
    id: 31  /  name: jspec  /  login: wycats
    id: 35  /  name: exception_logger  /  login: defunkt
    id: 36  /  name: ambition  /  login: defunkt
    id: 42  /  name: restful-authentication  /  login: technoweenie
    id: 43  /  name: attachment_fu  /  login: technoweenie
    id: 48  /  name: microsis  /  login: caged
    id: 52  /  name: s3  /  login: anotherjesse
    id: 53  /  name: taboo  /  login: anotherjesse
    id: 54  /  name: foxtracs  /  login: anotherjesse
    id: 56  /  name: fotomatic  /  login: anotherjesse
    id: 61  /  name: glowstick  /  login: mojombo
    id: 63  /  name: starling  /  login: defunkt
    id: 65  /  name: merb-more  /  login: wycats
    id: 68  /  name: thin  /  login: macournoyer
    id: 71  /  name: resource_controller  /  login: jamesgolick
    id: 73  /  name: markaby  /  login: jamesgolick
    id: 74  /  name: enum_field  /  login: jamesgolick
    id: 75  /  name: subtlety  /  login: defunkt
    id: 92  /  name: zippy  /  login: defunkt
    id: 93  /  name: cache_fu  /  login: defunkt
    id: 95  /  name: phosphor  /  login: KirinDave
    id: 98  /  name: sinatra  /  login: bmizerany
    id: 102  /  name: gsa-prototype  /  login: jnewland
    id: 105  /  name: duplikate  /  login: technoweenie
    id: 118  /  name: lazy_record  /  login: jnewland
    id: 119  /  name: gsa-feeds  /  login: jnewland
    id: 120  /  name: votigoto  /  login: jnewland
    id: 127  /  name: mofo  /  login: defunkt
    id: 129  /  name: xhtmlize  /  login: jnewland
    id: 130  /  name: ruby-git  /  login: ruby-git
    id: 131  /  name: bmhsearch  /  login: ezmobius
    id: 137  /  name: mofo  /  login: uggedal
    id: 139  /  name: simply_versioned  /  login: mmower
    id: 140  /  name: gchart  /  login: abhay
    id: 141  /  name: schemr  /  login: benburkert
    id: 142  /  name: calais  /  login: abhay
    id: 144  /  name: chronic  /  login: mojombo
    id: 165  /  name: git-wiki  /  login: sr
    id: 177  /  name: signal-wiki  /  login: queso
    id: 179  /  name: ruby-on-rails-tmbundle  /  login: drnic
    id: 185  /  name: low-pro-for-jquery  /  login: danwrong
    id: 186  /  name: merb-core  /  login: wayneeseguin
    id: 190  /  name: dst  /  login: sr
    id: 191  /  name: yaws  /  login: mojombo
    id: 192  /  name: yaws  /  login: KirinDave
    id: 193  /  name: tasks  /  login: sr
    id: 195  /  name: ruby-on-rails-tmbundle  /  login: mattetti
    id: 199  /  name: amazon-ec2  /  login: grempe
    id: 203  /  name: merblogger  /  login: wayneeseguin
    id: 204  /  name: merbtastic  /  login: wayneeseguin
    id: 205  /  name: alogr  /  login: wayneeseguin
    id: 206  /  name: autozest  /  login: wayneeseguin
    id: 207  /  name: rnginx  /  login: wayneeseguin
    id: 208  /  name: sequel  /  login: wayneeseguin
    id: 211  /  name: simply_versioned  /  login: bmizerany
    id: 212  /  name: switchpipe  /  login: peterc
    id: 213  /  name: arc  /  login: hornbeck
    id: 217  /  name: ebay4r  /  login: up_the_irons
    id: 218  /  name: merb-plugins  /  login: wycats
    id: 220  /  name: ram  /  login: up_the_irons
    id: 230  /  name: ambitious_activeldap  /  login: defunkt
    id: 232  /  name: fitter_happier  /  login: atmos
    id: 237  /  name: oebfare  /  login: brosner
    id: 245  /  name: credit_card_tools  /  login: up_the_irons
    id: 248  /  name: rorem  /  login: jnicklas
    id: 249  /  name: braid  /  login: cristibalan
    id: 251  /  name: uploadcolumn  /  login: jnicklas
    id: 252  /  name: ruby-on-rails-tmbundle  /  login: simonjefford
    id: 256  /  name: rack-mirror  /  login: leahneukirchen
    id: 257  /  name: coset-mirror  /  login: leahneukirchen
    id: 267  /  name: javascript-unittest-tmbundle  /  login: drnic
    id: 273  /  name: eycap  /  login: engineyard
    id: 279  /  name: gitsum  /  login: leahneukirchen
    id: 293  /  name: sequel-model  /  login: wayneeseguin
    id: 305  /  name: god  /  login: kevinclark
    id: 307  /  name: blerb-core  /  login: hornbeck
    id: 312  /  name: django-mptt  /  login: brosner
    id: 314  /  name: bus-scheme  /  login: technomancy
    id: 319  /  name: javascript-bits  /  login: caged
    id: 320  /  name: groomlake  /  login: caged
    id: 322  /  name: forgery  /  login: sevenwire
    id: 324  /  name: ambitious-sphinx  /  login: technicalpickles
    id: 329  /  name: soup  /  login: lazyatom
    id: 332  /  name: rails  /  login: josh
    id: 334  /  name: backpacking  /  login: cdcarter
    id: 339  /  name: capsize  /  login: jnewland
    id: 351  /  name: starling  /  login: bs
    id: 360  /  name: ape  /  login: sr
    id: 362  /  name: awesomeness  /  login: collectiveidea
    id: 363  /  name: audited  /  login: collectiveidea
    id: 364  /  name: acts_as_geocodable  /  login: collectiveidea
    id: 365  /  name: acts_as_money  /  login: collectiveidea
    id: 367  /  name: calendar_builder  /  login: collectiveidea
    id: 368  /  name: clear_empty_attributes  /  login: collectiveidea
    id: 369  /  name: css_naked_day  /  login: collectiveidea


pd.read_html(url)  
- 해당 html의 데이터를 실시간 로드해서 <table>태그 안의 내용들을 df로 변환 후 반환
- table 태그가 여러개라면, 각 테이블은 리스트의 인덱스들로 들어감
- 주의: table이 하나 뿐이어도 리스트로 반환되므로 [0]을 붙여서 호출해야 한다.


```python
# 실시간으로 제공되는 사이트 크롤링(실시간 로드)

# pip install lxml
# lxml: XML parser로서 주로 이용되는 패키지
# pip install html5lib
import pandas as pd


# table 태그가 여러개라면, 리스트로 반환
def get_code(name):
    a = pd.read_html('http://kind.krx.co.kr/corpgeneral/corpList.do?method=download', index_col='회사명')[0]
    code = a.loc[name, '종목코드']
    code = '{:0=6d}'.format(code) # 6자리 숫자, 부족한 자릿수는 0으로 채워줌
    return code
                
```


```python
get_code('삼성전자')
```




    '005930'




```python
# (주의) 네이버 금융 사이트의 경우, 브라우저의 요청에만 응답하도록 사이트가 변경됨.
# 따라서, 브라우저인 '척'을 하여, 유효한 웹 요청을 만드는 작업이 필요하다.
url = 'https://finance.naver.com/item/sise_day.nhn?code=005930'

# requests에 headers를 아래와 같이 넣으면 브라우저인 것 처럼 할 수 있다(?)
headers = {'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) \
            AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.96 Safari/537.36'} 

# 당당하게(?) 웹 요청을 시도한다. 성공!
html = requests.get(url, headers = headers)


html = BeautifulSoup(html.text, 'lxml')
html_table = html.select('table')

table = pd.read_html(str(html_table))
table[0].dropna()

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
      <th>날짜</th>
      <th>종가</th>
      <th>전일비</th>
      <th>시가</th>
      <th>고가</th>
      <th>저가</th>
      <th>거래량</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2021.07.02</td>
      <td>80000.0</td>
      <td>100.0</td>
      <td>80000.0</td>
      <td>80400.0</td>
      <td>79900.0</td>
      <td>8181692.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021.07.01</td>
      <td>80100.0</td>
      <td>600.0</td>
      <td>80500.0</td>
      <td>80600.0</td>
      <td>80000.0</td>
      <td>13382882.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021.06.30</td>
      <td>80700.0</td>
      <td>300.0</td>
      <td>81100.0</td>
      <td>81400.0</td>
      <td>80700.0</td>
      <td>13288643.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021.06.29</td>
      <td>81000.0</td>
      <td>900.0</td>
      <td>81900.0</td>
      <td>82100.0</td>
      <td>80800.0</td>
      <td>15744317.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021.06.28</td>
      <td>81900.0</td>
      <td>300.0</td>
      <td>81700.0</td>
      <td>82000.0</td>
      <td>81600.0</td>
      <td>11578529.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2021.06.25</td>
      <td>81600.0</td>
      <td>400.0</td>
      <td>81500.0</td>
      <td>81900.0</td>
      <td>81200.0</td>
      <td>13481405.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2021.06.24</td>
      <td>81200.0</td>
      <td>1100.0</td>
      <td>80400.0</td>
      <td>81400.0</td>
      <td>80100.0</td>
      <td>18771080.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2021.06.23</td>
      <td>80100.0</td>
      <td>100.0</td>
      <td>80500.0</td>
      <td>80600.0</td>
      <td>79900.0</td>
      <td>13856548.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2021.06.22</td>
      <td>80000.0</td>
      <td>100.0</td>
      <td>80200.0</td>
      <td>80300.0</td>
      <td>79900.0</td>
      <td>11773365.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2021.06.21</td>
      <td>79900.0</td>
      <td>600.0</td>
      <td>79700.0</td>
      <td>80000.0</td>
      <td>79600.0</td>
      <td>16063340.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
def get_stock_info(code):
    url = 'https://finance.naver.com/item/sise_day.nhn?code='+code
    headers = {'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) \
            AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.96 Safari/537.36'} 
    html = requests.get(url, headers = headers)

    html = BeautifulSoup(html.text, 'lxml')
    html_table = html.select('table')

    table = pd.read_html(str(html_table))
    table = table[0].dropna()
    return table
```


```python
info = get_stock_info(get_code('삼성전자'))
info
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
      <th>날짜</th>
      <th>종가</th>
      <th>전일비</th>
      <th>시가</th>
      <th>고가</th>
      <th>저가</th>
      <th>거래량</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2021.07.02</td>
      <td>80000.0</td>
      <td>100.0</td>
      <td>80000.0</td>
      <td>80400.0</td>
      <td>79900.0</td>
      <td>8181692.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021.07.01</td>
      <td>80100.0</td>
      <td>600.0</td>
      <td>80500.0</td>
      <td>80600.0</td>
      <td>80000.0</td>
      <td>13382882.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021.06.30</td>
      <td>80700.0</td>
      <td>300.0</td>
      <td>81100.0</td>
      <td>81400.0</td>
      <td>80700.0</td>
      <td>13288643.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021.06.29</td>
      <td>81000.0</td>
      <td>900.0</td>
      <td>81900.0</td>
      <td>82100.0</td>
      <td>80800.0</td>
      <td>15744317.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021.06.28</td>
      <td>81900.0</td>
      <td>300.0</td>
      <td>81700.0</td>
      <td>82000.0</td>
      <td>81600.0</td>
      <td>11578529.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2021.06.25</td>
      <td>81600.0</td>
      <td>400.0</td>
      <td>81500.0</td>
      <td>81900.0</td>
      <td>81200.0</td>
      <td>13481405.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2021.06.24</td>
      <td>81200.0</td>
      <td>1100.0</td>
      <td>80400.0</td>
      <td>81400.0</td>
      <td>80100.0</td>
      <td>18771080.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2021.06.23</td>
      <td>80100.0</td>
      <td>100.0</td>
      <td>80500.0</td>
      <td>80600.0</td>
      <td>79900.0</td>
      <td>13856548.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2021.06.22</td>
      <td>80000.0</td>
      <td>100.0</td>
      <td>80200.0</td>
      <td>80300.0</td>
      <td>79900.0</td>
      <td>11773365.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2021.06.21</td>
      <td>79900.0</td>
      <td>600.0</td>
      <td>79700.0</td>
      <td>80000.0</td>
      <td>79600.0</td>
      <td>16063340.0</td>
    </tr>
  </tbody>
</table>
</div>
