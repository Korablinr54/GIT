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

```