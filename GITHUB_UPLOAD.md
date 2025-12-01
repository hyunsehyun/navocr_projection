# GitHub 업로드 가이드

## 1. GitHub에서 새 저장소 생성

1. https://github.com/new 방문
2. Repository name: `navocr_projection` (또는 원하는 이름)
3. Description: `NavOCR 3D SLAM projection with rtabmap integration`
4. Public/Private 선택
5. **"Add a README file" 체크 해제** (이미 있음)
6. **"Add .gitignore" 선택 안 함** (이미 있음)
7. "Create repository" 클릭

## 2. 로컬 저장소와 연결

GitHub에서 저장소를 생성한 후, 다음 명령어를 실행하세요:

```bash
cd ~/ros2_ws/src/navocr_projection

# GitHub 저장소 URL로 변경 (예시)
git remote add origin https://github.com/YOUR_USERNAME/navocr_projection.git

# 또는 SSH 사용 시
# git remote add origin git@github.com:YOUR_USERNAME/navocr_projection.git

# 푸시
git push -u origin main
```

## 3. 인증

### HTTPS 사용 시:
- Username과 Personal Access Token 입력 필요
- Token 생성: https://github.com/settings/tokens

### SSH 사용 시 (권장):
```bash
# SSH 키 생성 (이미 있으면 생략)
ssh-keygen -t ed25519 -C "your_email@example.com"

# 공개 키 복사
cat ~/.ssh/id_ed25519.pub

# GitHub Settings > SSH and GPG keys에 추가
```

## 4. 완료 확인

푸시 후 GitHub 저장소 페이지에서 다음을 확인:
- ✅ README.md가 표시됨
- ✅ 10개 파일이 업로드됨
- ✅ 초기 커밋 메시지가 보임

## 5. 추가 작업 (선택)

### .github/workflows/ci.yml 추가 (CI/CD)
자동 빌드 테스트를 원하면 나중에 추가 가능

### LICENSE 파일 추가
```bash
# BSD-3-Clause 라이센스 추가 (예시)
```

### 배지 추가 (README에)
- Build status
- ROS version
- License

---

## 현재 저장소 정보

- Branch: main
- Commits: 1
- Files: 10
- Latest commit: 49a307d

## 필요 시 추가 명령어

```bash
# 원격 저장소 확인
git remote -v

# 푸시 전 상태 확인
git log --oneline

# 강제 푸시 (주의!)
git push -f origin main
```
