# Просмотрм изменений: `git diff`
В самом просто варианте команда git diff покажет нам изменения между последним коммитом и измененным файлом, который мы еще даже не добавили в индекс. Приведем пример:  
```bash
git diff

diff --git a/README.md b/README.md
index 6a0f1e6..f60c61b 100644
--- a/README.md
+++ b/README.md
@@ -15,4 +15,6 @@

-- [8. Очистка рабочей директории](./8%20Очистка%20рабочей%20директории/README.md) # последняя строка в старой версии файла README.md
\ No newline at end of file
+- [8. Очистка рабочей директории](./8%20Очистка%20рабочей%20директории/README.md)
+
+Тестовая строка для првоерки git diff # новая версия файла
\ No newline at end of file # эта строка была добавлена
```

Таким образом команда `git diff` в базовом случае отобразит изменения, которые были внесены но не добавлены в индекс.  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git add .

user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git diff

user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$

# после добавления изменений в индекс команда git diff, в ее базовом варианте перестанет возвращать какую-либо информацию.
```
## git diff HEAD
`git diff HEAD` показывает все изменения по сравнению с последним коммитом, включая как добавленные в индекс, так и нет:  
```bash
git diff HEAD

diff --git a/README.md b/README.md
index 6a0f1e6..f60c61b 100644
--- a/README.md
+++ b/README.md
@@ -15,4 +15,6 @@

-- [8. Очистка рабочей директории](./8%20Очистка%20рабочей%20директории/README.md) # последняя строка в старой версии файла README.md
\ No newline at end of file
+- [8. Очистка рабочей директории](./8%20Очистка%20рабочей%20директории/README.md)
+
+Тестовая строка для првоерки git diff # новая версия файла
\ No newline at end of file # эта строка была добавлена
```
## `git diff --staged`
`git diff --staged` — показывает изменения, которые уже добавлены в индекс (staging area) и готовы к коммиту.
```bash
git diff --staged

diff --git a/README.md b/README.md
index 6a0f1e6..f60c61b 100644
--- a/README.md
+++ b/README.md
@@ -15,4 +15,6 @@

-- [8. Очистка рабочей директории](./8%20Очистка%20рабочей%20директории/README.md) # последняя строка в старой версии файла README.md
\ No newline at end of file
+- [8. Очистка рабочей директории](./8%20Очистка%20рабочей%20директории/README.md)
+
+Тестовая строка для првоерки git diff # новая версия файла
\ No newline at end of file # эта строка была добавлена
```
## `git diff head^`
В случае, если мы изменения уже закоммитили а хотим посмотреть изменения между последним и предпоследним коммитами, или любым другим количеством коммитов то используем `git diff head^` или `git diff head~1, 2, 3 и тд`. Напомню, что `HEAD` - уазатель на последний коммит, `HEAD^` или `HEAD~1` это указатель на предыдущий коммит. Либо использовать hash-код вместо `HEAD^`.  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/GIT/GIT (main)
$ git add . # добавдяем измененяи в индекс

user@WIN-CVKT899RCS2 MINGW64 /d/GIT/GIT (main)
$ git commit -m'git diff practice_2' # коммитим
[main 83f69d9] git diff practice_2
 1 file changed, 1 insertion(+), 1 deletion(-)

user@WIN-CVKT899RCS2 MINGW64 /d/GIT/GIT (main)
$ git diff head # даже с head мы не видим изменения

user@WIN-CVKT899RCS2 MINGW64 /d/GIT/GIT (main)
$ git diff head^ # возвращаем измененяи между последним коммитом и коммитом -1 от последнего
diff --git a/README.md b/README.md
index 6234a84..7153cb4 100644
--- a/README.md
+++ b/README.md
@@ -18,4 +18,4 @@
 - [8. Очистка рабочей директории](./8%20Очистка%20рабочей%20директории/README.md)
 - [9. Просмотр изменений](./9%20Просмотр%20изменений/README.md)

-Тестовая строка для првоерки git diff # старая строка
\ No newline at end of file
+Тестовая строка для првоерки git diff - изменения # новая строка
\ No newline at end of file
```