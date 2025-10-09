# Безопасное обновление локального репозитория

Рассмотрим пример. Команда `git status` покзывает нам, что наш локальный репозиторий опережает удаленный (origin) на 13 коммитов. 
```sh
# Пример вывода git status
$ git status
On branch main
Your branch is ahead of 'origin/main' by 13 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Это значит, что нам нужно выполнить `git push`, т.е. отправить наши наработки в удаленный репозиторий, чтобы коллеги могли и также использовать.
```sh
# Выполняем git push
$ git push
Enumerating objects: 39, done.
Counting objects: 100% (39/39), done.
Delta compression using up to 8 threads
Compressing objects: 100% (21/21), done.
Writing objects: 100% (33/33), 2.87 KiB | 2.87 MiB/s, done.
Total 33 (delta 15), reused 0 (delta 0)
remote: Resolving deltas: 100% (15/15), completed with 10 local objects.
To https://github.com/user/repo.git
   a1b2c3d..f4e5g6h  main -> main
```

Мы могли бы проверить, что хеш нашего последнего коммита в локальном репозитории и хеш последнего коммита после пуша совпадают, чтобы удостовериться, что репозитории синхронизированы.  
```sh
# Проверяем хеш последнего коммита локально
$ git log --oneline -1
f4e5g6h (HEAD -> main) Add new authentication feature

# Проверяем хеш в удаленном репозитории
$ git log --oneline origin/main -1
f4e5g6h (origin/main) Add new authentication feature

# Хеши совпадают - значит репозитории синхронизированы
```

Теперь представим, что наш коллега выполнил какой-то коммит. Мы понимаем, что локально этого коммита на нашей машине конечно же нет. Можем проверить последние 5 коммитов командой `git log --oneline -5`. Это значит, что нам нужно скачать эти изменения себе, т.е. выполнить `git pull`. 
```sh
# Проверяем локальную историю ДО pull
$ git log --oneline -5
f4e5g6h (HEAD -> main) Add new authentication feature
a1b2c3d Fix user registration bug
7890abc Update README.md
4567def Add initial project structure
1234abc Initial commit
```
```sh
# Выполняем git pull
$ git pull
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (1/1), 652 bytes | 652.00 KiB/s, done.
From https://github.com/user/repo
   f4e5g6h..h8i9j0k  main     -> origin/main
Updating f4e5g6h..h8i9j0k
Fast-forward
 database.py | 3 +++
 1 file changed, 3 insertions(+)

# Проверяем историю ПОСЛЕ pull
$ git log --oneline -5
h8i9j0k (HEAD -> main, origin/main) Add database connection pool by colleague
f4e5g6h Add new authentication feature
a1b2c3d Fix user registration bug
7890abc Update README.md
4567def Add initial project structure
```
Все хорошо, мы синхронизировались с удаленным репозиторием. Это не было проблемой, т.к. у нас не было изменений в локальном репозитории отсуствующих на сервере.  

Проиллюстрируем другую ситуацию, когда локальный и удаленный репозиторий будут расходиться. В каждом из них будет какой-то новый коммит, которого нет во втором репозитории. 
```sh
# Проверяем состояние ДО конфликта
$ git log --oneline -3
k1l2m3n (HEAD -> main) Add local feature X

$ git log --oneline origin/main -3
p4q5r6s (origin/main) Add remote feature Y by colleague

# Видим, что коммиты разные в локальном и удаленном репозитории
```

При этом нам нужно синхронизироваться. Значит выполянем `git pull` и получаем сообщение о том, что нужно сделать `git merge`. 
```sh
$ git pull
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (1/1), 652 bytes | 652.00 KiB/s, done.
From https://github.com/user/repo
   k1l2m3n..p4q5r6s  main     -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
Merge made by the 'ort' strategy.
 database.py | 2 ++
 1 file changed, 2 insertions(+)
```

Смотрим на сообщение о том, что нового мы скачали. Теперь смотрим `git log --oneline --graph -5`.
```sh
$ git log --oneline --graph -5
*   t7u8v9w (HEAD -> main) Merge branch 'main' of https://github.com/user/repo
|\  
| * p4q5r6s (origin/main) Add remote feature Y by colleague
* | k1l2m3n Add local feature X
|/  
* f4e5g6h Previous common commit
```

И мы видим, что 3 коммит с конца это наш локальный коммит, за ним идет коммит из удаленного репозитория а последний это merge коммит, в котором мы объединяли наши коммиты в репозитории (локальный и удаленный). 


Git сам смог резрешить этот вопрос, смерджить и теперь можем спокойно пушить. 
```sh
$ git push
Everything up-to-date
```