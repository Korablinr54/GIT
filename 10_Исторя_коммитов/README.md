# Просмотр статистики коммитов: `git log --stat`
Мы можем познакомиться со статистикой коммитов используя команду `git log` и ключом `--stat`. Такой вывод команды говорит нам о характере изменений и файлах,в  которых они происходили. Вот как это выглядит:
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git log --stat
commit 3196c5e8e90ba7fddb94ba016f4dc6f43f85ba02
Author: Roman <******@gmail.com>
Date:   Thu Oct 2 15:18:53 2025 +0700

    git show hash --name-only

 9 Просмотр изменений/README.md | 16 +++++++++++++++-
 1 file changed, 15 insertions(+), 1 deletion(-)

commit 7d9e7ba28b25c83460b7f6024eb869140cfd39db
Author: Roman <******@gmail.com>
Date:   Thu Oct 2 15:08:31 2025 +0700

    git show 3

 9 Просмотр изменений/README.md | 28 +++++++++++++++++++++++++++-
 1 file changed, 27 insertions(+), 1 deletion(-)

commit 69b0b63459a4c244b2127a61b9a37cee7a1e8522
Author: Roman <******@gmail.com>
Date:   Thu Oct 2 14:49:10 2025 +0700

    git diff 3
```
# Детальный просмотр наборов изменений: `git log -p`
Мы можем также вывести детальную информацию о внесенных изменения используя ключ `-p`. Это как `git show` но для каждого коммита по порядку:  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git log -p
commit 80d3954ad546956d51835dcef99cb64c67442f93 (HEAD -> main, origin/main, origin/HEAD)
Author: Roman <******@gmail.com>
Date:   Thu Oct 2 16:11:57 2025 +0700

    git log --stat

diff --git a/10 Исторя коммитов/README.md b/10 Исторя коммитов/README.md
index e69de29..53ed896 100644
--- a/10 Исторя коммитов/README.md
+++ b/10 Исторя коммитов/README.md
@@ -0,0 +1,29 @@
+# Просмотр статистики коммитов: `git log --stat`^M
+Мы можем познакомиться со статистикой коммитов используя команду `git log` и ключом `--stat`. Такой вывод команды говорит нам о характере изменений и файлах,в  которых они происходили. Вот как это выглядит:^M
+```bash^M
+user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)^M
+$ git log --stat^M
+commit 3196c5e8e90ba7fddb94ba016f4dc6f43f85ba02^M
+Author: Roman <******@gmail.com>^M
+Date:   Thu Oct 2 15:18:53 2025 +0700^M
+^M
+    git show hash --name-only^M
+^M
+ 9 Просмотр изменений/README.md | 16 +++++++++++++++-^M
+ 1 file changed, 15 insertions(+), 1 deletion(-)^M
+^M
+commit 7d9e7ba28b25c83460b7f6024eb869140cfd39db^M
+Author: Roman <******@gmail.com>^M
+Date:   Thu Oct 2 15:08:31 2025 +0700^M
+^M
+    git show 3^M
+^M
+ 9 Просмотр изменений/README.md | 28 +++++++++++++++++++++++++++-^M
+ 1 file changed, 27 insertions(+), 1 deletion(-)^M
+^M
+commit 69b0b63459a4c244b2127a61b9a37cee7a1e8522^M
+Author: Roman <******@gmail.com>^M
+Date:   Thu Oct 2 14:49:10 2025 +0700^M
+^M
+    git diff 3^M
+```
\ No newline at end of file
```
Вы также можете совмещать ключи `-p` & `--stat` для просомтра изменений и статистики:  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git log -p --stat
commit 80d3954ad546956d51835dcef99cb64c67442f93 (HEAD -> main, origin/main, origin/HEAD)
Author: Roman <******@gmail.com>
Date:   Thu Oct 2 16:11:57 2025 +0700

    git log --stat
---
 10 Исторя коммитов/README.md | 29 +++++++++++++++++++++++++++++
 1 file changed, 29 insertions(+)

diff --git a/10 Исторя коммитов/README.md b/10 Исторя коммитов/README.md
index e69de29..53ed896 100644
--- a/10 Исторя коммитов/README.md
+++ b/10 Исторя коммитов/README.md
@@ -0,0 +1,29 @@
+# Просмотр статистики коммитов: `git log --stat`^M
+Мы можем познакомиться со статистикой коммитов используя команду `git log` и ключом `--stat`. Такой вывод команды говорит нам о характере изменений и файлах,в  которых они происходили. Вот как это выглядит:^M
+```bash^M
+user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)^M
+$ git log --stat^M
+commit 3196c5e8e90ba7fddb94ba016f4dc6f43f85ba02^M
+Author: Roman <******@gmail.com>^M
+Date:   Thu Oct 2 15:18:53 2025 +0700^M
+^M
```

