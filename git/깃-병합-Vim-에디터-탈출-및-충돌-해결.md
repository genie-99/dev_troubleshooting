# 깃 병합 시 Vim 에디터 탈출 및 코드 충돌 해결

## 문제 상황

로컬 브랜치(sua)에 원격 main 브랜치를 병합하기 위해 `git pull origin main`을 실행했을 때, 갑자기 터미널에 영어가 가득한 Vim 편집기 화면이 떠서 멈추는 현상이 발생했습니다. Vim 편집기가 무엇인지 몰라 당황했으며, 이후 코드 충돌(Conflict)도 발생하여 BoardView.vue와 HomeView.vue 파일에서 추가 문제가 생겼습니다.

## 코드

**충돌이 발생했던 위치:**
```vue
// BoardView.vue, HomeView.vue에서 발생
const posts = ref([])  // 구버전 코드
const posts = ref([])  // 새 버전 코드 (중복)
```

## 오류 코드

- `Identifier 'posts' has already been declared` - 변수가 이중으로 선언됨
- Vim 편집기 화면이 뜨면서 터미널이 응답하지 않는 상태

## 해결 방법

1. **Vim 편집기 안전 탈출**
   - `ESC` 키 누르기 → `:` 입력 → `wq` 입력 → `Enter` 키 입력
   - 이것은 Git의 정상적인 병합 절차이며, Git이 합치기 기록의 이름을 정하기 위해 메모장을 띄우는 것입니다.

2. **코드 충돌 정리**
   - VS Code의 코드 편집기에서 충돌 부분을 확인하고 불필요한 구버전 변수 코드 삭제
   - 최신 버전의 코드만 남기기

3. **변경사항 커밋 및 푸시**
   - 충돌을 해결한 후 `git add .` → `git commit` → `git push origin sua` 실행

## 해결 완료

Vim 편집기 탈출 공식을 습득했고, 코드 충돌을 정리하여 최종적으로 로컬 브랜치에 main 병합을 성공적으로 완료했습니다.

## 정리

- **git pull의 범위**: 로컬 컴퓨터에서만 일어나므로, 원격 저장소에도 적용하려면 `git push origin [브랜치명]` 필수
- **Vim 편집기 탈출 공식**: ESC → : → wq → Enter
- **Git 병합 시 중요**: 충돌 시 불필요한 중복 코드를 정확히 제거하고, 최종 커밋 전에 코드 동작 확인 필요
