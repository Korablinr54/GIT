# Конфликт слияния

Конфликт возникает, когда Git не может автоматически объединить изменения из разных веток, потому что в одних и тех же местах файлов были сделаны противоречивые правки.

Способы разрешения конфликтов:

**Ручные**:  
1) Редактировать файлы - убрать маркеры <<<<<<<, =======, >>>>>>>, оставив нужный код  
2) Использовать mergetool - git mergetool для визуального разрешения

**Автоматические**:  
1) Принять свою версию - git checkout --ours файл  
2) Принять чужую версию - git checkout --theirs файл  
3) Стратегии слияния - git merge -X ours/theirs ветка  

## Автоматическое решение конфликта

Git сам выбирает какую версию кода оставить, без ручного редактирования. Наша задача - выбрать стратегию сливяния:
```sh
git merge -X ours ветка    # всегда предпочитать наши изменения
git merge -X theirs ветка  # всегда предпочитать их изменения
```

### Практический пример

Для иллюстрации приведем ситуацию, в которой разработка велась в двух ветках параллельно и это привело к конфликту слияния.
```sh
# вернем 5 последних коммитов в ветке master
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git log --oneline --graph -n 5
* 329aab3 (HEAD -> master, origin/master) Update README.md
* d2c1eb2 Rename README.MD to README.md
* f0ac494 some minor change
* 7f39bc7 add page3.html
* f46590a add readme file

# создадим новую ветку и переключимся на нее
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git switch -c new_feature
Switched to a new branch 'new_feature'

# отредактируем пару файлов
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ nano web_pages/page2.html

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ nano web_pages/page3.html

# убедимся, что гит видит изменения
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git status
On branch new_feature
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   web_pages/page2.html
        modified:   web_pages/page3.html

no changes added to commit (use "git add" and/or "git commit -a")

# коммитим наши изменения
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git commit -m 'new features'
[new_feature dae37f0] new features
 2 files changed, 3 insertions(+), 1 deletion(-)

# переключаемся обратно на основную метку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (new_feature)
$ git switch master
Switched to branch 'master'

# параллельно новой ветке редактируем в основной те же файлы
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ nano web_pages/page2.html

# этот вообще давайте удалим
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ rm web_pages/page3.html

# убеждаемся, что гит видит изменения
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   web_pages/page2.html
        deleted:    web_pages/page3.html

no changes added to commit (use "git add" and/or "git commit -a")

# коммитим изменения в основной ветке
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git commit -m 'main branch fix'
[master 4d2551a] main branch fix
 2 files changed, 1 insertion(+), 13 deletions(-)
 delete mode 100644 web_pages/page3.html

# возвращаем список существующих веток
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git branch
* master
  new_feature

# выполняем попытку слияния
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git merge new_feature
Auto-merging web_pages/page2.html
CONFLICT (content): Merge conflict in web_pages/page2.html # конфликт
CONFLICT (modify/delete): web_pages/page3.html deleted in HEAD and modified in new_feature.  Version new_feature of web_pages/page3.html left in tree. # конфликт
Automatic merge failed; fix conflicts and then commit the result.

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING) # т.к. слияние не завершено видим в строке преглашения такую запись

# возвращаем разницу
$ git diff
diff --cc web_pages/page2.html # отличия в этом файле
index c817325,bd9cabd..0000000
--- a/web_pages/page2.html
+++ b/web_pages/page2.html
@@@ -7,6 -7,7 +7,11 @@@
        <body>
                <h1>This is a Heading of page 2</h1>
                <p>This is a paragraph.</p>
++<<<<<<< HEAD # как сейчас в основной ветке
 +              <p>This is a bug.</p>
++======= # это просто разделитель
+               <p>This is a paragraph.</p>
+               <p>This is a paragraph.</p>
++>>>>>>> new_feature # а это изменения из ветки, которую мерджиим
        </body>
  </html>
* Unmerged path web_pages/page3.html

# выбираем стратегию слияния
# для файла page2.html решаем оставить код основной ветки
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git checkout --ours web_pages/page2.html
Updated 1 path from the index

# для файла page3.html примем изменения из новой ветки. напомню, этот файл в основной ветке мы удалили
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git checkout --theirs web_pages/page3.html
Updated 1 path from the index

# првоеряем статус
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add/rm <file>..." as appropriate to mark resolution)
        both modified:   web_pages/page2.html
        deleted by us:   web_pages/page3.html

no changes added to commit (use "git add" and/or "git commit -a")

# коммитим
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING)
$ git commit -m 'solve conflict'
[master a8d0fd6] solve conflict

# взглянем на последние 5 коммитов
# слияние прошло успешно
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git log --oneline --graph -n 5
*   a8d0fd6 (HEAD -> master) solve conflict
|\
| * dae37f0 (new_feature) new features
* | 4d2551a main branch fix
|/
* 329aab3 (origin/master) Update README.md
* d2c1eb2 Rename README.MD to README.md

# как видим файл page3.html снова присуствует 
# мы его вернули из сторонней ветки
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ ls web_pages/
index.html  page2.html  page3.html

# а здесь остался код из основной ветки
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ cat web_pages/page2.html
<!DOCTYPE html>
<html>
        <head>
                <title>Page 2 Title</title>
                <link rel="stylesheet" href="/css/styles.css">
        </head>
        <body>
                <h1>This is a Heading of page 2</h1>
                <p>This is a paragraph.</p>
                <p>This is a bug.</p>
        </body>
</html>
```

## Отмена слияния
Уместно упомянуть, что мы можем отменить начатое слияние командой `git merge --abort`. Давайте это продемонстрируем:  
```sh
# взглянем на текущее состояние репозитория
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git log --oneline --graph -5
*   a8d0fd6 (HEAD -> master) solve conflict
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

# видим два параллельных коммита еще не слиты в одну ветку
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git log --oneline --graph --all -5
* 4d2551a (HEAD -> master) main branch fix
| * dae37f0 (new_feature) new features
|/
* 329aab3 (origin/master) Update README.md
* d2c1eb2 Rename README.MD to README.md
* f0ac494 some minor change

# начинаем мерджить и получаем сообщение о конфликтах
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master)
$ git merge new_feature
Auto-merging web_pages/page2.html
CONFLICT (content): Merge conflict in web_pages/page2.html
CONFLICT (modify/delete): web_pages/page3.html deleted in HEAD and modified in new_feature.  Version new_feature of web_pages/page3.html left in tree.
Automatic merge failed; fix conflicts and then commit the result.

# отменяем начатое слияние
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master|MERGING) # изменена строка приглашения
$ git merge --abort

# смотрим, что вернулись в состояние до начала слияния
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn (master) # строка приглашения вернулась
$ git log --oneline --graph --all -5
* 4d2551a (HEAD -> master) main branch fix
| * dae37f0 (new_feature) new features
|/
* 329aab3 (origin/master) Update README.md
* d2c1eb2 Rename README.MD to README.md
* f0ac494 some minor change
```

## Ручное решение конфликта