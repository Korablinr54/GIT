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

