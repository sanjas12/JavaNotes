
`git merge <из какой ветки> 
							`--squash` -> объединяя все коммиты в один. Это полезно, если вы хотите сохранить историю более чистой и избежать создания множества мелких коммитов.
							`--no-commit` → обычный merge, просто Git ждёт твоего подтверждения (`git commit`) или отмены (`git merge --abort`). Если `git merge --abort` не сработал -> `git reset --hard HEAD`


Чтобы выполнить слияние веток в Git, выполните следующие шаги:

1. **Переключитесь на ветку, в которую хотите выполнить слияние** (обычно это `main` или `master`):
    
`git checkout main

2. **Слейте нужную ветку** (например, `feature-branch`) с вашей текущей веткой:

`git merge feature-branch



`git checkout main && git merge feature-branch --no-commit` 