# Ограничить вывод количества коммитов: `-p`
В базовом варианте команда `git log --stat` вернет тстатистику по всем коммитам, но если нам надо посмотреть только 2 последних коммита? Нужно делать так:  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git log --stat -2
commit c1d881412ab849e7f7ebfba054516b21b5a7949c (HEAD -> main, origin/main, origin/HEAD)
Author: Roman <******@gmail.com>
Date:   Thu Oct 2 16:17:07 2025 +0700

    git log -p --stat

 10 Исторя коммитов/README.md |  75 +++++++++++
 ertions(+)                   | 314 +++++++++++++++++++++++++++++++++++++++++++
 2 files changed, 389 insertions(+)

commit 80d3954ad546956d51835dcef99cb64c67442f93
Author: Roman <******@gmail.com>
Date:   Thu Oct 2 16:11:57 2025 +0700

    git log --stat

 10 Исторя коммитов/README.md | 29 +++++++++++++++++++++++++++++
 1 file changed, 29 insertions(+)
```
# Визуализация графов: `--graph`
Ключ `--graph` позволяет вывести коммиты с псевдовизуализацией веток, пока у нас одна ветка, это омжет быть не так полезно:  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git log --graph -3
* commit ed11c5e1c0bdf6ef153520d20e658fed11a92366 (HEAD -> main, origin/main, origin/HEAD)
| Author: Roman <****@gmail.com>
| Date:   Thu Oct 2 16:22:26 2025 +0700
|
|     git log --stat -2
|
* commit c1d881412ab849e7f7ebfba054516b21b5a7949c
| Author: Roman <****@gmail.com>
| Date:   Thu Oct 2 16:17:07 2025 +0700
|
|     git log -p --stat
|
* commit 80d3954ad546956d51835dcef99cb64c67442f93
| Author: Roman <****@gmail.com>
| Date:   Thu Oct 2 16:11:57 2025 +0700
|
|     git log --stat
```
Как видим, по левой кромке сообщений видна как бы ветка или линия коммитов.  

# Просмотр коммитов конкретного файла
Посмотреть историю по одному конкретному файлу можно передав его имя в команду:
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git log --oneline '10 Исторя коммитов/README.md' #
26e68e3 (HEAD -> main, origin/main, origin/HEAD) git log --graph
ed11c5e git log --stat -2
c1d8814 git log -p --stat
80d3954 git log --stat
f09ee29 10 commit hostory
```

# Просмотр коммитов конкретного автора
Мы можем посмотреть коммиты конкретного автора. Например, отобразим в кратком виде три последние коммита автора Roman. щапись будет следующая:  
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$ git log --author='Roman' --oneline -3
e6decc5 (HEAD -> main, origin/main, origin/HEAD) git log
26e68e3 git log --graph
ed11c5e git log --stat -2
```

# Редактирование формата вывода: `pretty`
В случае, если нам нужно выводить не всю инфомрацию об истории коммитов, а что-то конкретно, например, автора, дату, краткий хэш и сообщение используем такую команду, к слову, `pretty` содержит множество атрибутов, которые лучше смотрет ьв документации:
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$  git log --pretty=format"%h - %an - %as -%s" -5
format90502a7 - Roman - 2025-10-02 -git log --author
formate6decc5 - Roman - 2025-10-02 -git log
format26e68e3 - Roman - 2025-10-02 -git log --graph
formated11c5e - Roman - 2025-10-02 -git log --stat -2
formatc1d8814 - Roman - 2025-10-02 -git log -p --stat
```
Либо нам надо добавит ьв выод пустые строки для разделения, тоже можно:
```bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git (main)
$  git log --pretty=format"%h - %an - %as - %n - %s %n" -5
format4cdd758 - Roman - 2025-10-02 -
 - git log pretty # текст коммита на новой строке
# пустая строка разделитель
format90502a7 - Roman - 2025-10-02 -
 - git log --author

formate6decc5 - Roman - 2025-10-02 -
 - git log

format26e68e3 - Roman - 2025-10-02 -
 - git log --graph

formated11c5e - Roman - 2025-10-02 -
 - git log --stat -2
```
# Фильтрация по дате
Коммиты ПОСЛЕ указанной даты:  
```bash
git log --since="ГГГГ-ММ-ДД"
```

Коммиты ДО указанной даты:  
```bash
git log --until="ГГГГ-ММ-ДД"
```

Коммиты за определенный период:  
```bash
# Коммиты за октябрь 2025
git log --since="2025-10-01" --until="2025-10-31"
```
Комбинируется с другими ключами.  

<br>

**Поддерживаемые форматы дат**:  
- `"ГГГГ-ММ-ДД"` - конкретная дата  
- `"ГГГГ-ММ-ДД ЧЧ:ММ:СС"` - дата и время  
- `"N days/weeks/months ago"` - относительное время  
- `"yesterday"`, `"today"`, `"now"` - специальные значения  