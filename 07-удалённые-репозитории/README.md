# 07. Удалённые репозитории

| Команда | Синтаксис | Описание |
|---------|-----------|----------|
| `git remote` | `git remote` | Список удалённых |
| `git remote -v` | `git remote -v` | Удалённые с URL |
| `git remote add` | `git remote add имя URL` | Добавить удалённый |
| `git remote remove` | `git remote remove имя` | Удалить удалённый |
| `git fetch` | `git fetch имя_удалённого` | Получить изменения (без слияния) |
| `git pull` | `git pull имя_удалённого ветка` | Получить и автоматически слить |
| `git pull --rebase` | `git pull --rebase` | Переместить локальные коммиты поверх новых |
| `git push` | `git push имя_удалённого ветка` | Отправить изменения |
| `git push -u` | `git push -u origin ветка` | Установить upstream связь |
| `git push -d` | `git push origin -d ветка` | Удалить ветку на удалённом |


