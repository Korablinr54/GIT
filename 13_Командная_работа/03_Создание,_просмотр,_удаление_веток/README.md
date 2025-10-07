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

## Практический пример

Предположим, что у нас возникла ситуация, когда в ветке мастер появился баг и нам нужно найти коммит с багом, исправить его в отдельной ветке и влить фикс обратно в мастер:
```sh
# возвращаем список коммитов
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
* 791b642 (HEAD -> master, origin/master, origin/HEAD) some bug was fixed
*   ae28ef0 (origin/some_new_branch, some_new_branch) Merge branch 'test_branch'
|\
| * 180fd0d (origin/test_branch, test_branch) add test file
* | 01424c0 (origin/feature_poem, feature_poem) add the poem
* | 329aab3 Update README.md
* | d2c1eb2 Rename README.MD to README.md
* | f0ac494 some minor change
* | 7f39bc7 add page3.html
* | f46590a add readme file
* | 2cc4e0b force adding debug.log
* | 4cd50da add gitignore file
* | 10f7044 (origin/hotfix/branch_fix, hotfix/branch_fix) move files to separate folders
* | 8a51408 create folder for css styles
|/
* bea5ea0 rename index file
* 7139d06 Revert "add some new features"
* 59b19d0 Reapply "add some new features"
* 7fd94af Revert "Reapply "add some new features""
* 4b2413e Reapply "add some new features"
* b0272be Revert "add some new features"
* 9e59768 add some new features
* 8a8b14c refactor css styles
* ca3a6b1 add css styles
* e4de21c add new paragraph
* 0e47d61 add page2
* 55f77dc add page1.html

# переключаемся между коммитами, имитируем поиск бага
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git checkout 7f39bc7
Note: switching to '7f39bc7'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 7f39bc7 add page3.html

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project ((7f39bc7...))
$ ls
README.MD  css/  debug.log  rare_critical_logs.log  web_pages/

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project ((7f39bc7...))
$ git checkout 4cd50da
Previous HEAD position was 7f39bc7 add page3.html
HEAD is now at 4cd50da add gitignore file

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project ((4cd50da...))
$ ls
css/  rare_critical_logs.log  web_pages/

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project ((4cd50da...))
$ cat some_fixe_file
cat: some_fixe_file: No such file or directory

# найдя баг возвращаемся к последнему коммиту ветки мастер
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project ((4cd50da...))
$ git checkout master
Previous HEAD position was 4cd50da add gitignore file
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

# проверяем, что вернулись к последнему коммиту
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
* 791b642 (HEAD -> master, origin/master, origin/HEAD) some bug was fixed

# ...лишнее вырежем

# создаем новую ветку от последнего здорового коммита
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git switch -c hotfix/bug_fix 4cd50da
Switched to a new branch 'hotfix/bug_fix'

# убеждаемся ,что последний здоровый коммит в нововй ветке является последним
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (hotfix/bug_fix)
$ git log --oneline --graph
* 4cd50da (HEAD -> hotfix/bug_fix) add gitignore file
* 10f7044 (origin/hotfix/branch_fix, hotfix/branch_fix) move files to separate folders
* 8a51408 create folder for css styles
* bea5ea0 rename index file
* 7139d06 Revert "add some new features"
* 59b19d0 Reapply "add some new features"
* 7fd94af Revert "Reapply "add some new features""
* 4b2413e Reapply "add some new features"
* b0272be Revert "add some new features"
* 9e59768 add some new features
* 8a8b14c refactor css styles
* ca3a6b1 add css styles
* e4de21c add new paragraph
* 0e47d61 add page2
* 55f77dc add page1.html

# проводим манипуляции, имитируем работу по фиксу бага
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (hotfix/bug_fix)
$ touch some_bug_fix_file

# првоеряем статус, вимидм, что в индекс фикс не попал
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (hotfix/bug_fix)
$ git status
On branch hotfix/bug_fix
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        some_bug_fix_file

nothing added to commit but untracked files present (use "git add" to track)

# добавляем результат работы в индекс
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (hotfix/bug_fix)
$ git add .

# коммитим
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (hotfix/bug_fix)
$ git commit -m 'some_bug_was_fixed'
[hotfix/bug_fix c447c64] some_bug_was_fixed
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 some_bug_fix_file

# возвращаемся на ветку мастер
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (hotfix/bug_fix)
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

# првоеряем, что вернулись обратно
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
# ... лишнее сократим

# мерджим мастер и фикс
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git merge hotfix/bug_fix
hint: Waiting for your editor to close the file... unix2dos: converting file C:/Users/user/git_learn/first_project/.git/MERGE_MSG to DOS format...
dos2unix: converting file C:/Users/user/git_learn/first_project/.git/MERGE_MSG to Unix format...
Merge made by the 'ort' strategy.
 some_bug_fix_file | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 some_bug_fix_file

# проверяем. появилась вторая ветка с фиксом
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
*   d7d2989 (HEAD -> master) Merge branch 'hotfix/bug_fix'
|\
| * c447c64 (hotfix/bug_fix) some_bug_was_fixed
* | 791b642 (origin/master, origin/HEAD) some bug was fixed
* |   ae28ef0 (origin/some_new_branch, some_new_branch) Merge branch 'test_branch'
|\ \
| * | 180fd0d (origin/test_branch, test_branch) add test file
* | | 01424c0 (origin/feature_poem, feature_poem) add the poem
* | | 329aab3 Update README.md
* | | d2c1eb2 Rename README.MD to README.md
* | | f0ac494 some minor change
* | | 7f39bc7 add page3.html
* | | f46590a add readme file
* | | 2cc4e0b force adding debug.log
| |/
|/|
* | 4cd50da add gitignore file
* | 10f7044 (origin/hotfix/branch_fix, hotfix/branch_fix) move files to separate folders
* | 8a51408 create folder for css styles
|/
* bea5ea0 rename index file
* 7139d06 Revert "add some new features"
* 59b19d0 Reapply "add some new features"
* 7fd94af Revert "Reapply "add some new features""
* 4b2413e Reapply "add some new features"
* b0272be Revert "add some new features"
* 9e59768 add some new features

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ ls
#..
some_bug_fix_file  # вот наш "фикс" в ветке мастер 
#..
```

