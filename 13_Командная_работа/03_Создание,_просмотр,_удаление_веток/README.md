# Просмотр списка веток

Для отображения списка веток используется команда `git branch`:  
```sh
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch
  feature_poem
* master    # звездочкой отмечена текущая ветка
  test_branch
```

<br>

Также мы могли бы использовать команду `git branch` с ключами:  
- `-a` показать все ветки, т.е. локальные и удаленные    
- `-r` показать удаленные ветки      
- `-v` показать последний коммит в ветке    
- `-vv` для отображения связи локальных и удаленных веток   
```sh
# `-a` показать все ветки, т.е. локальные и удаленные
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -a
  feature_poem
* master
  test_branch
  remotes/origin/HEAD -> origin/master
  remotes/origin/master

# `-r` показать удаленные ветки 
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -r
  origin/HEAD -> origin/master
  origin/master

# `-v` показать последний коммит в ветке 
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -v
  feature_poem 01424c0 add the poem
* master       ae28ef0 [ahead 3] Merge branch 'test_branch'
  test_branch  180fd0d add test file

# `-vv` для отображения связи локальных и удаленных веток
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -vv
  feature_poem 01424c0 add the poem
* master       ae28ef0 [origin/master: ahead 3] Merge branch 'test_branch'
  test_branch  180fd0d add test file
```

# git push с ветками

Использовать команду git push можно также с дополнительными ключами и указанием конкретных веток:  
```sh
# использование команды по умолчанию пушит текущую ветку
git push

# проверим
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -r
  origin/HEAD -> origin/master
  origin/master

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -v
  feature_poem 01424c0 add the poem
* master       ae28ef0 [ahead 3] Merge branch 'test_branch'
  test_branch  180fd0d add test file

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -vv
  feature_poem 01424c0 add the poem
* master       ae28ef0 [origin/master: ahead 3] Merge branch 'test_branch'
  test_branch  180fd0d add test file

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -r
  origin/HEAD -> origin/master
  origin/master

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -vv
  feature_poem 01424c0 add the poem
* master       ae28ef0 [origin/master: ahead 3] Merge branch 'test_branch'
  test_branch  180fd0d add test file

# можно одновременно запушить все ветки в удаленный репозиторий
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git push --all
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 12 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 1.48 KiB | 1.48 MiB/s, done.
Total 7 (delta 3), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (3/3), completed with 2 local objects.
To https://github.com/Korablinr54/first_project.git
   329aab3..ae28ef0  master -> master
 * [new branch]      feature_poem -> feature_poem
 * [new branch]      test_branch -> test_branch

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -r
  origin/HEAD -> origin/master
  origin/feature_poem
  origin/master
  origin/test_branch

# либо мы можем пушить выбранные ветки
git push origin branch1 branch2 branch3
```

# Создание веток

В самом общем виде команда `git branch` вернет список локальных веток. 
```sh
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch
  feature_poem
* master
  test_branch
```

<br>

Для создания новой ветки нам нужно добавить ее имя. Обращаю внимание, что при создании новой ветки не происходит автоматического переключения как с командой `git checkout`:
```sh
# проверям список веток и видим, что их сейчас 3
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch
  feature_poem
* master
  test_branch

# создаем новую ветку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch some_new_branch

# снова проверяем список и видим, что уже 4 ветки вместо 3
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch
  feature_poem
* master
  some_new_branch
  test_branch

# переключаемся на новую ветку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git switch some_new_branch
Switched to branch 'some_new_branch'

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (some_new_branch) # переключение прошло успешно

# переключаемся обратно
$ git switch -
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

# Практический пример

Предположим, что у нас возникла ситуация, когда в ветке мастер появился баг и нам нужно найти коммит с багом, исправить его в отдельной ветке и влить фикс обратно в мастер:
```sh
#

```
