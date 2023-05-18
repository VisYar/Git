## Шпаргалка для работы с GIT

[GIT команды](https://somov-qa.github.io/projects/git/index.html)

***Полезные ссылки:***
- **[GIT documentation](https://git-scm.com/doc)**

### **Коротко о том, что такое GIT?**

***GIT*** – это распределенная система контроля версий, то есть — это система, записывающая изменения в файл или набор файлов в течение времени и позволяющая вернуться позже к определённой версии. Мы хотим гибко управлять некоторым набором файлом, откатываться до определенных версий в случае необходимости. Можно отменить те или иные изменения файла, откатить его удаление, посмотреть кто что поменял. Как правило, системы контроля версий применяются для хранения исходного кода, но это необязательно. Они могут применяться для хранения файлов совершенно любого типа.
Такая Система контроля версий помогает отслеживать изменения, внесённые в базу кода. Более того, Система записывает, кто внёс изменения и может восстановить стёртый или изменённый код.
Перезаписанных кодов не существует, поскольку ***Git*** сохраняет несколько копий в хранилище, то есть говоря простым языком, если вы решили накосячить где-нибудь на проекте внеся свои изменения, то можно вернуть в рабочее русло исправный билд проекта.

## Основные команды
*Чтобы найти нужную команду, нажмите ***Ctrl+F***, если у вас Windows, или ***Cmd+F***, если у вас MacOS, а затем введите название команды.

#### Работа с репозиторием на локальном компьютере

```bash
git init # создать новый проект в текущей директории
git clone # клонировать удаленный репозиторий
```

#### Просмотр изменений

```bash
git status # показать состояние репозитория (отслеживаемые, изменённые, новые файлы)
git diff # сравнить рабочую директорию и индекс
git diff --color-words  # сравнить рабочую директорию и индекс, показать отличия в словах
git diff master branch_name # посмотреть что сделано в ветке branch_name по сравнению с веткой master
```
#### Логи

```bash
Git log                                           # покажет список произведенных изменений
git log --pretty=oneline                          # однострочная история
git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short
--pretty="..."                                    # определяет формат вывода; 
%h                                                # укороченный hash коммита; 
%d                                                # дополнения коммита («головы» веток или теги);
%ad                                               # дата коммита; 
%s                                                # комментарий; 
%an                                               # имя автора; 
--graph                                           # отображает дерево коммитов в виде ASCII-графика; 
--date=short # сохраняет формат даты коротким и симпатичным
```
#### Добавление изменений в индекс

```bash
git add . # добавить в индекс все новые, изменённые, удалённые файлы из текущей директории и её поддиректорий
git add file_name # добавить в индекс указанный файл (был изменён, был удалён или это новый файл)
git add -i # запустить интерактивную оболочку для добавления в индекс только выбранных файлов
git add -p # показать новые/изменённые файлы по очереди с указанием их изменений и вопросом об отслеживании/индексировании
```

#### Удаление изменений из индекса

```bash
git reset # убрать из индекса все добавленные в него изменения
git reset file_name # убрать из индекса изменения указанного файла
```

#### Отмена изменений

```bash
git checkout file_name # отменить изменения в файле, вернуть состояние файла, имеющееся в индексе
git reset --hard # отменить изменения; вернуть то, что в коммите, на который указывает HEAD
git clean -df # удалить неотслеживаемые файлы и директории
```

#### Коммиты

```bash
git commit -m "Commit message" # зафиксировать в коммите проиндексированные изменения + добавить имя коммита
git commit -am "Commit message" # проиндексировать отслеживаемые файлы (только отслеживаемые) и закоммитить + добавить имя коммита
```

#### Отмена коммитов и перемещение по истории
Все коммиты, которые уже были отправлены в удалённый репозиторий, должны отменяться новыми коммитами (***git revert***), дабы избежать проблем с историей разработки у других участников проекта.

```bash
git revert HEAD --no-edit # создать новый коммит, отменяющий изменения последнего коммита без запуска редактора сообщения
```

Все команды, приведённые ниже, можно выполнять только если коммиты еще не были отправлены в удалённый репозиторий.

```bash
git commit --amend -m "commit message"  # «перекоммитить» изменения последнего коммита, заменить его новым коммитом с другим сообщением
git reset --hard @~ # передвинуть HEAD (и ветку) на предыдущий коммит, рабочую директорию и индекс сделать такими, какими они были в момент предыдущего коммита
git reset --soft @~ # передвинуть HEAD (и ветку) на предыдущий коммит, но в рабочей директории и индексе оставить все изменения
```

#### Переключиться на другой коммит и продолжить работу с него
Потребуется создание новой ветки, начинающейся с указанного коммита.

```bash
git checkout -b new_branch 5589877   # создать ветку new_branch, начинающуюся с коммита c хешем 5589877 (переместить HEAD на указанный коммит, рабочую директорию вернуть к состоянию, на момент этого коммита, создать указатель на этот коммит (ветку) с указанным именем)
```
#### Временно переключиться на другой коммит
```bash
git checkout b9533bb # переключиться на коммит с указанным хешем (переместить HEAD на указанный коммит, рабочую директорию вернуть к состоянию, на момент этого коммита)
git checkout master # переключиться на коммит, на который указывает master (переместить HEAD на коммит, на который указывает master, рабочую директорию вернуть к состоянию на момент этого коммита)
```
#### Копирование коммита (перенос коммитов)

```bash
git cherry-pick 5589877 # скопировать на активную ветку изменения из указанного коммита, закоммитить эти изменения
git cherry-pick master~2..master # скопировать на активную ветку изменения из master (2 последних коммита)
git cherry-pick -n 5589877 # скопировать на активную ветку изменения из указанного коммита, но НЕ КОММИТИТЬ (подразумевается, что мы сами потом закоммитим)
```

#### Удаление файла

```bash
git rm file_name # удалить отслеживаемый неизменённый файл и проиндексировать это изменение
git rm -f file_name # удалить отслеживаемый изменённый файл и проиндексировать это изменение
git rm -r log/ # удалить всё содержимое отслеживаемой директории log/ и проиндексировать это изменение
git rm ind* # удалить все отслеживаемые файлы с именем, начинающимся на «ind» в текущей директории и проиндексировать это изменение
```

#### Перемещение/переименование файлов
Для ***GIT*** не существует переименования. Переименование воспринимается как удаление старого файла и создание нового. Факт переименования может быть определен только после индексации изменения.

```bash
git mv file_name new_file_name # переименовать файл «file_name» в «new_file_name» и проиндексировать это изменение
git mv file_name folder/ # переместить файл file_name в директорию folder/ (должна существовать) и проиндексировать это изменение
```

#### История коммитов

```bash
git log master # показать коммиты в указанной ветке
git log -2 # показать последние 2 коммита в активной ветке
git log -2 --stat # показать последние 2 коммита и статистику внесенных ими изменений
git log -p -22 # показать последние 22 коммита и внесенную ими разницу на уровне строк
git log --since=2.weeks # показать коммиты за последние 2 недели
git log --after '2022-07-25' # показать коммиты, сделанные после указанной даты
git log file_name # показать историю изменений файла file_name (только коммиты)
git log -5 file_name # показать историю изменений файла, последние 5 коммитов (только коммиты)
git log -p file_name # показать историю изменений файла (коммиты и изменения)
git log --grep qwerty -i # показать коммиты, в описании которых есть буквосочетание qwerty (НЕ регистрозависимо, только коммиты текущей ветки)
git show 60d6582 # показать изменения из коммита с указанным хешем
git show HEAD~ # показать данные о предыдущем коммите в активной ветке
```

#### Ветки

```bash
git branch # показать список веток
git branch -v # показать список веток и последний коммит в каждой
git branch new_branch # создать новую ветку с указанным именем на текущем коммите
git branch new_branch 5589877 # создать новую ветку с указанным именем на указанном коммите
git checkout new_branch # перейти в указанную ветку
git checkout -b new_branch # создать новую ветку с указанным именем и перейти в неё
git merge branch_name # влить в ветку, в которой находимся, данные из ветки branch_name
git merge branch_name -m "Commit message" # влить в ветку, в которой находимся, данные из ветки branch_name (указано сообщение коммита слияния)
git branch -d branch_name # удалить ветку branch_name (используется, если её изменения уже влиты в главную ветку)
git branch -a # показать все имеющиеся ветки (в т.ч. на удаленных репозиториях)
git branch -m branch_name new_branch_name # переименовать локально ветку branch_name в new_branch_name
```

#### Теги

```bash
git tag v1.0.0 # создать тег с указанным именем на коммите, на который указывает HEAD
git tag -d tag_name # удалить тег с указанным именем
git tag -n # показать все теги, и по 1 строке сообщения коммитов, на которые они указывают
git tag -n -l 'name*' # показать все теги, которые начинаются с 'name*'
```

#### Временное сохранение изменений без коммита

```bash
git stash # временно сохранить незакоммиченные изменения и убрать их из рабочей директории
git stash pop # вернуть сохраненные командой git stash изменения в рабочую директорию
```

#### Удалённые репозитории

Есть два распространённых способа привязать удалённый репозиторий к локальному: по ***HTTPS*** и по ***SSH***. Если ***SSH*** у вас не настроен (или вы не знаете что это), привязывайте удалённый репозиторий по ***HTTPS*** (адрес привязываемого репозитория должен начинаться с ***https://***).

```bash
git remote -v # показать список удалённых репозиториев, связанных с локальным
git remote add origin https://github.com:nicothin/test.git # добавить удалённый репозиторий (с сокр. именем origin) с указанным URL
git remote rm origin # удалить привязку удалённого репозитория
git fetch origin # скачать все ветки с удаленного репозитория (с сокр. именем origin), но не сливать со своими ветками
git push origin master # отправить в удалённый репозиторий (с сокр. именем origin) данные своей ветки master
git pull origin # влить изменения с удалённого репозитория (все ветки)
git pull origin master # влить изменения с удалённого репозитория (только указанная ветка)
```

#### Конфликт слияния

Предполагается ситуация: есть ветка ***master*** и есть ветка ***branch***. В обеих ветках есть коммиты, сделанные после расхождения веток. В ветку ***master*** пытаемся влить ветку ***branch*** (***git merge branch***), получаем конфликт, т.к. в обеих ветках есть изменения одной и той же строки в файле ***file_name***.
При возникновении конфликта, репозиторий находится в состоянии прерванного слияния. Нужно оставить в конфликтующих местах файлов только нужный код, проиндексировать изменения и закоммитить.

```bash
git merge branch # влить в активную ветку изменения из ветки branch
git merge-base master branch # показать хеш последнего общего коммита для двух указанных веток
git reset --hard # прекратить это прерванное слияние, вернуть рабочую директорию и индекс как было в момент коммита, на который указывает HEAD, а я пойду немного поплачу
git reset --merge # прекратить это прерванное слияние, но оставить изменения, не закоммиченные до слияния (для случая, когда слияние делается не на чистом статусе)
```

#### «Перенос» ветки

```bash
git rebase master # перенести все коммиты (создать их копии) активной ветки так, будто активная ветка ответвилась от master на нынешней вершине master (часто вызывает конфликты)
git rebase --onto master branch # перенести коммиты активной ветки на master, начиная с того места, в котором активная ветка отделилась от ветки branch
git rebase --abort # прервать конфликтный rebase, вернуть рабочую директорию и индекс к состоянию до начала rebase
git rebase --continue # продолжить конфликтный rebase (сработает только после разрешения конфликта и индексации такого разрешения)
```

#### Как отменить rebase

```bash
git reflog branch -2 # смотрим лог перемещений ветки, которой делали rebase (в этом примере — branch), видим последний коммит ПЕРЕД rebase, на него и нужно перенести указатель ветки
git reset --hard branch@{1} # переместить указатель ветки branch на один коммит назад, обновить рабочую директорию и индекс
```
