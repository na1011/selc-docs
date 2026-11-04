---
name: merge-main
description: 자기 작업 브랜치(work/<이름>)를 main에 fast-forward로 반영하고 GitHub에 올린다. "main에 반영", "머지", "푸시" 요청 때 쓴다.
---

# main 반영

`<이름>`은 지금 worktree 이름(= 팀원 이름)이다. 먼저 `git branch --show-current`가 `work/<이름>`인지 확인한다.

1. `git status`로 커밋 안 된 변경이 없는지 확인한다. 있으면 먼저 커밋할지 사용자에게 묻는다.
2. 자기 worktree에서 최신 main 위로 올린다.

   ```bash
   cd ~/wt/<이름>
   git rebase main
   ```

3. 통합 체크아웃에서 fast-forward로만 반영하고 GitHub에 올린다.

   ```bash
   git -C ~/selc-docs merge --ff-only work/<이름>
   git -C ~/selc-docs push origin main
   ```

   - `--ff-only`가 실패하면, 그 사이 다른 사람이 먼저 반영한 것이다. 2부터 다시 한다.
   - `index.lock` 오류가 나면 잠깐 뒤에 다시 실행한다. lock 파일은 지우지 않는다.

## rebase 중 충돌

- 충돌은 자기 worktree 안에서만 푼다. 양쪽 수정의 의도를 모두 살리는 것이 원칙이다.
- 상대 쪽 변경의 의도를 모르면 `git rebase --abort`로 되돌린 뒤, 그 부분을 쓴 사람에게 확인한다.
- 문서 내용끼리 모순되면 한쪽을 골라 지우지 말고 두 내용을 함께 남겨 보고한다(CLAUDE.md 「작업과 커밋」).
