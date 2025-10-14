# Firebase 설정 가이드

아바타 골프 연습 기록 웹사이트의 Firebase 설정을 위한 상세 가이드입니다.

## 1. Firebase 프로젝트 생성

### 1.1 Firebase 콘솔 접속
1. 웹 브라우저에서 [Firebase 콘솔](https://console.firebase.google.com/)에 접속
2. Google 계정으로 로그인

### 1.2 새 프로젝트 생성
1. "프로젝트 추가" 또는 "Create a project" 버튼 클릭
2. 프로젝트 이름 입력: `avatar-golf-tracker` (또는 원하는 이름)
3. "계속" 클릭
4. Google Analytics 설정:
   - **개발/테스트용**: "이 프로젝트에 Google Analytics 사용 설정" 체크 해제
   - **운영용**: Google Analytics 사용 설정 (선택사항)
5. "프로젝트 만들기" 클릭
6. 프로젝트 생성 완료까지 대기 (1-2분 소요)

## 2. Firestore 데이터베이스 설정

### 2.1 Firestore 활성화
1. Firebase 콘솔 좌측 메뉴에서 "Firestore Database" 클릭
2. "데이터베이스 만들기" 버튼 클릭

### 2.2 보안 규칙 설정
**개발/테스트용 설정:**
- "테스트 모드에서 시작" 선택
- "다음" 클릭

**운영용 설정 (나중에 변경):**
- "프로덕션 모드에서 시작" 선택
- 보안 규칙을 수동으로 설정

### 2.3 데이터베이스 위치 선택
- **권장 위치**: `asia-northeast3` (서울)
- 또는 가장 가까운 지역 선택
- "완료" 클릭

## 3. 웹 앱 등록

### 3.1 웹 앱 추가
1. Firebase 콘솔에서 프로젝트 개요 페이지로 이동
2. "웹" 아이콘 (`</>`) 클릭
3. 앱 닉네임 입력: `avatar-golf-web`
4. "Firebase Hosting 설정" 체크 해제 (선택사항)
5. "앱 등록" 클릭

### 3.2 Firebase SDK 설정 정보 복사
웹 앱 등록 후 나타나는 설정 객체를 복사합니다:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "avatar-golf-tracker.firebaseapp.com",
  projectId: "avatar-golf-tracker",
  storageBucket: "avatar-golf-tracker.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890abcdef"
};
```

## 4. 코드에 Firebase 설정 적용

### 4.1 index.html 파일 수정
1. 프로젝트의 `index.html` 파일을 텍스트 에디터로 열기
2. 다음 부분을 찾기:
```javascript
// Firebase 설정 (실제 프로젝트 설정으로 교체 필요)
const firebaseConfig = {
    apiKey: "your-api-key",
    authDomain: "your-project.firebaseapp.com",
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "123456789",
    appId: "your-app-id"
};
```

3. 위에서 복사한 실제 Firebase 설정으로 교체:
```javascript
const firebaseConfig = {
    apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    authDomain: "avatar-golf-tracker.firebaseapp.com",
    projectId: "avatar-golf-tracker",
    storageBucket: "avatar-golf-tracker.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:abcdef1234567890abcdef"
};
```

## 5. 보안 규칙 설정

### 5.1 개발/테스트용 보안 규칙
Firebase 콘솔 > Firestore Database > 규칙 탭에서:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

### 5.2 운영용 보안 규칙 (권장)
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // practiceRecords 컬렉션에 대한 제한적 접근
    match /practiceRecords/{document} {
      allow read, write: if true; // 필요시 인증 로직 추가
    }
  }
}
```

## 6. 테스트 및 확인

### 6.1 웹사이트 실행
1. `index.html` 파일을 웹 브라우저에서 열기
2. 또는 로컬 서버 실행:
```bash
# Python 3
python -m http.server 8000

# Node.js (http-server 설치 필요)
npx http-server

# PHP
php -S localhost:8000
```

### 6.2 데이터 저장 테스트
1. "연습기록" 탭에서 테스트 데이터 입력
2. "기록 저장" 버튼 클릭
3. Firebase 콘솔 > Firestore Database > 데이터 탭에서 데이터 확인

### 6.3 데이터 조회 테스트
1. "기록보기" 탭에서 저장된 데이터 확인
2. "분석" 탭에서 통계 데이터 확인

## 7. 문제 해결

### 7.1 일반적인 오류

**오류: "Firebase: Error (auth/api-key-not-valid)"**
- 해결: Firebase 설정의 apiKey가 올바른지 확인

**오류: "Firebase: Error (permission-denied)"**
- 해결: Firestore 보안 규칙이 올바르게 설정되었는지 확인

**오류: "Firebase: Error (project-not-found)"**
- 해결: projectId가 올바른지 확인

### 7.2 네트워크 오류
- 방화벽이나 프록시 설정 확인
- 브라우저 개발자 도구의 네트워크 탭에서 오류 확인

### 7.3 데이터가 저장되지 않는 경우
1. Firebase 콘솔에서 Firestore가 활성화되었는지 확인
2. 보안 규칙이 올바르게 설정되었는지 확인
3. 브라우저 콘솔에서 JavaScript 오류 확인

## 8. 운영 환경 배포

### 8.1 Firebase Hosting 사용 (권장)
```bash
# Firebase CLI 설치
npm install -g firebase-tools

# Firebase 로그인
firebase login

# 프로젝트 초기화
firebase init hosting

# 배포
firebase deploy
```

### 8.2 다른 호스팅 서비스 사용
- GitHub Pages
- Netlify
- Vercel
- AWS S3 + CloudFront

## 9. 백업 및 복구

### 9.1 데이터 백업
Firebase 콘솔 > Firestore Database > 데이터 탭에서 수동으로 데이터 내보내기

### 9.2 자동 백업 설정
Firebase 콘솔 > Firestore Database > 백업 탭에서 자동 백업 설정

## 10. 모니터링 및 분석

### 10.1 Firebase Analytics
- 사용자 행동 분석
- 성능 모니터링
- 오류 추적

### 10.2 Firestore 사용량 모니터링
- Firebase 콘솔 > 사용량 탭에서 읽기/쓰기 작업 모니터링
- 비용 최적화를 위한 쿼리 최적화

---

이 가이드를 따라하면 아바타 골프 연습 기록 웹사이트의 Firebase 설정이 완료됩니다. 추가 문의사항이 있으시면 언제든지 연락주세요!
