# Branch Strategy

## Branch 구조

```
main (배포용, 안정 버전)
  │
  └── develop (통합 브랜치)
        │
        ├── feature/1-username-sptBasic
        ├── feature/1-username-lazyLoading
        ├── fix/1-username-pageFaultCrash
        └── ...
```

## Branch Naming Convention

### 형식

```
<type>/<milestone#>-<github-username>-<description(camelCase)>
```

### Type

| Prefix | 용도 | 예시 |
|--------|------|------|
| `feature/` | 새 기능 개발 | `feature/1-username-sptInit` |
| `fix/` | 버그 수정 | `fix/1-username-nullPointer` |
| `refactor/` | 코드 리팩토링 | `refactor/1-username-vmCleanup` |
| `chore/` | CI, 설정, 빌드 등 | `chore/username-ciUpdate` |

### Milestone 번호

| # | Milestone |
|---|-----------|
| 1 | VM 기본 인프라 (P2 회귀 복구) |
| 2 | Memory Mapped Files 기본 |
| 3 | File-backed Page |
| 4 | Memory Mapped Files 완성 |
| 5 | Swap 인프라 |
| 6 | Swap 완성 |
| 7 | Copy-on-Write (Extra) |

### 예시

```bash
feature/1-username-sptBasic            # Milestone 1: SPT 기본 구조
feature/1-username-lazyLoading         # Milestone 1: Lazy loading
feature/1-username-setupStack          # Milestone 1: 스택 설정
feature/2-username-mmapSyscall         # Milestone 2: mmap 시스템콜
fix/1-username-frameAllocationBug      # Milestone 1 버그 수정
refactor/1-username-sptHashTable       # Milestone 1 리팩토링
chore/username-ciTestTimeout           # CI 설정 변경
```

## Branch 생성 기준

| 상황 | 새 브랜치? | 이유 |
|------|-----------|------|
| 새 함수/기능 구현 | ✅ Yes | 독립적인 작업 단위, PR 리뷰 필요 |
| 버그 수정 | ✅ Yes | 리뷰 필요, 롤백 용이 |
| 리팩토링 | ✅ Yes | 기존 기능에 영향 가능 |
| 작은 수정 (오타, 주석) | ❌ No | develop에 직접 커밋 |
| CI/설정 변경 | ✅ Yes | 전체 빌드에 영향 |

## Workflow

### 1. 브랜치 생성

```bash
git checkout develop
git pull origin develop
git checkout -b feature/1-username-sptBasic
```

### 2. 작업 및 커밋

```bash
# 작업 진행...
git add .
git commit -m "Add SPT hash table structure"
```

### 3. Push 및 PR 생성

```bash
git push origin feature/1-username-sptBasic
```

PR 생성 시 본문에 이슈 연결:
```markdown
## Summary
SPT 기본 구조 구현

closes #2
```

### 4. 리뷰 및 Merge

- PR 리뷰 후 `develop`에 merge
- merge 후 feature 브랜치 삭제

### 5. 주기적으로 develop → main

- Milestone 완료 시 `develop` → `main` merge

## 주의사항

- `main` 브랜치에 직접 push 금지
- `develop` 브랜치에 직접 push는 작은 수정만
- PR merge 전 CI 통과 확인
- 브랜치명은 영어, 하이픈(-) 사용 (설명은 camelCase)
