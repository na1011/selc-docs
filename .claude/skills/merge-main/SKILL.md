---
name: merge-main
description: 지금 worktree 브랜치를 GitHub main에 fast-forward로 반영한다. "main에 반영", "머지", "푸시" 요청 때 쓴다.
---

# main 반영

git 명령은 모두 자기 worktree에서 실행한다.

1. `git status`로 커밋 안 된 변경이 없는지 확인한다. 있으면 먼저 커밋할지 사용자에게 묻는다.
2. 최신 main 위로 올린다.

   ```bash
   git fetch origin
   git rebase origin/main
   ```

3. GitHub main에 올린다. fast-forward가 아니면 GitHub이 거절하므로, 이것이 곧 fast-forward 전용 반영이다.

   ```bash
   git push origin HEAD:main
   ```

   - `rejected (non-fast-forward)`로 거절되면 그 사이 다른 세션이 먼저 반영한 것이다. 2부터 다시 한다.
   - `index.lock` 오류가 나면 잠깐 뒤에 다시 실행한다. lock 파일은 지우지 않는다.

4. 통합 체크아웃(`~/selc-docs`)은 갱신하지 않는다. push가 성공하면 이 맥의 `origin/main`이 바로 바뀌고, 새 세션의 worktree는 `origin/main`에서 만들어진다.

## rebase 중 충돌

- 충돌은 자기 worktree 안에서만 푼다. 양쪽 수정의 의도를 모두 살리는 것이 원칙이다.
- 상대 쪽 변경의 의도를 모르면 `git rebase --abort`로 되돌린 뒤, 그 부분을 쓴 사람에게 확인한다.
- 문서 내용끼리 모순되면 한쪽을 골라 지우지 말고 두 내용을 함께 남겨 보고한다(CLAUDE.md 「작업과 커밋」).
