## Репозиторий для изученя Git

### Команды Bash
| Команда                                   | Синтаксис                                   | Описание                                                                                   |
|-------------------------------------------|---------------------------------------------|--------------------------------------------------------------------------------------------|
| `pwd`                                     | `pwd`                                       | Показать путь к текущей папке (print working directory).                                  |
| `cd`                                      | `cd имя_папки/имя_папки`                   | Сменить директорию (change directory).                                                    |
| `cd ~`                                    | `cd ~`                                      | Переместиться в домашнюю директорию.                                                      |
| `cd ..`                                   | `cd ..`                                     | Вернуться на уровень выше.                                                                 |
| `ls`                                      | `ls`                                        | Показать список всех файлов и папок в текущей директории (list directory contents).       |
| `ls -a`                                   | `ls -a`                                     | Показать список всех файлов и папок в текущей директории, в том числе скрытых.            |
| `touch`                                   | `touch имя_файла.расширение`                | Создать новый файл или обновить время последнего доступа и изменения файла.               |
| `mv`                                      | `mv старое_имя новое_имя`                  | Переименовать файл или папку (move).                                                      |
| `mv`                                      | `mv имя_файла директория`                  | Переместить файл или директорию.                                                          |
| `rm`                                      | `rm имя_файла`                             | Удалить файл (remove).                                                                     |
| `mkdir`                                   | `mkdir имя_папки`                          | Создать директорию в текущей папке (make directory).                                      |
| `mkdir -p`                                | `mkdir -p имя_папки/имя_папки`            | Создать иерархию директорий.                                                              |
| `rmdir`                                   | `rmdir имя_папки`                          | Удалить пустую директорию (remove directory).                                             |
| `rm -r`                                   | `rm -r имя_папки`                          | Удалить директорию и все ее содержимое.                                                  |
| `cp`                                      | `cp имя_файла имя_папки`                   | Скопировать файл в указанную директорию.                                                 |
| `cp -r`                                   | `cp -r имя_папки имя_папки`                | Скопировать папку в указанную директорию.                                                |
| `cat`                                     | `cat имя_файла`                            | Показать содержимое файла.                                                                 |
| `less`                                    | `less имя_файла`                           | Показать содержимое файла постранично (можно пролистывать).                               |
| `head`                                    | `head имя_файла`                           | Показать первые 10 строк содержимого файла.                                               |
| `tail`                                    | `tail имя_файла`                           | Показать последние 10 строк содержимого файла.                                           |

### Команды GIT
| Команда         | Синтаксис            | Описание                                                                                      |
|------------------|---------------------|-----------------------------------------------------------------------------------------------|
| `git init`       | `git init`          | Находясь в директории, которую мы хотим инициализировать как локальный репозиторий, нужно использовать команду `git init`. После чего в папке появится скрытая директория `.git`. |
| `git status`     | `git status`        | Показывает текущую рабочую ветку, статус файлов, изменения в области файлов и информацию о коммитах. |
| `git add`        | `git add` <имя файла> ... <имя файла> | Добавление файла в промежуточную область `index`. Когда вы вносите изменения в файлы вашего проекта, Git не добавляет их автоматически в следующий коммит. Вместо этого вам нужно явно указать, какие изменения вы хотите сохранить. Команда `git add` позволяет выбрать конкретные файлы или изменения, которые будут включены в следующий коммит. |
| `git commit -m`  | `git commit -m`     | Команда используется для создания нового коммита с указанием сообщения, описывающего изменения, которые были внесены в проект. |
| `git log`        | `git log`           | Выводит список коммитов в локальном репозитории в обратном хронологическом порядке.          |
| `git switch`        | `git switch <имя ветки>`          | Переключение на указанную ветку репозитория          |

### Команды для работы с ветками
|Команда|Синтаксис|Описание|
|----------------|------------------|--------------------------------------------------------------------------|
|git branch      |git branch  --all | Позволяеет увидеть список всех локальных веток в репозитории. --all позволяет увидеть как локальные так и удаленные ветки |
|git push -d     |git push <короткое_имя> -d <имя ветки>  | Удаляет указанную ветку в удаленном репозитории|

### Настройка подключения к удаленному репозиторию
|Команда|Синтаксис|Описание|
|----------------|-------------------------------------------------|--------------------------------------------------------------------------|
|git remote      |git remote                                       | выводит список подключений, коротких имен                                |
|git remote -v   |git remote -v                                    | выводит список подключений, имя + url                                    |
|git remote add  |git remote add <Короткое имя, напр origin> <URL> | используется для создания локальной копии удалённого репозитория         |


### Команды для работы с удаленным репозиторием
|Команда|Синтаксис|Описание|
|----------------|------------------|--------------------------------------------------------------------------|
|git clone       |git clone < URL > <Имя каталога>| используется для создания локальной копии удалённого репозитория         |
|git push        |git push <Короткое имя> <Имя ветки>         | отправка изменений из локального репозитория в удалённый|

