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