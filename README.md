### 💸 개발 기간

* 2025.09.01(깃허브 생성일)~2025.09.17(주점 행사 시작일)


### 💸개발 주제
* 연합 동아리 주점 행사에서 사용할 수 있는 주문 웹 사이트

> 고객은 매장 내 테이블 QR 코드를 스캔해 주문하고, 서버는 이를 처리하여 OMS(주방/점원 화면)에 표시

### 💸 팀원
* front : 김채현 (cHynekim)
* back : 김하경 (8lueis)

### 💸 기술 스택
* Front
	
    * React (CRA 기반)
    * Axios
    * React Router

* Back

   * Django 5.x~
   * Gunicorn (WSGI 서버)
   * Django REST Framework
   * SimpleJWT

* DB
	
    * MySQL
    * (ORM: Django ORM)

* Infrastructure / DevOps

	
    * Nginx (Reverse Proxy + 정적 파일 서빙)
    * AWS LightSail (배포 서버)
    	* Ubuntu 22.04 
    > * 도메인: http://aehan-mutejujeom.com
 
* Tools / Collaboration
	
    * Git & GitHub (버전 관리)
    * VS Code / PyCharm (개발 환경)


<br> 

### 📈 주요 기능

* QR 스캔 → 테이블 번호 기반 JWT 토큰 발급
* 메뉴 조회, 장바구니 담기, 주문 생성
* 주문 내역 확인 (History)
* OMS(주방) 화면에서 실시간 주문 모니터링
* 관리자 DB에서 매출 합계 확인 가능

<br>

### 주요 URL 

* 프론트 진입: 주문 화면 
> http://aehan-mutejujeom.com/aehanmute/order?table=1

* 점원/주방 화면 
> http://aehan-mutejujeom.com/oms
> * `is_paid = True` 결제 완료, `is_ready=False` 조리 미완성인 경우에만 화면에 출력 

* Django Admin 화면 
> http://aehan-mutejujeom.com/admin/

* API 엔드 포인트 
> http://aehan-mutejujeom.com/api/...


<br>

### GitHub 

* server -> git push 

```git
git add .
git commit -m "2509DD_NAME_배포 환경_수정 내용"
git push origin main
```

* local 최신화 

```git 
git pull origin main
```

* local -> server 
```
git push origin main   # 로컬에서
git pull origin main   # 서버에서
```


<br>

### 로그 확인 

* Gunicorn (Django)
```cmd 
sudo journalctl -u gunicorn -f
sudo journalctl -u gunicorn -n 100 # 최신 100줄 

```

* Nginx (Front)
```cmd
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```


 
 
 <br>

 
 
### 배포 명령어 
 
* Front 
```linux
cd ~/payProject/payProject/front
npm run build
sudo systemctl restart nginx

```

* Back 
```linux
sudo systemctl restart gunicorn

```


### 자주 겪은 에러 

* **401 Unauthorized** → 고객이 뒤로 가기를 눌러 토큰 중복으로 만료가 뜨거나 POST에 정보가 정확히 안 왔을 때 발생 (후자는 주점 당일 수정 완료 ㅜㅜ)
* **~~CORS 오류~~** → API_URL과 도메인 불일치 / `settings.py`에 `CORS_ALLOWED_ORIGINS`에 추가 후 해결 
* **~~404 (정적 파일)~~** → React 빌드 후 Nginx 경로 수정해서 해결 
* **~~500 Internal Server Error~~** → `sudo journalctl -u gunicorn -f` 로 로그 확인 후 해결 (대부분 경로나 post 구조가 다른 경우 발생)
