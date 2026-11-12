
# 🚀 개발 워크스테이션 구축 프로젝트 결과보고서

## 1. 프로젝트 개요

본 프로젝트는 개발 환경 구축에 필요한 Linux 터미널 조작, 파일 권한 관리, Docker 컨테이너 운영 및 Git/GitHub 버전 관리 환경을 직접 구성하고 실습하는 것을 목적으로 한다.

### 주요 실습 목표
- Linux 기본 명령어를 활용한 파일 및 디렉터리 관리
- chmod를 이용한 파일 및 디렉터리 권한 관리
- Docker 이미지 및 컨테이너 생성과 관리
- Dockerfile을 활용한 커스텀 이미지 제작
- 포트 매핑을 통한 컨테이너 서비스 접속
- 바인드 마운트 및 Docker 볼륨을 통한 데이터 영속성 검증
- Git을 활용한 버전 관리 및 GitHub 연동

---

## 2. 실행 환경

| 구분 | 환경 및 버전 |
|---|---|
| Host OS | Windows 11 |
| 가상화 환경 | WSL2 |
| Linux 배포판 | Ubuntu 26.04.1 LTS |
| Linux Kernel | 6.18.54.1-microsoft-standard-WSL2 |
| Shell | /bin/bash |
| Bash 버전 | 5.3.9(1)-release |
| Terminal | VS Code 통합 터미널 |
| Docker Desktop | 4.98.0 |
| Docker Client | 29.9.2 |
| Docker Engine | 29.9.2 |
| Docker Compose | 5.6.1 |
| Git | 2.53.0 |
| Visual Studio Code | 1.143.1 |

### 환경 확인 명령어

```bash
uname -a
cat /etc/os-release
echo $SHELL
bash --version
docker --version
docker version
docker compose version
git --version
code --version
```

---

## 3. 수행 항목 체크리스트

- [x] Linux 터미널 기본 명령어 실습
- [x] 파일 및 디렉터리 생성, 복사, 이동, 삭제
- [x] chmod를 이용한 파일 및 디렉터리 권한 설정
- [x] Docker 설치 및 기본 환경 확인
- [x] hello-world 컨테이너 실행
- [x] Ubuntu 컨테이너 생성
- [x] Docker 이미지 및 컨테이너 상태 확인
- [ ] Dockerfile을 이용한 커스텀 이미지 제작
- [ ] 포트 매핑 및 브라우저 접속 확인
- [ ] 바인드 마운트 데이터 변경 반영 확인
- [ ] Docker 볼륨 데이터 영속성 검증
- [ ] Git 저장소 초기화 및 커밋 실습
- [ ] GitHub 원격 저장소 연결 및 Push 확인

※ 실제 실습과 검증이 완료된 항목만 완료 처리한다.

---

## 4. Linux 터미널 및 권한 관리 실습

### 4.1 기본 명령어 실습

실습 경로:

`~/codyssey-practice/terminal-test`

| 명령어 | 기능 |
|---|---|
| pwd | 현재 작업 디렉터리 확인 |
| ls -la | 숨김 파일을 포함한 목록 확인 |
| cd | 디렉터리 이동 |
| mkdir | 디렉터리 생성 |
| touch | 파일 생성 |
| cp | 파일 복사 |
| mv | 파일 이동 및 이름 변경 |
| cat | 파일 내용 출력 |
| rm | 파일 삭제 |
| realpath | 절대 경로 확인 |

### 4.2 실제 실행 명령어

```bash
mkdir -p ~/codyssey-practice/terminal-test
cd ~/codyssey-practice/terminal-test

mkdir test-dir
echo "Hello Codyssey" > test.txt
touch .hidden-test

ls -la

cp test.txt copy.txt
mv copy.txt renamed.txt

cat test.txt
cat renamed.txt

rm renamed.txt

realpath test.txt
```

### 4.3 파일 및 디렉터리 권한 관리

Linux의 chmod 명령어를 이용하여 파일과 디렉터리의 권한을 설정하였다.

