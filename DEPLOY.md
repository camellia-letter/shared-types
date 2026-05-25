# @camellia/shared-types 배포 가이드

## 📋 배포 준비

### 1. GitHub Personal Access Token 생성

1. GitHub 설정 → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. **Generate new token (classic)** 클릭
3. 권한 선택:
   - ✅ `write:packages` (패키지 업로드)
   - ✅ `read:packages` (패키지 다운로드)
   - ✅ `delete:packages` (패키지 삭제 - 선택)
4. 토큰 복사 (한 번만 표시됨!)

### 2. .npmrc 파일 생성

`.npmrc.example` 파일을 복사하여 `.npmrc` 파일 생성:

```bash
cp .npmrc.example .npmrc
```

`.npmrc` 파일을 열어 토큰 입력:

```
//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN
@camellia:registry=https://npm.pkg.github.com
```

`YOUR_GITHUB_TOKEN`을 실제 생성한 토큰으로 교체합니다.

**⚠️ 중요**: `.npmrc` 파일은 `.gitignore`에 추가되어 있어 커밋되지 않습니다.

---

## 🚀 패키지 배포

### 버전 업데이트

배포 전 버전을 업데이트합니다:

```bash
# patch (0.1.0 -> 0.1.1) - 버그 수정, 타입 설명 추가
npm version patch

# minor (0.1.0 -> 0.2.0) - 새 타입/필드 추가
npm version minor

# major (0.1.0 -> 1.0.0) - 호환성 깨지는 변경
npm version major
```

### 빌드 및 배포

```bash
# 빌드 (prepublishOnly로 자동 실행됨)
npm run build

# 배포 (GitHub Packages로)
npm publish
```

### 배포 확인

1. GitHub 저장소 → **Packages** 탭
2. `@camellia/shared-types` 패키지 확인
3. 버전 확인

---

## 📦 API 프로젝트에서 사용

### 1. api 프로젝트에 .npmrc 설정

`api/.npmrc` 파일 생성:

```
//npm.pkg.github.com/:_authToken=${GITHUB_TOKEN}
@camellia:registry=https://npm.pkg.github.com
```

환경변수를 사용하여 토큰을 주입합니다.

### 2. package.json 수정

```json
{
  "dependencies": {
    "@camellia/shared-types": "^0.1.0"
  }
}
```

### 3. 설치

```bash
# 로컬 개발
export GITHUB_TOKEN=your_token_here
npm install

# CloudType/프로덕션
# 환경변수 GITHUB_TOKEN 설정 필요
```

### 4. CloudType 환경변수 설정

CloudType 대시보드에서:
- 변수명: `GITHUB_TOKEN`
- 값: GitHub Personal Access Token

---

## 🔄 배포 자동화 (선택)

`.github/workflows/publish-types.yml` 생성:

```yaml
name: Publish Types Package

on:
  push:
    branches: [main]
    paths:
      - 'types/**'

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          registry-url: 'https://npm.pkg.github.com'

      - name: Install dependencies
        working-directory: ./types
        run: npm ci

      - name: Build
        working-directory: ./types
        run: npm run build

      - name: Publish to GitHub Packages
        working-directory: ./types
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## ❓ 문제 해결

### 401 Unauthorized

- GitHub Token 권한 확인 (`write:packages`)
- `.npmrc` 파일 형식 확인
- 토큰 만료 여부 확인

### 404 Package not found

- `publishConfig.registry` 확인
- 패키지 이름에 스코프(`@camellia/`) 포함 확인
- GitHub 저장소 접근 권한 확인

### You cannot publish over the previously published versions

- `package.json`의 버전 업데이트 필요
- `npm version patch/minor/major` 실행

### Module not found 에러 (API 배포 시)

- CloudType 환경변수에 `GITHUB_TOKEN` 설정 확인
- `api/.npmrc` 파일 존재 확인
- 패키지 버전이 올바른지 확인

---

## 📝 체크리스트

### 배포 전
- [ ] GitHub Personal Access Token 생성
- [ ] `.npmrc` 파일 생성 및 토큰 입력
- [ ] `npm run build` 정상 동작 확인
- [ ] `npm version` 으로 버전 업데이트
- [ ] `npm publish` 실행

### 배포 후
- [ ] GitHub Packages에서 패키지 확인
- [ ] API 프로젝트 `.npmrc` 설정
- [ ] API 프로젝트 `package.json` 수정
- [ ] CloudType 환경변수 `GITHUB_TOKEN` 추가
- [ ] API 재배포 및 성공 확인

---

## 💡 대안: npm public registry

GitHub Packages 대신 npm 공개 레지스트리 사용 시:

1. npm 계정 생성 (https://www.npmjs.com)
2. `npm login` 실행
3. `package.json`에서 `publishConfig` 제거
4. `npm publish --access public` 실행

단, 공개 레지스트리는 패키지가 전 세계에 공개됩니다.
