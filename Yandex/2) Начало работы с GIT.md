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

Добвляем файлы, чтобы отслеживать состояние обоих.  
```Bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ git add todo.txt && git add readme.txt
# добавляем файлы

user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ git status
# проверяем статус

On branch main  
Your branch is up to date with 'origin/main'.  
Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        new file:   readme.txt  
        new file:   todo.txt 
```

Теперь файлы отслеживаются и готовы к сохранению. Но сохранения пока не произошло, потому что команда git add только запоминает текущее содержимое (контент) файла.

Команда ```git add``` не сохраняет содержимое файлов в репозитории. Само сохранение, или фиксацию состояния файлов, называют коммитом (от англ. commit — «совершать», «фиксировать»). «Сделать коммит» значит сохранить текущую версию файла.  
 Если провести аналогию, команду ```git add``` можно сравнить с добавлением товаров в корзину в интернет-магазине, а коммит — с оформлением и оплатой заказа.

 Если сейчас отредактировать любой из «зелёных» файлов в папке first-project, он перейдёт в состояние modified (англ. «изменённый») и будет и в «зелёном», и в «красном» списках.   
 Например, откройте файл todo.txt в любом редакторе (подойдёт даже блокнот) и напишите в нём: 1. Пройти пару уроков по Git.  

```Bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ git status
# проверяем статус после внесения изменения в файл

On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   todo.txt

no changes added to commit (use "git add" and/or "git commit -a")
```  

Красным отмечена версия с текстом 1. Пройти пару уроков по Git.
Чтобы запомнить новое состояние файла, нужно снова ввести команду git add и передать в качестве параметра имя изменённого файла.  

```Bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ git add todo.txt
# добавляем обновленный файл

user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ git status
# првоеряем статус

On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   todo.txt # файл изменен
```

## Делаем первый коммит  

**Коммит в Git** — это ключевая операция в системе контроля версий, которая позволяет разработчикам фиксировать изменения, внесенные в код проекта. Коммит создает снимок текущего состояния проекта, включая все изменения, и сохраняет его в репозитории. Это позволяет отслеживать историю изменений, возвращаться к предыдущим версиям и управлять процессом разработки.  

Выполнить коммит — ```git commit```  
Сделать коммит можно командой ```git commit``` c ключом ```-m``` (от англ. message — «сообщение»), который присваивает коммиту сообщение.  

```Bash
$ git commit -m 'Добавляем файлы в репозиторий'
```

## Ещё раз о разнице между ```git add``` и ```git commit```  
Сначала команда ```git add``` сообщает Git, какие именно файлы нужно сохранить и какую их версию. Затем с помощью команды ```git commit``` происходит само сохранение.   

## Просматриваем историю коммитов  

Просмотреть историю коммитов — ```git log```  
Обратите внимание, что по умолчанию git log выводит коммиты в обратном хронологическом порядке — последние коммиты оказываются первыми сверху. В этом можно убедиться, если посмотреть на дату и время их создания.   

```Bash
user@WIN-CVKT899RCS2 MINGW64 /d/git/git/yandex (main)
$ git log
commit ab38203710c162a75d51aad3e918f3fc1a265334 (HEAD -> main, origin/main, origin/HEAD)
Author: Korablin Roman <*****@gmail.com>
Date:   Thu Mar 27 21:26:38 2025 +0700

    Добавляем файлы в репозиторий 3

commit 09cbc0bb06006964eb34601fa2d621e166e8a560
Author: Korablin Roman <*****@gmail.com>
Date:   Thu Mar 27 21:26:12 2025 +0700

    Добавляем файлы в репозиторий 2

commit f42bd56d333f9fa44556b350c0446c41bd379aec
Author: Korablin Roman <*****@gmail.com>
Date:   Thu Mar 27 21:14:30 2025 +0700

    Добавляем файлы в репозиторий 1

commit 32d81676a0ffbfb27c122df7fc20f8f6595745de
Author: Korablin Roman <*****@gmail.com>
Date:   Sun Mar 23 20:43:21 2025 +0700
```

