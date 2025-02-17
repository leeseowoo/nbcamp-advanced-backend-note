병합 커밋을 남기지 않으려면 **merge 대신 rebase**를 사용한다. git rebase는 (feature) 브랜치의 커밋들을 (dev) 브랜치 위로 재배치하여 병합 커밋 없이 깔끔하게 변경 사항을 반영할 수 있다.
작업 중인 파일이 있을 경우 stash 후 최신 변경 사항을 가져와서 rebase 후 stash pop을 한다.
