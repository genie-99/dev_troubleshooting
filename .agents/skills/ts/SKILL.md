---
name: ts
description: "Create and publish a Korean troubleshooting note for a problem, bug, or error. Use when the user invokes `$ts` or asks to record a troubleshooting result: add a new Markdown file in the appropriate topic folder with a short title, code, error, solution, `해결 완료`, and summary; then commit only the note-related changes and push them to the configured upstream."
---

# 문제 해결 노트 작성 도우미

이 스킬은 문제를 겪었을 때, 한국어 제목과 한국어 파일명으로 노트를 정리할 때 사용합니다.

## Instructions
1. 제목과 파일명은 한국어로 짧고 명확하게 작성합니다.
2. 문제 상황은 무엇이 발생했는지, 왜 헷갈렸는지, 어떤 맥락에서 생겼는지까지 조금 더 자세하게 설명합니다.
3. **중요: 여러 개의 오류가 있으면 각각을 분리해서 별도의 markdown 파일로 만듭니다.**
   - 각 오류마다 독립적인 파일 생성
   - 하나의 파일에는 하나의 오류/문제에만 집중
4. 코드와 오류 코드를 서로 다른 섹션으로 나눕니다.
5. 해결 방법은 3개 이하로 간단히 정리합니다.
6. 문서 끝에 "해결 완료" 섹션을 넣습니다.
7. 마지막에 짧은 정리를 추가합니다.
8. 주제에 맞는 폴더에 markdown 파일을 생성합니다.
9. 기존 노트가 있더라도 덮어쓰지 말고, 새 파일로 추가합니다.
10. 모든 파일 생성이 완료되면 노트와 노트 작성에 필요한 설정 변경만 커밋합니다.
11. 명시적인 `$ts` 호출은 해당 커밋을 기본 upstream에 푸시할 권한으로 처리합니다. 사용자가 초안, 푸시하지 않음, 또는 검토만을 요청하면 커밋과 푸시를 건너뜁니다.
12. 주제에 맞는 폴더가 없다면 새롭게 폴더를 추가합니다.

## Publish Workflow

1. 저장소 루트, 현재 브랜치, 원격, 기존 변경사항을 확인합니다.
2. 새 노트와 노트 작성에 필요한 스킬 또는 분류 설정만 스테이징합니다. 사용자의 관련 없는 변경사항은 절대 포함하지 않습니다.
3. 스테이징된 diff를 확인한 뒤 의미 있는 커밋 메시지로 커밋합니다.
4. 기본 upstream에 푸시하고, 커밋 해시와 원격 브랜치를 보고합니다.

## Template
```md
# 제목

## 코드
- 

## 오류 코드
- 

## 해결 방법
1. 
2. 
3. 

## 해결 완료

## 정리
- 
```

## Notes
- 파일명은 한국어로 쓰고, 주제와 내용을 바로 알 수 있게 합니다.
- 내용은 실용적이고 나중에 검색하기 쉽게 작성합니다.
- **여러 오류 분리 예시:**
  - 하나의 상황에서 3가지 오류 발생 → 3개의 독립적인 파일 생성
  - 예: Vim 탈출 문제 / 코드 충돌 문제 / 변수 중복 선언 → 각각 별도 파일
- 커밋 메시지는 간단하고 의미 있게 작성합니다.
- 명시적인 `$ts` 호출에서는 기록·커밋·푸시까지 완료합니다. 초안 또는 푸시 제외 요청이 있을 때만 중단합니다.