| 대상 | 변경 전 | 실행 명령 | 변경 후 |
|---|---|---|---|
| permission-test.txt | 644 (-rw-r--r--) | chmod 644 permission-test.txt | 644 (-rw-r--r--) |
| permission-dir | 755 (drwxr-xr-x) | chmod 755 permission-dir | 755 (drwxr-xr-x) |

※ 실습 당시 기본 생성 권한이 이미 644와 755였으므로 변경 전후 권한 비트에는 차이가 없었다.

권한 확인 명령어:

```bash
stat -c '%a %A %n' permission-test.txt permission-dir
find . -maxdepth 2 -printf '%M %p\n'
```

---

## 5. Docker 설치 및 컨테이너 운영

### 5.1 Docker 환경 확인

Docker Desktop과 WSL2 Ubuntu 간 연동 상태를 확인하였다.

```bash
docker --version
docker version
docker info
docker context show
docker images
docker ps -a
```

확인 결과:
- Docker Client 정상 작동
- Docker Engine 정상 연결
- Docker Context: default
- Client 및 Server OS/Arch: linux/amd64
- Docker Desktop 서버 연결 확인

### 5.2 hello-world 컨테이너 실행

```bash
docker run hello-world
```

실행 결과:
- hello-world 이미지 다운로드 확인
- 컨테이너 실행 성공
- "Hello from Docker!" 출력 확인

### 5.3 Ubuntu 컨테이너 생성

```bash
docker run -it --name codyssey-ubuntu ubuntu bash
```

Ubuntu 이미지를 기반으로 컨테이너를 생성하고 대화형 Bash 터미널에 접속하였다.

컨테이너 내부 확인 명령어:

```bash
pwd
ls -la
echo "Hello Codyssey Docker"
cat /etc/os-release
exit
```

※ 내부 명령 실행 결과는 추가 확인 후 기록한다.

### 5.4 Docker 이미지 및 컨테이너 관리

실제 확인된 이미지:

| 이미지 | 확인 결과 |
|---|---|
| hello-world:latest | 다운로드 및 실행 확인 |
| ubuntu:latest | 다운로드 및 컨테이너 생성 확인 |

컨테이너 확인 명령어:

```bash
docker images
docker ps
docker ps -a
docker logs codyssey-ubuntu
docker stats --no-stream
```

기존 Ubuntu 컨테이너 확인 결과:
- 컨테이너 이름: codyssey-ubuntu
- 이미지: ubuntu
- 실행 명령: bash
- 상태: Exited (0)

---

## 6. Dockerfile 기반 커스텀 이미지 제작

※ 실습 진행 예정

Dockerfile을 이용하여 기본 이미지를 확장하고 정적 웹 콘텐츠를 제공하는 커스텀 이미지를 제작한다.

### 6.1 빌드 및 실행 예시

```bash
docker build -t codyssey-web:v1 .

docker run -d \
  -p 8080:80 \
  --name codyssey-web \
  codyssey-web:v1
```

### 6.2 검증 방법

```bash
docker ps
curl http://localhost:8080
```

실제 Dockerfile 내용과 실행 결과는 실습 완료 후 추가한다.

---

## 7. 포트 매핑 및 네트워크 검증

※ 실습 진행 예정

호스트 컴퓨터의 포트와 컨테이너 내부 포트를 연결하여 웹 서비스 접속 여부를 검증한다.

예시:
- 호스트 포트: 8080
- 컨테이너 포트: 80
- 접속 주소: http://localhost:8080

실제 브라우저 접속 또는 curl 실행 결과를 확인한 후 증거 자료를 추가한다.

---

## 8. 데이터 영속성 검증

### 8.1 바인드 마운트 (Bind Mount)

※ 실습 진행 예정

호스트의 디렉터리와 컨테이너 내부 디렉터리를 연결하여 파일 변경 사항이 반영되는지 확인한다.

```bash
docker run -d \
  -p 8080:80 \
  -v "$(pwd)/html:/usr/share/nginx/html" \
  nginx:alpine
```

호스트에서 HTML 파일을 수정한 후 컨테이너 및 브라우저에서 변경 사항이 반영되는지 확인한다.

### 8.2 Docker 볼륨 (Volume)

※ 실습 진행 예정

