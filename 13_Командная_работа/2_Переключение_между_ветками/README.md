# Сценарии использования команды `git checkout`

## Основные варианты применения

### Переключение между ветками

Перемещает рабочую директорию на указанную ветку.
```bash
git checkout <branch_name>
```

### Создание и переключение на новую ветку

Создает новую ветку и сразу переключается на нее.
```bash
git checkout -b <branch_name>
```

### Переход к конкретному коммиту

Переход в состояние "detached HEAD" для осмотра кода на момент конкретного коммита.
```bash
git checkout <hash>
```
**Режим detached HEAD**
* При переходе по хэшу коммита создается временное состояние

* Все новые коммиты могут быть утеряны, если не создать ветку

* Для возврата: `git switch -` 

## Практический пример

Перейдем к конкретному коммиту, создадим ветку и смерджим с основной.  

Аналогия с git checkout:  
```bash
# Традиционный способ
git checkout -b feature/new_feature

# Современная альтернатива
git switch -c feature/new_feature
```

<br>

Разберем пример:

```sh
# 1) возвращаем список коммитов
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline 
01424c0 (HEAD -> master, feature_poem) add the poem
329aab3 (origin/master, origin/HEAD) Update README.md
d2c1eb2 Rename README.MD to README.md
f0ac494 some minor change
7f39bc7 add page3.html
f46590a add readme file
2cc4e0b force adding debug.log
4cd50da add gitignore file
10f7044 move files to separate folders
8a51408 create folder for css styles
bea5ea0 rename index file # выбрали коммит
7139d06 Revert "add some new features"
59b19d0 Reapply "add some new features"
7fd94af Revert "Reapply "add some new features""
4b2413e Reapply "add some new features"
b0272be Revert "add some new features"
9e59768 add some new features
8a8b14c refactor css styles
ca3a6b1 add css styles
e4de21c add new paragraph
0e47d61 add page2
55f77dc add page1.html

# 2) выбрав коммит переключаемся на него
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git checkout bea5ea0
Note: switching to 'bea5ea0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at bea5ea0 rename index file

# убеждаемся, что выбранный коммит теперь является последним
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project ((bea5ea0...))
$ git log --oneline
bea5ea0 (HEAD) rename index file # последний коммит
7139d06 Revert "add some new features"
59b19d0 Reapply "add some new features"
7fd94af Revert "Reapply "add some new features""
4b2413e Reapply "add some new features"
b0272be Revert "add some new features"
9e59768 add some new features
8a8b14c refactor css styles
ca3a6b1 add css styles
e4de21c add new paragraph
0e47d61 add page2
55f77dc add page1.html

# создаем новую ветку и переключаемся на нее
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project ((bea5ea0...))
$ git switch -c test_branch
Switched to a new branch 'test_branch'

# вносим какие-то изменения
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (test_branch)
$ touch test_branch_file

# видим, что изменения (нвый файл) добавлены
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (test_branch)
$ ls
index.html  
page2.html  
styles.css  
test_branch_file # новый файл

# запросив статус видим, что новый файл не отслеживается
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (test_branch)
$ git status
On branch test_branch
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test_branch_file

nothing added to commit but untracked files present (use "git add" to track)

# добавляем новый файл в индекс
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (test_branch)
$ git add .

# коммитим 
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (test_branch)
$ git commit -m 'add test file'
[test_branch 180fd0d] add test file
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test_branch_file

# переключаемся обратно на ветку master
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (test_branch)
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

# мерджим с тестовой веткой test_branch
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git merge test_branch
hint: Waiting for your editor to close the file... unix2dos: converting file C:/Users/user/git_learn/first_project/.git/MERGE_MSG to DOS format...
dos2unix: converting file C:/Users/user/git_learn/first_project/.git/MERGE_MSG to Unix format...
Merge made by the 'ort' strategy.
 test_branch_file | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test_branch_file

# после мерджа проверяем список файлов
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ ls
README.md  
css/  
debug.log  
poem.txt  
rare_critical_logs.log  
test_branch_file # наш новый файл с тестовой ветки 
web_pages/

# смотрим лог с визуализацией графов, видим вторую ветку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
*   ae28ef0 (HEAD -> master) Merge branch 'test_branch'
|\
| * 180fd0d (test_branch) add test file
* | 01424c0 (feature_poem) add the poem
* | 329aab3 (origin/master, origin/HEAD) Update README.md
* | d2c1eb2 Rename README.MD to README.md
* | f0ac494 some minor change
* | 7f39bc7 add page3.html
* | f46590a add readme file
* | 2cc4e0b force adding debug.log
* | 4cd50da add gitignore file
* | 10f7044 move files to separate folders
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
```