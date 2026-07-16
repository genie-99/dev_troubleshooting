# 윈도우 자격 증명으로 GitHub 인증 실패

## 코드
```bash
git clone https://github.com/owner/repository.git
```

## 오류 코드
```text
fatal: unable to access 'https://github.com/owner/repository.git':
The requested URL returned error: 403
```

## 해결 방법
1. Windows 자격 증명 관리자에서 GitHub 관련 저장된 자격 증명을 확인합니다.
2. 잘못된 또는 오래된 GitHub 자격 증명을 삭제합니다.
3. 다시 `git clone`을 실행하면 새로 인증을 시도하게 됩니다.

## 해결 완료
Windows에 저장된 GitHub 자격 증명을 삭제한 뒤 다시 인증하면 `git clone`이 정상적으로 진행되었습니다.

## 정리
- GitHub 인증이 계속 실패하면 Windows 자격 증명에 오래된 정보가 남아 있을 수 있습니다.
- 자격 증명을 지운 뒤 다시 인증하면 대부분 해결됩니다.
- `git` 작업 전에는 저장된 인증 정보가 올바른지 확인하는 것이 좋습니다.