# Удаление и переименование веток
Удалять ветки можно двумя способами. Используяю ключи:  
- `git branch -d <имя_ветки>` Безопасное удаление (только если изменения слиты)  
- `git branch -D <имя_ветки>` Принудительное удаление (даже если не слито)


## Практический пример
Пример с использование ключа `-d`:  
```sh
#
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch
  feature_poem
  hotfix/branch_fix
  hotfix/bug_fix # это удалим
  hotgix/readme_fix
* master
  some_new_branch
  test_branch

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
*   d7d2989 (HEAD -> master) Merge branch 'hotfix/bug_fix' # это удалим
|\
| * c447c64 (hotfix/bug_fix) some_bug_was_fixed # это удалим
* | 791b642 (origin/master, origin/HEAD) some bug was fixed
* |   ae28ef0 (origin/some_new_branch, some_new_branch) Merge branch 'test_branch'
|\ \
| * | 180fd0d (origin/test_branch, test_branch) add test file
* | | 01424c0 (origin/feature_poem, feature_poem) add the poem
* | | 329aab3 Update README.md
* | | d2c1eb2 Rename README.MD to README.md
* | | f0ac494 some minor change
* | | 7f39bc7 add page3.html
* | | f46590a add readme file
* | | 2cc4e0b force adding debug.log
| |/
|/|
* | 4cd50da add gitignore file
* | 10f7044 (origin/hotfix/branch_fix, hotfix/branch_fix) move files to separate folders
* | 8a51408 create folder for css styles
|/
* bea5ea0 rename index file
* 7139d06 Revert "add some new features"
* 59b19d0 Reapply "add some new features"
* 7fd94af Revert "Reapply "add some new features""
* 4b2413e Reapply "add some new features"
* b0272be Revert "add some new features"
* 9e59768 add some new features

# удаляем ветку hotfix/bug_fix
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch -d hotfix/bug_fix
Deleted branch hotfix/bug_fix (was c447c64).

# ветка hotfix/bug_fix удалена 
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git branch
  feature_poem
  hotfix/branch_fix
  hotgix/readme_fix
* master
  some_new_branch
  test_branch
```


