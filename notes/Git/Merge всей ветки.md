
`git merge <из какой ветки> 
							`--squash`  объединяя все коммиты в один. Это полезно, если вы хотите сохранить историю более чистой и избежать создания множества мелких коммитов.


Чтобы выполнить слияние веток в Git, выполните следующие шаги:

1. **Переключитесь на ветку, в которую хотите выполнить слияние** (обычно это `main` или `master`):
    
`git checkout main

2. **Слейте нужную ветку** (например, `feature-branch`) с вашей текущей веткой:

`git merge feature-branch

