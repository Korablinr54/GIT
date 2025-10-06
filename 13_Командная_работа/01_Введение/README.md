# Ветвление в Git

В системе контроля версий Git ветки представляют собой обособленные направления разработки. Их использование позволяет параллельно вести работу над различными функциональностями или версиями проекта, не внося изменений в основную кодобазую.

## Классификация веток

### Главная (Main/Master)
Содержит исключительно стабильный и проверенный код, готовый к развертыванию.

### Функциональные (Feature)
Предназначены для реализации новых возможностей и улучшений.

### Ветки исправлений (Bugfix)
Используются для устранения выявленных ошибок и недочетов.

### Релизные (Release)
Создаются для финальной подготовки и стабилизации версии перед выпуском.

### Экстренные исправления (Hotfix)
Необходимы для оперативного решения критических проблем в рабочей среде.

## Практический пример
Для одновременного создания новой ветки и переключения на него воспользуемся командой `git checkout -b <batch_name>`:  

<br>

Для начала просто взглянем на список файлов:  
```bash
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ ls
README.md  
debug.log               
web_pages/
css/       
rare_critical_logs.log
```

<br>

Теперь создадим новую ветку и переключимся на нее:  
```bash
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (master)
$ git checkout -b feature_poem
Switched to a new branch 'feature_poem'

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (feature_poem)
```  
Ключ `-b` указывает на необходимость создания новой ветки, без него мы просто переключимся на другую ветку если она существует или получим ошибку, если ветки нет.

Видим также, что теперь строка преглашения изменилась на `user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (feature_poem)`

Добавим файл со стихотворением и взгляем на содержимое репозитория.  
```bash
user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (feature_poem)
$ ls # вернем список файлов
README.md  
css/  
debug.log  
poem.txt # наш новый файл
rare_critical_logs.log  
web_pages/

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (feature_poem)
$ head -n 5 poem.txt # посмотрим на первые 5 строк файла
У лукоморья дуб зелёный (отрывок из поэмы «Руслан и Людмила»)
У лукоморья дуб зелёный;
Златая цепь на дубе том:
И днём и ночью кот учёный
Всё ходит по цепи кругом;

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (feature_poem)
$ git status
On branch feature_poem
Untracked files: # новый файл еще не отслеживается
  (use "git add <file>..." to include in what will be committed)
        poem.txt

nothing added to commit but untracked files present (use "git add" to track)

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (feature_poem)
$ git add . # добавляем новйы файл в индекс

user@WIN-CVKT899RCS2 MINGW64 ~/git_learn/first_project (feature_poem)
$ git commit -m 'add the poem' # коммитим
[feature_poem 01424c0] add the poem
 1 file changed, 34 insertions(+)
 create mode 100644 poem.txt
```