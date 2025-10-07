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
