# @camellia-letter/shared-types

CamelliaLetter 프로젝트의 공용 타입 및 스키마 패키지입니다.

## 📦 설치

### 로컬 개발 (file 의존성)

```bash
# api/package.json
{
  "dependencies": {
    "@camellia-letter/shared-types": "file:../types"
  }
}
```

### GitHub Packages (프로덕션)

#### 1. GitHub Personal Access Token 생성

1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. `read:packages` 권한 선택
3. 토큰 생성 및 복사

#### 2. .npmrc 파일 설정

프로젝트 루트에 `.npmrc` 파일을 생성하거나 `.npmrc.example`을 복사하여 수정:

```bash
# .npmrc
//npm.pkg.github.com/:_authToken=YOUR_GITHUB_PERSONAL_ACCESS_TOKEN
@camellia-letter:registry=https://npm.pkg.github.com
```

**주의**: `.npmrc` 파일은 절대 Git에 커밋하지 마세요. 이미 `.gitignore`에 포함되어 있습니다.

#### 3. 패키지 설치

```bash
npm install @camellia-letter/shared-types
```

## 🎯 포함된 타입

- **Common**: 페이지네이션 등 공통 타입
- **Error**: API 에러 응답 및 에러 코드
- **Guestbook**: 방명록 타입
- **Invitation**: 청첩장 및 블록 타입
- **Print Invitation**: 실물 청첩장 PDF 설정
- **RSVP**: 참석 여부 응답 타입
- **User**: 사용자 및 세션 타입
- **Schemas**: Zod 유효성 검증 스키마

## 📖 사용법

### 타입 import

```typescript
import type { Invitation, InvitationBlock, UserRole } from '@camellia-letter/shared-types';
```

### 스키마 import (런타임 검증)

```typescript
import { CreateInvitationDtoSchema, ErrorCode } from '@camellia-letter/shared-types';

// Zod 검증
const result = CreateInvitationDtoSchema.parse(data);
```

## 🛠️ 개발

### 빌드

```bash
npm run build
```

### 타입체크

```bash
npm run typecheck
```

### 클린

```bash
npm run clean
```

## 📝 버전 관리

- **patch**: 타입 설명, 내부 정리, 호환 변경
- **minor**: 새 타입/필드 추가
- **major**: 필드 제거, 이름 변경, enum 값 제거 등 호환성 깨짐

## 🔗 관련 프로젝트

- [api](https://github.com/camellia-letter/api)
- [web](https://github.com/camellia-letter/web)
- [admin](https://github.com/camellia-letter/admin)
