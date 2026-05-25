# @camellia/shared-types

CamelliaLetter 프로젝트의 공용 타입 및 스키마 패키지입니다.

## 📦 설치

### 로컬 개발 (file 의존성)

```bash
# api/package.json
{
  "dependencies": {
    "@camellia/shared-types": "file:../types"
  }
}
```

### GitHub Packages (프로덕션)

```bash
npm install @camellia/shared-types
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
import type { Invitation, InvitationBlock, UserRole } from '@camellia/shared-types';
```

### 스키마 import (런타임 검증)

```typescript
import { CreateInvitationDtoSchema, ErrorCode } from '@camellia/shared-types';

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
