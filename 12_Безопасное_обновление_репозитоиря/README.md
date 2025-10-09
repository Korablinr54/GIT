# Безопасное обновление локального репозитория (изменения в разных файлах)

Рассмотрим пример. Команда `git status` покзывает нам, что наш локальный репозиторий опережает удаленный (origin) на 13 коммитов. 
```sh
# Пример вывода git status
$ git status
On branch master
Your branch is ahead of 'origin/master' by 13 commits.
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
   a1b2c3d..f4e5g6h  master -> master
```

Мы могли бы проверить, что хеш нашего последнего коммита в локальном репозитории и хеш последнего коммита после пуша совпадают, чтобы удостовериться, что репозитории синхронизированы.  
```sh
# Проверяем хеш последнего коммита локально
$ git log --oneline -1
f4e5g6h (HEAD -> master) Add new authentication feature

# Проверяем хеш в удаленном репозитории
$ git log --oneline origin/master -1
f4e5g6h (origin/master) Add new authentication feature

# Хеши совпадают - значит репозитории синхронизированы
```

Теперь представим, что наш коллега выполнил какой-то коммит. Мы понимаем, что локально этого коммита на нашей машине конечно же нет. Можем проверить последние 5 коммитов командой `git log --oneline -5`. Это значит, что нам нужно скачать эти изменения себе, т.е. выполнить `git pull`. 
```sh
# Проверяем локальную историю ДО pull
$ git log --oneline -5
f4e5g6h (HEAD -> master) Add new authentication feature
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
   f4e5g6h..h8i9j0k  master     -> origin/master
Updating f4e5g6h..h8i9j0k
Fast-forward
 database.py | 3 +++
 1 file changed, 3 insertions(+)

# Проверяем историю ПОСЛЕ pull
$ git log --oneline -5
h8i9j0k (HEAD -> master, origin/master) Add database connection pool by colleague
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
k1l2m3n (HEAD -> master) Add local feature X

$ git log --oneline origin/master -3
p4q5r6s (origin/master) Add remote feature Y by colleague

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
   k1l2m3n..p4q5r6s  master     -> origin/master
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
*   t7u8v9w (HEAD -> master) Merge branch 'master' of https://github.com/user/repo
|\  
| * p4q5r6s (origin/master) Add remote feature Y by colleague
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

# Безопасное обновление локального репозитория (изменения в одном файле)

Предположим, что пока мы редактировали файл на своей удаленной машине, кто-то в свою очередь также внес какие-то правки в этот же самый файл и запушил их уже в удаленный репозиторий.
```sh
# Локальный файл config.py ДО изменений
$ cat config.py
DATABASE_URL = "sqlite:///app.db"
DEBUG = True

# Локальные изменения
$ cat config.py
DATABASE_URL = "postgresql://localhost/mydb"  # Мы изменили на PostgreSQL
DEBUG = True
MAX_CONNECTIONS = 10  # Мы добавили новую настройку
```

Такую ситуацию Git уже не сможет разрешить самостоятельно.  
Мы видим, что в удаленном репозитории и в локальном отличаются последние коммиты, при этом изменения затрагивают один и тот же файл, но тем не менее нам нужно синхронизироваться.  
```sh
# Проверяем состояние
$ git log --oneline -1
a1b2c3d (HEAD -> master) Change to PostgreSQL database

$ git log --oneline origin/master -1
e4f5g6h (origin/master) Add Redis configuration by colleague
```

Выполнив `git pull` мы увидим сообщение об ошибке, что git пробовл сделать `auto-merge`, обнаружил конфликт в файле (который мы с коллегой редактировали, каждый в своей ветке, но он запушил свои разработки раньше чем мы) и последняя стрчока говорит о том, что `merge failed`, не удалось git самостоятельно разрешить этот конфликт. Также в строке приглашения мы видим, что находисмся в процессе `merging`.  
```sh
$ git pull
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (1/1), 652 bytes | 652.00 KiB/s, done.
From https://github.com/user/repo
   a1b2c3d..e4f5g6h  master     -> origin/master
Auto-merging config.py
CONFLICT (content): Merge conflict in config.py
Automatic merge failed; fix conflicts and then commit the result.

# Обратите внимание на приглашение командной строки:
user@pc:~/project (master|MERGING)$
```

Выполнив `git status` мы видим, что у нас есть конфликт.  
```sh
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   config.py

