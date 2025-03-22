# 📦 PHP 패키징과 배포

PHP 애플리케이션을 실제 서비스 환경에 배포하려면 **코드를 패키징하고, 필요한 설정 및 라이브러리를 함께 준비**해야 합니다.  
또한 배포 자동화(CI/CD), 캐시 최적화, 권한 설정 등도 함께 고려해야 합니다.  

---

## 1️⃣ PHP 애플리케이션 패키징 기본

PHP는 일반적으로 **컴파일 없이 소스 코드 자체를 배포**합니다.  

#### 구성 예시

```text
my-app/
├── public/           # 웹 루트 (index.php)
├── src/              # PHP 소스 코드
├── vendor/           # Composer 의존성
├── config/           # 설정 파일
├── .env              # 환경 변수
├── composer.json     # 패키지 정보
└── index.php
```

✔ `vendor/`는 `composer install`로 생성 가능  
✔ `.env`는 민감한 정보가 담기므로 외부에 노출되지 않게 주의  

---

## 2️⃣ Composer로 패키지화

PHP 라이브러리 또는 모듈을 **Composer 패키지**로 만들어 배포할 수 있습니다.

#### composer.json 예시

```json
{
  "name": "yourname/my-library",
  "description": "유틸 함수 모음",
  "type": "library",
  "autoload": {
    "psr-4": {
      "MyLib\\": "src/"
    }
  },
  "require": {
    "php": ">=7.4"
  }
}
```

✔ `composer install` → 의존성 설치  
✔ `composer dump-autoload` → 오토로딩 설정  
✔ GitHub와 Packagist를 연동하여 공개 배포 가능  

---

## 3️⃣ 실제 서비스 서버에 배포하기

| 방법             | 설명                                                                 |
|------------------|----------------------------------------------------------------------|
| 수동 배포        | FTP/SFTP로 직접 파일 업로드                                           |
| 자동 배포        | Git → 서버 Pull, CI/CD 툴 (GitHub Actions, Jenkins 등)로 자동화       |
| Docker 배포      | 컨테이너 환경으로 일관된 서버 구성 가능                              |
| 클라우드 배포     | AWS EC2 / Elastic Beanstalk / Heroku 등에서 배포 가능                 |

#### Git으로 서버 배포 예시

```bash
# 서버에서
git clone https://github.com/username/project.git
cd project
composer install --no-dev
php artisan migrate  # (Laravel일 경우)
```

---

## 4️⃣ 배포 시 고려할 항목

| 항목               | 설명 |
|--------------------|------|
| `.env`             | 개발/운영 환경에 따라 설정 분리 (`.env.production` 등) |
| `composer install` | `--no-dev` 옵션으로 프로덕션 의존성만 설치 |
| `cache clear`      | 라우팅, 설정, 뷰 캐시 제거 (Laravel 등) |
| `chmod`            | `storage/`, `cache/` 디렉토리 쓰기 권한 설정 |
| 오류 표시 끄기     | `display_errors = Off` (php.ini 또는 ini_set) |
| 로깅 설정          | 로그 파일 위치 및 권한 확인 (`logs/`, `error_log`) |

---

## 5️⃣ 파일/폴더 권한 설정 (보안 중요)

```bash
chmod -R 755 .
chmod -R 775 storage
chmod -R 775 bootstrap/cache
```

✔ 소스코드는 읽기 권한만 필요  
✔ 로그/세션/캐시 디렉토리는 웹서버 쓰기 권한 필요  

---

## 6️⃣ 배포 자동화 (CI/CD)

#### GitHub Actions 예시 (`.github/workflows/deploy.yml`)

```yaml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: 8.1
      - run: composer install --no-dev --optimize-autoloader
      - run: rsync -avz . user@server:/var/www/project
```

✔ 푸시 시 자동으로 서버에 코드 복사 및 빌드 가능  

---

## 7️⃣ Docker로 PHP 배포

#### 1. Dockerfile 예시:

```Dockerfile
FROM php:8.2-apache

COPY . /var/www/html
RUN docker-php-ext-install pdo pdo_mysql
```

#### 2. 이미지 빌드: `docker build -t my-php-app .`  
#### 3. 컨테이너 실행: `docker run -p 8080:80 my-php-app`

✔ 환경 통일, 이식성 향상 됩니다.  

---

## 🎯 정리

✔ PHP는 소스 코드 중심의 배포 구조이며, Composer를 통해 패키지/라이브러리화할 수 있습니다.  
✔ 서비스 배포 시에는 `.env`, `vendor/`, `권한 설정`, `캐시 삭제` 등을 반드시 점검해야 합니다.  
✔ FTP 배포보다는 Git, CI/CD, Docker 등을 통한 자동화된 배포가 실무에 적합합니다.  
✔ 배포 후 보안을 위해 `오류 출력 OFF`, `파일 권한 제한`, `접근 제어` 설정도 중요합니다.

