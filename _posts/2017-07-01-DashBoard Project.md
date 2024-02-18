---
layout: post
title:  "2017.07.01 DashBoard Project"
date:   2017-07-01 18:34:00 +09:00
author: Jieun
categories: Project
cover:  "/assets/instacode.png"
---

### DashBoard Project Info
글을 등록, 수정, 삭제할 수 있는 게시판 웹입니다.<br/>
게시글 리스트에선 글 제목, 작성자, 작성날짜를 알 수 있고, 최근 글이 맨 앞에 위치해있습니다.<br/>
검색 기능이 있어서 작성자, 제목, 내용을 기준으로 게시글을 검색할 수 있습니다.<br/>
📝Development Language<br/>
> PHP, HTML, JavaScript<br/>
>
💻Development Environment<br/>
> phpMyAdmin<br/>
>
<img src="/assets/2017_DashBoard/DashBoard_Info.png" title="DashBoard Info">
<br/><br/><br/>
### Project Structure
dbconfig.php엔 DB연결 정보를 담고, board.php 한 파일에 모든 기능을 구현했습니다.<br/>
board.php파일은 크게 4개 form으로 구성되어 있고, form에서 보낸 정보를 JavaScript 함수로 처리했습니다.<br/>
데이터베이스로 boardList table과 b_comment table을 만들어 게시글과 댓글 정보를 저장했습니다.<br/>
<img src="/assets/2017_DashBoard/DashBoard_structure.png" title="DashBoard Structure">
<br/><br/><br/>
### Web UI
<img src="/assets/2017_DashBoard/DashBoard_Screenshot.jpg" title="DashBoard Screenshot">
