cat << 'EOF' > create_readme.sh
#!/bin/bash

# README.md 파일 생성 시작
cat << 'OUTER_EOF' > README.md
# 🚀 개발 워크스테이션 구축 프로젝트 결과보고서

본 프로젝트는 효율적인 개발 환경을 위한 터미널 조작, 권한 설정, Docker 컨테이너 운영 및 Git/GitHub 연동 실습 과정을 기록한 문서입니다.

---

## 1. 프로젝트 개요
*   **미션 목표**: 리눅스 환경 숙달, Docker를 이용한 애플리케이션 컨테이너화, 데이터 영속성 확보 및 버전 관리 시스템 구축
*   **핵심 성과**: 커스텀 Docker 이미지 생성, 볼륨 마운트를 통한 데이터 보존, GitHub 연동 완료

## 2. 실행 환경
| 항목 | 상세 정보 |
| :--- | :--- |
| **OS / Shell** | $(uname -s) / $SHELL |
| **Terminal** | [예: VSCode Terminal / iTerm2] |
| **Docker 버전** | $(docker --version 2>/dev/null || echo "Docker not installed") |
| **Git 버전** | $(git --version 2>/dev/null || echo "Git not installed") |

---

## 3. 수행 항목 체크리스트
- [ ] **터미널 조작**: 기본 명령어(ls, cd, mkdir 등) 숙달 및 로그 기록
- [ ] **권한 관리**: 파일/디렉토리 권한 변경(chmod) 및 확인
- [ ] **Docker 설치**: 엔진 설치 및 \`hello-world\` 실행 확인
- [ ] **커스텀 이미지**: Dockerfile 작성 및 빌드/실행
- [ ] **네트워크/포트**: 호스트-컨테이너 포트 매핑 및 접속 확인
- [ ] **데이터 영속성**: 바인드 마운트 및 Docker 볼륨 검증
- [ ] **Git/GitHub**: 사용자 설정 및 VSCode 연동 완료

---

## 4. 터미널 조작 및 권한 실습 로그

### 4.1 기본 명령어 수행 (로그)
\`\`\`bash
# 현재 위치 확인 및 목록 확인
pwd
ls -al

# 디렉토리 생성 및 이동
mkdir workspace && cd workspace

# 파일 생성 및 복사/이름변경
touch test.txt
cp test.txt copy.txt
mv copy.txt renamed.txt

# 삭제
rm renamed.txt
\`\`\`

### 4.2 권한 실습 (chmod)
*   **변경 전/후 비교**:
| 대상 | 변경 전 | 실행 명령 | 변경 후 |
| :--- | :--- | :--- | :--- |
| 파일 (sample.txt) | \`-rw-r--r--\` | \`chmod 600 sample.txt\` | \`-rw-------\` |
| 디렉토리 (data/) | \`drwxr-xr-x\` | \`chmod 755 data/\` | \`drwxr-xr-x\` |

---

## 5. Docker 운영 및 컨테이너 실습

### 5.1 기본 점검 및 운영 로그
\`\`\`bash
# Docker 설치 확인
docker --version
docker info

# 이미지 및 컨테이너 상태 확인
docker images
docker ps -a
docker logs [컨테이너_ID]
docker stats --no-stream
\`\`\`

### 5.2 커스텀 이미지 제작 (Dockerfile)
*   **베이스 이미지**: [예: nginx:alpine]
*   **커스텀 포인트**: [예: index.html 교체, 환경변수 설정]
*   **빌드 및 실행 명령**:
\`\`\`bash
# 이미지 빌드
docker build -t 내이름/my-web-server:v1 .

# 컨테이너 실행 (포트 매핑 포함)
docker run -d -p 8080:80 --name my-web 내이름/my-web-server:v1
\`\`\`

### 5.3 포트 매핑 및 접속 증거
*   **접속 주소**: \`http://localhost:8080\`
*   **증거 사진**: ![접속결과](./images/browser_success.png) *(이미지 경로를 수정하세요)*

---

## 6. 데이터 영속성 검증 (Mount & Volume)

### 6.1 바인드 마운트 (Bind Mount)
*   **명령**: \`docker run -v \$(pwd)/html:/usr/share/nginx/html ...\`
*   **검증**: 호스트에서 \`index.html\` 수정 후 브라우저 새로고침 시 즉시 반영됨을 확인.

### 6.2 Docker 볼륨 (Volume)
*   **검증 절차**:
    1. 볼륨 생성: \`docker volume create my-data\`
    2. 컨테이너 연결 후 파일 생성
    3. 컨테이너 삭제 (\`docker rm -f\`) 후 새 컨테이너에서 동일 볼륨 연결
    4. 데이터가 그대로 남아있음을 확인
*   **로그**:
\`\`\`bash
docker volume inspect my-data
\`\`\`

---

## 7. Git & GitHub 연동
*   **Git 설정 확인**:
\`\`\`bash
git config --list | grep user
# user.name=홍길동
# user.email=gildong@example.com
\`\`\`
*   **연동 증거**: ![GitHub연동](./images/github_link.png) *(VSCode 연동 스크린샷 등)*

---

## 8. 트러블슈팅 (Troubleshooting)

### Case 01: Docker Permission Denied
*   **문제**: 일반 사용자로 docker 명령 실행 시 권한 에러 발생
*   **원인 가설**: 현재 사용자가 docker 그룹에 포함되지 않음
*   **해결**: \`sudo usermod -aG docker \$USER\` 실행 후 세션 재로그인

### Case 02: 포트 충돌 (Port Already Allocated)
*   **문제**: 8080 포트 사용 중으로 컨테이너 실행 실패
*   **원인 가설**: 기존에 실행 중인 다른 프로세스가 8080 사용 중
*   **해결**: \`lsof -i :8080\`으로 PID 확인 후 종료하거나 다른 포트(8081) 사용

---

## 9. 학습 요약 (Self-Reflection)
1.  **경로 차이**: 절대 경로는 \`/\`부터 시작하는 고유 주소, 상대 경로는 현재 위치 기준 주소임.
2.  **파일 권한**: \`755\`는 소유자(rwx), 그룹/기타(rx)를 의미함.
3.  **커스텀 이미지**: \`Dockerfile\`을 통해 필요한 패키지가 설치된 나만의 이미지를 자동화하여 만들 수 있음.
4.  **포트 매핑**: 외부(호스트) 포트와 내부(컨테이너) 포트를 연결해야 브라우저에서 접속 가능함.
5.  **Docker 볼륨**: 컨테이너가 삭제되어도 데이터를 영구적으로 보관하기 위해 필수적임.
6.  **Git vs GitHub**: Git은 로컬 도구, GitHub은 원격 저장소 및 협업 플랫폼임.

OUTER_EOF

echo "✅ README.md 파일이 생성되었습니다!"
EOF

# 스크립트 실행 권한 부여 및 실행
chmod +x create_readme.sh
./create_readme.sh