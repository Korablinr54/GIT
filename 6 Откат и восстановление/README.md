# 6. Откат и восстановление

## Рабочая директория
| Команда | Синтаксис | Описание |
|---------|-----------|----------|
| `git restore` | `git restore файл` | Откатить изменения в файле |
| `git restore .` | `git restore .` | Откатить все изменения |

## Индекс (Staging Area)
| Команда | Синтаксис | Описание |
|---------|-----------|----------|
| `git restore --staged` | `git restore --staged файл` | Убрать файл из индекса |
| `git restore --staged .` | `git restore --staged .` | Убрать все файлы из индекса |
| `git reset` | `git reset` | Сбросить индекс |
| `git reset файл` | `git reset файл` | Убрать конкретный файл |

## История коммитов
| Команда | Синтаксис | Описание |
|---------|-----------|----------|
| `git commit --amend` | `git commit --amend` | Добавить изменения к последнему коммиту |
| `git commit --amend --no-edit` | `git commit --amend --no-edit` | Без изменения сообщения |
| `git commit --amend -m` | `git commit --amend -m "новое"` | Изменить сообщение |

## Reset
| Команда | Синтаксис | Описание |
|---------|-----------|----------|
| `git reset --soft HEAD~1` | `git reset --soft HEAD~1` | Откат с сохранением индекса |
| `git reset --mixed HEAD~1` | `git reset --mixed HEAD~1` | Откат с сохранением рабочей директории |
| `git reset --hard HEAD~1` | `git reset --hard HEAD~1` | Полный откат |
| `git reset HEAD~2` | `git reset HEAD~2` | Отменить 2 коммита |

## Revert и восстановление
| Команда | Синтаксис | Описание |
|---------|-----------|----------|
| `git revert` | `git revert хеш_коммита` | Создать обратный коммит |
| `git revert старший..младший` | `git revert старший_хеш..младший_хеш` | Отменить диапазон коммитов |
| `git reflog` | `git reflog` | История перемещений HEAD |
| `git reset --hard ORIG_HEAD` | `git reset --hard ORIG_HEAD` | Вернуться к состоянию до операции |
| `git reset --hard <commit_id>` | `git reset --hard <commit_id>` | Вернуться к коммиту из reflog |
