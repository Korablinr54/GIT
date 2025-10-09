# Fast-forward merge

Рассмотрим сценарий, в котором создается новая ветка и при этом параллельной работы в основной ветке не ведется, после выполняется слияние и такая работал провоцирует следующую ситуацию. Давайте рассмотрим:  
```sh
# для начала взглянем на лог коммитов
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
*   d7d2989 (HEAD -> master) Merge branch 'hotfix/bug_fix' 
|\
| * c447c64 some_bug_was_fixed
* | 791b642 (origin/master, origin/HEAD) some bug was fixed
* |   ae28ef0 (origin/some_new_branch, some_new_branch) Merge branch 'test_branch'
# ..

# создаем новую ветку и переключаемся на нее
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git switch -c 'new_branch_test'
Switched to a new branch 'new_branch_test'

# делаем вид, что работает
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (new_branch_test)
$ touch some_file

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (new_branch_test)
$ git status
On branch new_branch_test
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        some_file

nothing added to commit but untracked files present (use "git add" to track)

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (new_branch_test)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (new_branch_test)
$ git commit -m "some_feature"
[new_branch_test 70a019d] some_feature
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 some_file

# возвращаемся на основную ветку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (new_branch_test)
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

# сливаем ветки
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git merge new_branch_test
Updating d7d2989..70a019d
Fast-forward
 some_file | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 some_file

# видим такую картину тк параллельной работы не велось, гит отображает коммит не совсем интуитивно
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
* 70a019d (HEAD -> master, new_branch_test) some_feature
*   d7d2989 Merge branch 'hotfix/bug_fix'
|\
| * c447c64 some_bug_was_fixed
* | 791b642 (origin/master, origin/HEAD) some bug was fixed
* |   ae28ef0 (origin/some_new_branch, some_new_branch) Merge branch 'test_branch'
```

Используем ключ `--no-ff` и посмотрим как изменится ситуация с отображением коммитов:  
```sh
# для начала откатимся на предпоследний коммит
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git reset --hard HEAD^
HEAD is now at d7d2989 Merge branch 'hotfix/bug_fix'

# ок, мы вернулись на предыдущий коммит
$ git log --oneline --graph
*   ae28ef0 (HEAD -> master, origin/some_new_branch, some_new_branch) Merge branch 'test_branch'
|\
| * 180fd0d (origin/test_branch, test_branch) add test file

# теперь повторим все действия до слияния
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git switch -c no-ff_branch
Switched to a new branch 'no-ff_branch'

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (no-ff_branch)
$ touch some_file_no-ff

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (no-ff_branch)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (no-ff_branch)
$ git commit -m'some msg'
[no-ff_branch 5fd1e31] some msg
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 some_file_no-ff

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (no-ff_branch)
$ git switch master
Switched to branch 'master'
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

# используем ключ `--no-ff`
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git merge --no-ff no-ff_branch
hint: Waiting for your editor to close the file... unix2dos: converting file C:/Users/user/git_learn/first_project/.git/MERGE_MSG to DOS format...
dos2unix: converting file C:/Users/user/git_learn/first_project/.git/MERGE_MSG to Unix format...
Merge made by the 'ort' strategy.
 some_file_no-ff | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 some_file_no-ff

# видим насколько изменилась картина с отображением 
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git log --oneline --graph
*   76d28f2 (HEAD -> master) Merge branch 'no-ff_branch'
|\
| * 5fd1e31 (no-ff_branch) some msg
|/
*   ae28ef0 (origin/some_new_branch, some_new_branch) Merge branch 'test_branch'
```
несмотря на то, что параллельной работы в основной ветке не велось, новая ветка все равно была вынесена графически, что на мой взгляд гораздо нагляднее.  