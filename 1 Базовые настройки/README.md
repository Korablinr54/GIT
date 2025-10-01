# 1. Базовые настройки

| Команда | Описание | Дополнительные флаги/примеры |
|---------|----------|------------------------------|
| `git config --global user.name "Ваше Имя"` | Установка имени пользователя (для всех репозиториев) | |
| `git config --global user.email "ваш@email.com"` | Установка email пользователя (для всех репозиториев) | |
| `git config --global core.editor "редактор"` | Установка текстового редактора по умолчанию | Например: `nano`, `vim`, `code --wait` |
| `git config --global init.defaultBranch main` | Установка имени ветки по умолчанию | |
| `git config --global color.ui auto` | Включение цветного вывода в терминале | |
| `git config --global alias.сокращение "команда"` | Создание алиасов для команд | Пример: `git config --global alias.st "status -s"` |
| `git config --list` | Просмотр всех текущих настроек | `--show-origin` - показывает источник настроек |
| `git init` | Инициализация нового репозитория | `--bare` - для создания bare-репозитория |
| `git clone <url>` | Клонирование существующего репозитория | `--depth 1` - поверхностное клонирование |
| `git config --global core.editor "notepad"` | Изменение редактора ввода коммита по умолчанию | или `git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"` |