Docker 볼륨을 생성하고 컨테이너에 연결하여 컨테이너 삭제 후에도 데이터가 유지되는지 확인한다.

```bash
docker volume create codyssey-data
docker volume inspect codyssey-data
```

검증 절차:
1. Docker 볼륨 생성
2. 컨테이너에 볼륨 연결
3. 볼륨 내부에 테스트 파일 생성
4. 기존 컨테이너 삭제
5. 새로운 컨테이너에 동일 볼륨 연결
6. 기존 데이터 유지 여부 확인

---

## 9. Git 및 GitHub 버전 관리

### 9.1 Git 환경 확인

```bash
git --version
git config --global user.name
git config --global init.defaultBranch
git status
git remote -v
```

현재 확인된 설정:

| 항목 | 확인 결과 |
|---|---|
| Git 버전 | 2.53.0 |
| 사용자 이름 | anki |
| 기본 브랜치 | main |
| 원격 저장소 | 추가 확인 필요 |

### 9.2 Git 기본 작업 흐름

```bash
git init
git add README.md
git commit -m "Initial commit"
git log --oneline
```

Git은 로컬 버전 관리 도구이며 GitHub은 원격 저장소 및 협업 플랫폼이다.

원격 저장소 연결이 확인된 후 업로드한다.

```bash
git remote -v
git push -u origin main
```

---

## 10. 트러블슈팅 (Troubleshooting)

### Case 01. Docker와 WSL2 연결 오류

**문제**

Docker 명령 실행 시 Docker Engine에 연결하지 못하는 오류가 발생하였다.

```text
failed to connect to the docker API
The system cannot find the file specified.
```

**원인 가설**

WSL2 내부에서 Docker Desktop의 CLI 연동 경로 및 Docker Engine 연결이 정상적으로 제공되지 않는 상태로 판단하였다.

**확인 과정**

```bash
docker context show
docker version
ls -l /usr/bin/docker
```

초기에는 Docker CLI의 연결 경로가 정상적으로 작동하지 않았으며, WSL Docker Desktop 연동 경로도 확인되지 않았다.

**해결 및 검증**

이후 Docker Desktop과 WSL2 연동이 복구된 상태에서 다음 결과를 확인하였다.

```text
Docker version 29.9.2
OS/Arch: linux/amd64
Server: Docker Desktop 4.98.0
```

※ 정확한 설정 변경 원인은 추가 확인이 필요하다.

### Case 02. Git 저장소 외부에서 commit 실행

**문제**

Git 저장소가 아닌 디렉터리에서 commit 명령어를 실행하여 오류가 발생하였다.

```text
fatal: not a git repository (or any of the parent directories): .git
```

**원인**

현재 작업 디렉터리가 Git 저장소로 초기화된 폴더 내부가 아니었다.

**해결 방법**

기존 저장소 위치를 확인한 후 해당 디렉터리로 이동하여 Git 명령어를 실행한다.

```bash
pwd
find ~ -maxdepth 5 -type d -name .git -print
cd 저장소_경로
git status
```

---

## 11. 학습 내용 및 결과

1. Linux 터미널의 기본 명령어를 사용하여 파일과 디렉터리를 생성, 복사, 이동 및 삭제할 수 있다.
2. chmod를 통해 파일과 디렉터리의 권한을 설정하고 확인할 수 있다.
3. Docker 이미지와 컨테이너의 차이 및 기본 실행 구조를 이해하였다.
4. Docker Client와 Server의 연결 상태를 확인하고 컨테이너를 관리하는 방법을 학습하였다.
5. Git은 로컬에서 변경 이력을 관리하며 GitHub은 원격 저장소로 활용된다는 점을 이해하였다.

※ 미완료 실습은 진행 후 결과를 추가한다.

---

## 12. 실습 증거 자료

실습 과정에서 생성한 스크린샷과 결과 자료를 아래 폴더에 저장한다.

```text
codisey1/
├── README.md
├── hello.py
└── images/
    ├── terminal-practice.png
    ├── docker-hello-world.png
    ├── docker-container.png
    ├── docker-web-browser.png
    └── github-commit.png
```

※ 실제로 저장한 이미지 파일만 README에 연결한다.
