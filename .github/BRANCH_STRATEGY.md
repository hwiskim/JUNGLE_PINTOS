# Branch Strategy

## 핵심 원칙

- **Milestone 1개 = Branch 1개 = PR 1개**
- 오프라인 코드리뷰 부담 최소화
- 커밋 단위로 작업 구분

## Branch 구조

```
main (배포용, 안정 버전)
  │
  └── develop (통합 브랜치)
        │
        ├── milestone/1-vmInfra
        ├── milestone/2-mmapBasic
        ├── milestone/3-fileBacked
        └── ...
```

## Branch Naming Convention

### 형식

```
milestone/<milestone#>-<description(camelCase)>
```

### Milestone 목록

| # | Milestone | 브랜치명 |
|---|-----------|----------|
| 1 | VM 기본 인프라 (P2 회귀 복구) | `milestone/1-vmInfra` |
| 2 | Memory Mapped Files 기본 | `milestone/2-mmapBasic` |
| 3 | File-backed Page | `milestone/3-fileBacked` |
| 4 | Memory Mapped Files 완성 | `milestone/4-mmapComplete` |
| 5 | Swap 인프라 | `milestone/5-swapInfra` |
| 6 | Swap 완성 | `milestone/6-swapComplete` |
| 7 | Copy-on-Write (Extra) | `milestone/7-cow` |

## Commit Convention

브랜치 대신 **커밋 헤더**로 작업 유형 구분

### 형식

```
<type>: <description>
```

### Type

| Type | 용도 | 예시 |
|------|------|------|
| `feat` | 새 기능 구현 | `feat: Add SPT hash table structure` |
| `fix` | 버그 수정 | `fix: Fix null pointer in page fault handler` |
| `refactor` | 코드 리팩토링 | `refactor: Simplify frame allocation logic` |
| `chore` | 빌드, 설정 등 | `chore: Update Makefile for VM build` |
| `docs` | 문서 수정 | `docs: Add SPT design notes` |
| `test` | 테스트 관련 | `test: Add unit test for spt_find_page` |

### Milestone 1 커밋 예시

```bash
feat: Add hash_elem to struct page
feat: Implement page_hash and page_less
feat: Implement supplemental_page_table_init
feat: Implement spt_find_page and spt_insert_page
feat: Add struct frame and vm_get_frame
feat: Implement vm_alloc_page_with_initializer
feat: Implement vm_do_claim_page and vm_claim_page
feat: Implement lazy_load_segment
feat: Implement vm_try_handle_fault
feat: Implement setup_stack with VM system
feat: Implement uninit_destroy and anon_destroy
feat: Implement supplemental_page_table_copy
feat: Implement supplemental_page_table_kill
fix: Fix page alignment issue in spt_find_page
```

## Workflow

### 1. Milestone 브랜치 생성

```bash
git checkout develop
git pull origin develop
git checkout -b milestone/1-vmInfra
```

### 2. 작업 및 커밋 (Phase별)

```bash
# Phase 1-2: SPT 구조체 및 자료구조
git add .
git commit -m "feat: Add hash_elem to struct page"
git commit -m "feat: Implement page_hash and page_less"
git commit -m "feat: Implement supplemental_page_table_init"

# Phase 3: SPT 기본 연산
git commit -m "feat: Implement spt_find_page"
git commit -m "feat: Implement spt_insert_page"

# ... 계속
```

### 3. Push 및 PR 생성

```bash
git push origin milestone/1-vmInfra
```

PR 생성 시:
```markdown
## Summary
Milestone 1: VM 기본 인프라 구현

- SPT 자료구조 (해시 테이블)
- 프레임 관리
- Lazy loading
- Page fault 처리
- 스택 설정
- 페이지 정리 (fork/exit)

closes #2
```

### 4. 오프라인 코드리뷰

- Milestone 완료 시 팀 전체 오프라인 리뷰
- 리뷰 후 `develop`에 merge

### 5. develop → main

- Milestone merge 후 테스트 통과 확인
- 안정화되면 `main`에 merge

## 주의사항

- `main` 브랜치에 직접 push 금지
- `develop` 브랜치에 직접 push는 작은 수정(오타, 주석)만
- PR merge 전 CI 통과 확인
- 커밋 메시지는 영어로 작성
