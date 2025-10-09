# stash

Временно сохраняет незакоммиченные изменения, чтобы переключиться на другую ветку без коммита.  

## Сохранение изменений

Сохраняет изменения в stash и откатывает рабочую директорию до последнего коммита.  
```sh
git stash
```

**Ключи**:  
- `-u` или `--include-untracked` - включить неотслеживаемые файлы  
- `-a` или `--all` - включить все файлы (даже игнорируемые)  
- `-m <message>` или `--message <message>` - добавить описание  
- `-p` или `--patch` - интерактивный выбор изменений  

**Пример**:  
```sh
# Изменили файл main.py
git status  # покажет измененный файл
git stash   # изменения сохранены, рабочая директория чистая
git status  # ничего не показывает
```

## Просмотр списка stash

Показывает все сохраненные stash'и.
```sh
git stash list
```

## Восстановление изменений

Восстанавливает последний stash и удаляет его из списка.
```sh
git stash pop
```

Восстанавливает последний stash, но не удаляет его из списка.
```sh
git stash apply
```

**Пример**:
```sh
git stash list    # stash@{0}: WIP on main: abc1234
git stash pop     # восстанавливаем и удаляем stash
# или
git stash apply   # восстанавливаем, но stash остается в списке
```

## Сохранение с именем

```sh
git stash save "описание изменений"
```

## Удаление stash

```sh
git stash drop     # удаляет последний stash
git stash clear    # удаляет все stash'и
```

# Практический пример

Симитируем сценарий, в котором нас просят срочно переключиться на другую ветку и выполнить какую-то работу. А поскольку работу в своей ветке мы еще не закнчили мы не можем коммитить нерабочий код и нужно как-то не потерять наши наработки при этом не коммитя их.

```sh
# взглянем на текущее состояние
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git log --oneline --graph -5
*   b0b10e5 (HEAD -> master) the conflict was resolved
|\
| * dae37f0 (new_feature) new features
* | 4d2551a main branch fix
|/
* 329aab3 (origin/master) Update README.md
* d2c1eb2 Rename README.MD to README.md

# откатимся на один коммит назад, до слияния
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git reset --hard HEAD~1
HEAD is now at 4d2551a main branch fix

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git log --oneline --graph --all -5
* 4d2551a (HEAD -> master) main branch fix
| * dae37f0 (new_feature) new features
|/
* 329aab3 (origin/master) Update README.md
* d2c1eb2 Rename README.MD to README.md
* f0ac494 some minor change

# и переключимся на вторую ветку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git switch new_feature
Switched to branch 'new_feature'

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature) # переключили

# симитируем работу в новой ветке
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ ls
README.md  css/  debug.log  rare_critical_logs.log  web_pages/

# создадим новый файл со строкой Привет мир!
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ echo Hello world! > new_file

# проверим, что он появился
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ ls
# ..
 new_file  
# ..

# убеждаемся, что гит его видит
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git status
On branch new_feature
Untracked files: # неотслеживаемый еще
  (use "git add <file>..." to include in what will be committed)
        new_file # вот наш нвоый файл

nothing added to commit but untracked files present (use "git add" to track)

# тут нас просят переключиться на другую ветку и срочно что-то сделать
# но тк мы еще не закончили работу во второй ветке мы не можем коммитить ее, тк код еще не рабочий

# делам stash с ключом `-u` для неотслеж файлов
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git stash -u
Saved working directory and index state WIP on new_feature: dae37f0 new features

# видим, что ноаш новый файл пропал (он все равно сохранен)
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ ls
README.md  css/  debug.log  rare_critical_logs.log  web_pages/

# гит статус также ничего не показывает
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git status
On branch new_feature
nothing to commit, working tree clean

# зато stash list видит наши сохраненные но незакоммиченные изменения
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git stash list
stash@{0}: WIP on new_feature: dae37f0 new features

# переключаемся на основную ветку и вносим изменения 
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git switch master
Switched to branch 'master'

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ echo Hello! >> web_pages/page2.html

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git commit -m'web pages2 fix'
[master cefd147] web pages2 fix
 1 file changed, 1 insertion(+)

# возвращаемся на нашу вторую, исходную ветку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git switch new_feature
Switched to branch 'new_feature'

# убеждаемся, что нашего нового файла нет
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ ls
README.md  css/  debug.log  rare_critical_logs.log  web_pages/

# в списке stash есть сохраненные изменения. вернем их
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git stash list
stash@{0}: WIP on new_feature: dae37f0 new features

# удаляем stash и возвращаем изменения
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git stash pop
Already up to date.
On branch new_feature
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new_file

nothing added to commit but untracked files present (use "git add" to track)
Dropped refs/stash@{0} (d9b2ab3b2a7d2f64ab1994740091bca1725bb9d5)

# файл появился
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ ls
# ..
new_file
# ..

# гит его видит как неотслеживаемый
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git status
On branch new_feature
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new_file

nothing added to commit but untracked files present (use "git add" to track)

# добавляем в индекс и комитим
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git commit -m'stash test'
[new_feature 5d669bc] stash test
 1 file changed, 1 insertion(+)
 create mode 100644 new_file

# затем переключаемся на мастер для выполнения слияния
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git switch master
Switched to branch 'master'

# мерджим
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git switch master
Switched to branch 'master'

# получаем уведомление о конфликте
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git merge new_feature
Auto-merging web_pages/page2.html
CONFLICT (content): Merge conflict in web_pages/page2.html # меняли параллельно в мастер этот файл
CONFLICT (modify/delete): web_pages/page3.html deleted in HEAD and modified in new_feature.  Version new_feature of web_pages/page3.html left in tree. # не слит (Unmerged path)
Automatic merge failed; fix conflicts and then commit the result.

# взглянем на конфликт детальнее
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git diff
diff --cc web_pages/page2.html
index a48cc04,bd9cabd..0000000
--- a/web_pages/page2.html
+++ b/web_pages/page2.html
@@@ -7,7 -7,7 +7,12 @@@
        <body>
                <h1>This is a Heading of page 2</h1>
                <p>This is a paragraph.</p>
++<<<<<<< HEAD
 +              <p>This is a bug.</p>
++=======
+               <p>This is a paragraph.</p>
+               <p>This is a paragraph.</p>
++>>>>>>> new_feature
        </body>
  </html>
 +Hello!
* Unmerged path web_pages/page3.html

# принимаем решение использовать код из сливаемой ветки
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git checkout --theirs .
Updated 2 paths from the index

# проверяем гит статус
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Changes to be committed:
        new file:   new_file # конфликт решен

Unmerged paths:
  (use "git add/rm <file>..." as appropriate to mark resolution)
        both modified:   web_pages/page2.html
        deleted by us:   web_pages/page3.html #  в текущей ветке (master) этот файл удален но в ветке new_feature он существует

# коммитим
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git commit -m 'stash merge'
[master 4265183] stash merge

# проверяем. все выполнено успешно
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git commit -m 'stash merge'^C

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git log --oneline --graph -5
*   4265183 (HEAD -> master) stash merge
|\
| * 5d669bc (new_feature) stash test
| * dae37f0 new features
* | cefd147 web pages2 fix
* | 4d2551a main branch fix
|/

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ ls
README.md  css/  debug.log  new_file  rare_critical_logs.log  web_pages/
```