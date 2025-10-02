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
# Просмотр изменений между коммитами
Давайте использую hash коммитов посомтрим между ними разницу.  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/GIT/GIT (main)
$ git log --oneline # выведем лог комитов 
fd8f124 (HEAD -> main, origin/main, origin/HEAD) git diff practice_3
83f69d9 git diff practice_2
1b34c2b git diff practice
9806438 git diff learn
bbd15b8 x
52bb25a git diff
b819a44 new structure
a6db07d new structure
88a6792 new structure
2833ece update
c37e895 update
8ac8680 test
b688f69 git mv
c652492 test

user@WIN-CVKT899RCS2 MINGW64 /d/GIT/GIT (main)
$ git diff 1b34c2b 9806438 # посмотрим на разницу между двумя коммитами, выберем их по hash

# информация об изменении
diff --git a/9 Просмотр изменений/README.md b/9 Просмотр изменений/README.md
index c63a751..aba99da 100644
--- a/9 Просмотр изменений/README.md
+++ b/9 Просмотр изменений/README.md
@@ -17,52 +17,3 @@ index 6a0f1e6..f60c61b 100644
 \ No newline at end of file # эта строка была добавлена
```

# Просмотр изменений одного конкретного файла
Давайте посмотрим как менялся какой-то конкретный файл.  
```bash
$ git diff HEAD~1 "9 Просмотр изменений/README.md" # просмотр изменений конкретного файла

# вывод команды
diff --git a/9 Просмотр изменений/README.md b/9 Просмотр изменений/README.md
index dd361af..1d1a1fc 100644
--- a/9 Просмотр изменений/README.md
+++ b/9 Просмотр изменений/README.md
@@ -94,4 +94,43 @@ index 6234a84..7153cb4 100644
 \ No newline at end of file
 +Тестовая строка для првоерки git diff - изменения # новая строка
```

# Просмотр изменений: `git show`
В самом базовом варианте команда `git show` вернет аналогичный результат команде `git diff HEAD^ / HEAD~1`.
`git show`
```bash
$ git show #
commit ea8d799532165654fab5e68c4aa40808b5da5410 (HEAD -> main, origin/main, origin/HEAD)
Author: Korablin Roman <korablinr22@gmail.com>
Date:   Wed Oct 1 20:30:59 2025 +0700

    git diff practice_5

diff --git a/9 Просмотр изменений/README.md b/9 Просмотр изменений/README.md
index 1d1a1fc..1c41cc1 100644
--- a/9 Просмотр изменений/README.md
+++ b/9 Просмотр изменений/README.md
@@ -125,12 +125,19 @@ index c63a751..aba99da 100644
 +++ b/9 Просмотр изменений/README.md
 @@ -17,52 +17,3 @@ index 6a0f1e6..f60c61b 100644
  \ No newline at end of file # эта строка была добавлена
```
и `git diff HEAD~1`
```bash
$ git diff HEAD~1
diff --git a/9 Просмотр изменений/README.md b/9 Просмотр изменений/README.md
index 1d1a1fc..1c41cc1 100644
--- a/9 Просмотр изменений/README.md
+++ b/9 Просмотр изменений/README.md
@@ -125,12 +125,19 @@ index c63a751..aba99da 100644
 +++ b/9 Просмотр изменений/README.md
 @@ -17,52 +17,3 @@ index 6a0f1e6..f60c61b 100644
  \ No newline at end of file # эта строка была добавлена
```
Как мы видими `git show` в базовом виде работает по аналогии с `git diff HEAD^ / HEAD~1`.  