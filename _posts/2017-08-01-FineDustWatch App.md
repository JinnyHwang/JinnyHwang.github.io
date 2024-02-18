---
layout: post
title:  "2017.08.01 FineDustWatch App"
date:   2017-08-01 17:32:00 +09:00
author: Jieun
categories: Project
cover:  "/assets/instacode.png"
---

### FineDustWatch App Info
한국환경공단에서 제공하는 국가대기오염정보 OpenAPI를 사용하여 기간별, 시도별 대기오염 정보와 통합대기환경지수 나쁨 이상 측정소 내역, 대기질 예보 통보 내역 등을 조회할 수 있습니다.
<br/>
<img src="/assets/2017_FineDustWatch/FineDustApp_Info.png" title="FineDustWatch App Info">
<br/>
<br/>
<br/>
### Project Structure
<img src="/assets/2017_FineDustWatch/FineDustApp_Structure.jpg" title="FineDustApp_Structure">
서비스 Key를 받아 정보를 활용할 수 있고, 교환데이터 표준은 xml이나 json 형식이며 메시지 교환 유형은 Request-Response방식을 사용합니다.
<br/>
사용자가 측정소 이름(stationName)과 요청 데이터 기간(dataTerm) 정보를 보내면 xml로 응답을 받아 items(목록) 태그에서 측정일(dataTime), 아황산가스 농도(so2Value), 일산화탄소 농도(coValue), 오존 농도(o3Value), 이산화질소 농도(no2Value), 미세먼지 농도(pm10Value), 초미세먼지 농도(pm25Value) 정보를 가져옵니다. 결과값(item)은 1시간 간격으로 있으므로 현재 시간을 검색할 경우 가장 최신 정보를 가져오고(제일 상단에 있는 item), 과거 날짜를 입력하는 경우 시간대별 리스트로 보여줍니다.
<br/>
안드로이드 스튜디오에서 만들 화면 UI는 data를 입력 받을 inputData화면과 content.php를 보여줄 webView화면 2개 입니다. 두 화면을 동작시키기 위해 사용자에게 입력 받은 값을 URI에 GET방식으로 전달하여 xml형식의 응답을 받습니다. 받은 값을 content.php에서 parsing하여 화면을 꾸미고 webView화면에 보여줍니다.
<br/>
<br/>
<br/>
### App UI
<img src="/assets/2017_FineDustWatch/FineDustApp Screen.jpg" title="FineDustWatch Img total">
<img src="/assets/2017_FineDustWatch/FineDustApp1.png" title="FineDustApp1">
<img src="/assets/2017_FineDustWatch/FineDustApp2.png" title="FineDustApp2">
<img src="/assets/2017_FineDustWatch/FineDustApp3.png" title="FineDustApp3">
<img src="/assets/2017_FineDustWatch/FineDustApp4.png" title="FineDustApp4">
<img src="/assets/2017_FineDustWatch/FineDustApp5.png" title="FineDustApp5">
<img src="/assets/2017_FineDustWatch/FineDustApp6.png" title="FineDustApp6">
<img src="/assets/2017_FineDustWatch/FineDustApp7.png" title="FineDustApp7">
<br/>
<br/>
<br/>
### GitHub Link
[2017.08 미세먼지정보기](https://github.com/JinnyHwang/2023_Study_backup/tree/main/2017.08%20%EB%AF%B8%EC%84%B8%EB%A8%BC%EC%A7%80%EC%A0%95%EB%B3%B4%EA%B8%B0)

