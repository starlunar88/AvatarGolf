# Avatar Golf Practice Tracker

아바타 골프 연습 기록 관리 웹 애플리케이션입니다. 골프 연습 기록을 체계적으로 관리하고 분석할 수 있는 반응형 웹사이트입니다.

## 주요 기능

### 🏌️ 골프 연습 기록
- **연습자 선택**: 오세민, 조재현
- **클럽 선택**: 드라이버부터 퍼터까지 모든 클럽 지원
- **연습 유형**:
  - 정확도 연습: 성공/실패 기록 (예: 7번 아이언 30m 10회중 6번 성공)
  - 거리 연습: 정확한 거리 측정 (예: 퍼터 5m 10회 기록)

### 📊 기록 분석
- 전체 연습 통계
- 클럽별 성과 분석
- 개인별 진보 추이
- 성공률 및 거리 정확도 분석

### 👥 멤버 관리
- 연습생별 개별 통계
- 총 연습 횟수 및 성공률 추적

### 📱 반응형 디자인
- 모바일, 태블릿, PC 모든 기기 지원
- 터치 친화적 인터페이스
- 다크 모드 지원

## 기술 스택

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Database**: Firebase Firestore
- **Styling**: CSS Grid, Flexbox, CSS Variables
- **Icons**: Font Awesome
- **Fonts**: Noto Sans KR

## 설치 및 설정

### 1. 프로젝트 클론
```bash
git clone [repository-url]
cd AvatarGolfKing
```

### 2. Firebase 프로젝트 설정

#### 2.1 Firebase 콘솔에서 프로젝트 생성
1. [Firebase 콘솔](https://console.firebase.google.com/)에 접속
2. "프로젝트 추가" 클릭
3. 프로젝트 이름 입력 (예: avatar-golf-tracker)
4. Google Analytics 설정 (선택사항)

#### 2.2 Firestore 데이터베이스 설정
1. Firebase 콘솔에서 "Firestore Database" 선택
2. "데이터베이스 만들기" 클릭
3. 보안 규칙을 "테스트 모드"로 설정 (개발용)
4. 위치 선택 (asia-northeast3 권장)

#### 2.3 웹 앱 등록
1. Firebase 콘솔에서 "프로젝트 설정" > "일반" 탭
2. "내 앱" 섹션에서 웹 아이콘 선택
3. 앱 닉네임 입력 (예: avatar-golf-web)
4. "앱 등록" 클릭

#### 2.4 Firebase 설정 정보 복사
웹 앱 등록 후 나타나는 Firebase 설정 객체를 복사합니다:

```javascript
const firebaseConfig = {
  apiKey: "your-api-key",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "your-app-id"
};
```

#### 2.5 코드에 Firebase 설정 적용
`index.html` 파일의 Firebase 설정 부분을 찾아서 위에서 복사한 설정으로 교체합니다:

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

### 3. 보안 규칙 설정 (운영 환경용)

개발 완료 후 Firestore 보안 규칙을 다음과 같이 설정하세요:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // practiceRecords 컬렉션에 대한 읽기/쓰기 허용
    match /practiceRecords/{document} {
      allow read, write: if true; // 개발용 - 운영시 인증 추가 필요
    }
  }
}
```

## 사용 방법

### 1. 연습 기록 입력
1. "연습기록" 탭 선택
2. 연습자 선택 (오세민 또는 조재현)
3. 사용한 클럽 선택
4. 연습 거리 입력 (미터 단위)
5. 연습 유형 선택:
   - **정확도 연습**: 총 시도 횟수와 성공 횟수 입력
   - **거리 연습**: 시도 횟수와 각 시도별 거리 입력
6. 메모 입력 (선택사항)
7. "기록 저장" 버튼 클릭

### 2. 기록 조회
1. "기록보기" 탭 선택
2. 필터를 사용하여 원하는 기록 검색:
   - 연습자별 필터링
   - 클럽별 필터링
   - 날짜별 필터링

### 3. 분석 확인
1. "분석" 탭 선택
2. 전체 통계, 클럽별 성과, 진보 추이 확인

### 4. 멤버 관리
1. "멤버관리" 탭 선택
2. 각 멤버의 연습 통계 확인

## 데이터 구조

### practiceRecords 컬렉션
```javascript
{
  user: "오세민" | "조재현",
  club: "드라이버" | "3번우드" | ... | "퍼터",
  distance: number, // 미터 단위
  practiceType: "accuracy" | "distance",
  timestamp: Timestamp,
  date: string, // YYYY-MM-DD 형식
  notes: string, // 선택사항
  
  // accuracy 타입인 경우
  totalAttempts?: number,
  successCount?: number,
  successRate?: number,
  
  // distance 타입인 경우
  attemptCount?: number,
  distances?: number[],
  averageDistance?: number,
  minDistance?: number,
  maxDistance?: number
}
```

## 브라우저 지원

- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

## 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다.

## 기여하기

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 문의사항

프로젝트에 대한 문의사항이나 버그 리포트는 GitHub Issues를 통해 제출해주세요.

---

**Avatar Golf Practice Tracker v1.0.0**  
골프 연습을 더 체계적으로, 더 효과적으로!
