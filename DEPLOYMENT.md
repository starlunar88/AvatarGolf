# 배포 가이드

이 문서는 Avatar Golf Practice Tracker를 다양한 플랫폼에 배포하는 방법을 설명합니다.

## 🚀 배포 옵션

### 1. Firebase Hosting (추천)

현재 프로젝트는 Firebase를 사용하므로 Firebase Hosting이 가장 적합합니다.

#### 사전 준비
1. Firebase CLI 설치
   ```bash
   npm install -g firebase-tools
   ```

2. Firebase 로그인
   ```bash
   firebase login
   ```

#### 배포 단계
1. **Firebase 프로젝트 초기화**
   ```bash
   firebase init hosting
   ```
   
   설정 옵션:
   - Use an existing project: `avatargolf-eb431`
   - Public directory: `.`
   - Configure as single-page app: `No`
   - Set up automatic builds: `Yes`

2. **수동 배포**
   ```bash
   firebase deploy
   ```

3. **자동 배포 설정 (GitHub Actions)**
   - Firebase 토큰 생성:
     ```bash
     firebase login:ci
     ```
   - GitHub 저장소 Settings → Secrets → Actions에서 `FIREBASE_TOKEN` 추가
   - main 브랜치에 push할 때마다 자동 배포

#### 배포 URL
- 프로덕션: `https://avatargolf-eb431.web.app`
- 미리보기: `https://avatargolf-eb431.firebaseapp.com`

### 2. Netlify

#### 배포 단계
1. [netlify.com](https://netlify.com) 가입
2. "New site from Git" 클릭
3. GitHub 저장소 연결
4. Build settings:
   - Build command: (비워두기)
   - Publish directory: `/`
5. Deploy site 클릭

#### 환경 변수 설정
- Firebase 설정이 이미 코드에 포함되어 있어 추가 설정 불필요

### 3. Vercel

#### 배포 단계
1. [vercel.com](https://vercel.com) 가입
2. "New Project" 클릭
3. GitHub 저장소 선택
4. Framework Preset: "Other"
5. Deploy 클릭

### 4. GitHub Pages

#### 제한사항
- Private 저장소는 유료 플랜 필요
- Firebase 설정이 노출될 수 있음

#### 배포 단계
1. GitHub 저장소 Settings → Pages
2. Source: "Deploy from a branch"
3. Branch: "main"
4. Folder: "/ (root)"

## 🔧 배포 전 체크리스트

### 필수 확인사항
- [ ] Firebase 프로젝트 설정이 올바른지 확인
- [ ] Firestore 보안 규칙 설정 확인
- [ ] 모든 기능이 정상 작동하는지 테스트
- [ ] 모바일 반응형 디자인 확인

### 보안 설정
1. **Firestore 보안 규칙** (운영 환경용)
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /practiceRecords/{document} {
         allow read, write: if true; // 개발용
         // 운영시 인증 추가 필요
       }
     }
   }
   ```

2. **환경 변수 관리**
   - Firebase API 키는 이미 코드에 포함되어 있음
   - 민감한 정보는 환경 변수로 관리 권장

## 📱 배포 후 확인사항

### 기능 테스트
- [ ] 연습 기록 입력/저장
- [ ] 기록 조회 및 필터링
- [ ] 분석 데이터 표시
- [ ] 멤버 관리 기능
- [ ] 반응형 디자인 (모바일/태블릿/PC)

### 성능 확인
- [ ] 페이지 로딩 속도
- [ ] Firebase 연결 상태
- [ ] 오프라인 동작 (PWA 기능 추가시)

## 🔄 업데이트 배포

### 자동 배포 (GitHub Actions)
- main 브랜치에 push하면 자동으로 배포됨
- Pull Request 생성시 미리보기 배포

### 수동 배포
```bash
# Firebase Hosting
firebase deploy

# 특정 기능만 배포
firebase deploy --only hosting
```

## 🆘 문제 해결

### 일반적인 문제
1. **Firebase 연결 오류**
   - Firebase 프로젝트 설정 확인
   - Firestore 보안 규칙 확인

2. **배포 실패**
   - 빌드 로그 확인
   - 환경 변수 설정 확인

3. **CORS 오류**
   - Firebase 설정에서 도메인 허용 확인

### 지원
- GitHub Issues를 통해 문제 리포트
- Firebase 콘솔에서 실시간 모니터링

---

**배포 완료 후 웹사이트 URL을 팀원들과 공유하세요!** 🎉