no changes added to commit (use "git add" and/or "git commit -a")
```

**Ключевая идея** - найти способ избегать таких проблемных ситуаций, чтобы не попасть в `merge hell`.  

Первое, что нам нужно сделать - откатить неудавшийся merge используя `git merge --abort`:
```sh
$ git merge --abort

# Убеждаемся, что в строке приглашения у нас снова просто имя ветки без merging
user@pc:~/project (master)$
```
Проверяем `git status`, `git log --oneline -5`, чтобы убедиться, что мы откатились к состоянию до неудавшегося слияния.
```sh
$ git status
On branch master
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean

$ git log --oneline -5
a1b2c3d (HEAD -> master) Change to PostgreSQL database
f7g8h9i Previous common commit
```

Данной ситуации можно было бы избежать воспользуйся мы командой `git fetch`. Команда позволяет скачать изменения из удаленного репозитория в локальный но при этом эти изменения не будут применяться в локальном репозитории после скачивания из удаленного. 

Теоретически:  
`git pull` = `git fetch` + `git merge`  

Если обратиться к нашей ситуации то именно на этапе `git merge` мы и столкнулись с проблемой.  

Если мы будем делать `git fetch` то у нас будет возможность заранее проверить, нет ли потенциальных конфликтов.  Давайте выполним `git fetch origin`:  
```sh
$ git fetch origin
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (1/1), 652 bytes | 652.00 KiB/s, done.
From https://github.com/user/repo
   a1b2c3d..e4f5g6h  master     -> origin/master
```
После выполнения `git fetch origin` гит знает, что есть в удаленном репозитории, но при этому никакие из этих измененйи не были применены к локальному репозиторию.  

Давайте взглянем на различия используя команду `git diff origin/master`:  
```sh
$ git diff origin/master
diff --git a/config.py b/config.py
index 789abc..def012 100644
--- a/config.py
+++ b/config.py
@@ -1,2 +1,3 @@
-DATABASE_URL = "sqlite:///app.db"
+DATABASE_URL = "postgresql://localhost/mydb"
 DEBUG = True
+MAX_CONNECTIONS = 10
```

Отлично, мы видим, что в одном и том же файле есть изменения, и в локальном и в удаленном.  
```sh
# Смотрим что изменилось в удаленной версии
$ git show origin/master:config.py
DATABASE_URL = "sqlite:///app.db"
DEBUG = True
REDIS_URL = "redis://localhost:6379"  # Коллега добавил Redis
```

Теперь убедимся на примере команды `git log master..origin/master`: 
```sh
$ git log master..origin/master --oneline
e4f5g6h Add Redis configuration by colleague
```
Видим, что есть какой-то комит отличающий эти два репозитория. Отсюда мы понимаем, что впереди нас ожидает конфликт. Значит мы можем к нему подготовиться. Давайте в текстовом редакторе приведем этот файл на локальной машине к такому виду, который не будет конфликтовать.   
```sh
# Редактируем config.py чтобы включить оба изменения
$ cat config.py
DATABASE_URL = "postgresql://localhost/mydb"  # Наше изменение
DEBUG = True
MAX_CONNECTIONS = 10  # Наше добавление
REDIS_URL = "redis://localhost:6379"  # Добавляем изменение коллеги
```

Добавляем эти изменения в индекс, коммитим, проверяем `git log --oneline -5` видим наш коммит и по идее `git diff` не должен вернуть ничего, т.к. мы подготовились к отличиям.  
```sh
$ git add config.py
$ git commit -m "Integrate both PostgreSQL and Redis configurations"
[master j1k2l3m4] Integrate both PostgreSQL and Redis configurations
 1 file changed, 3 insertions(+), 1 deletion(-)

$ git log --oneline -5
j1k2l3m4 (HEAD -> master) Integrate both PostgreSQL and Redis configurations
a1b2c3d Change to PostgreSQL database
f7g8h9i Previous common commit

$ git diff origin/master
# Вывод пустой - конфликтов нет!
```

Теперь, коггда мы подготовились можем спокойно выполнять `git merge origin/master` 
```sh
$ git merge origin/master
Already up to date.
```

Никаких конфликтов не произошло, это нам подскажет `git status` и `git log --oneline -5`. таки м образом мы избежали конфликта.
```sh
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

$ git log --oneline -5
j1k2l3m4 (HEAD -> master) Integrate both PostgreSQL and Redis configurations
a1b2c3d Change to PostgreSQL database
e4f5g6h (origin/master) Add Redis configuration by colleague
f7g8h9i Previous common commit
```

Теперь можно выполнять `git push` и синхронизировать репозитории.