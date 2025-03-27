# Первый коммит
## Работа с файлом настройки .gitconfig

В качестве значения user.name нужно указать своё имя или никнейм. Для настройки параметра user.email указывают электронную почту.

```Bash
$ git config --global user.name "User Namovich" 
# имя или ник нужно написать латиницей и в кавычках

$ git config --global user.email username@yandex.ru
# здесь нужно указать свой настоящий email
```

Вывести содержимое файла конфигурации Git той же командой ```git config``` с флагом ```--list``` или ```cat ~/.gitconfig```    
```Bash
$ git config --list
```

## Инициализируем репозиторий
### Сделать папку репозиторием — git init

Для начала отслеживания изменений в проекте через **Git** необходимо превратить папку проекта в репозиторий. Для этого выполните следующие шаги:  
* Перейдите в каталог проекта (папку с файлами).  
* Инициализируйте репозиторий с помощью команды ```git init```, которая создаёт скрытую директорию ```.git``` для хранения истории изменений.  

Таким образом, **Git** начнёт фиксировать изменения в файлах проекта.

```Bash
$ cd ~/git/project # перешли в нужную папку

$ git init # создали репозиторий
```
#### Удалить репозиторий
Если вы случайно сделали Git-репозиторием не ту папку, её можно «разгитить». Для этого нужно удалить скрытую подпапку .git.  
```Bash
$ cd <папка с репозиторием> # перешли в папку

$ rm -rf .git # удалили подпапку .git
```
* **Ключ -r** (рекурсивное удаление) позволяет команде rm удалять не только файлы, но и вложенные каталоги с их содержимым. Это особенно полезно для очистки сложных структур директорий.  

* **Ключ -f** (принудительное удаление) отключает запросы подтверждения для каждого файла, автоматически принимая решение об удалении без диалогов.  

### Проверить состояние репозитория — git status

Выполнив команду ```git status``` мы получим следующее сообщение:  

```Bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/project (master)
$ git status
On branch master # название текущей ветки

No commits yet # сообщение о том, что коммитов в репозитории еще нет

nothing to commit (create/copy files and use "git add" to track) 
# прежде чем коммитить нужно что-то для начала создать
```
## Добавляем файлы в репозиторий

```Bash
user@WIN-CVKT899RCS2 MINGW64 ~
$ cd /d/git/git/yandex
# идем в папку с репозиторием

user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ ls
# смотрим на содержимое
'1) Знакомство с GIT.md'  '2) Начало работы с GIT.md'

user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ touch todo.txt && touch readme.txt
# создаем два файла

user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ ls
# убеждаемся, что файлы созданы
'1) Знакомство с GIT.md'      readme.txt
'2) Начало работы с GIT.md'   todo.txt

user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ git status
# проверяем статус репозитория
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.txt
        todo.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Git сообщит, что в папке ```yandex``` есть ```untracked files``` — ещё не отслеживаемые файлы readme.txt и todo.txt.