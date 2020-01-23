---
title: Git
templateKey: 'article-page'
order: 1
tags: git, version control, commit, branch
---

# Git

## Управление версиями

**git** - приложение для управления версиями кода. Он позволяет создавать слепки (*коммиты*) состояния вашего проекта в любой момент времени, к которым всегда можно будет вернуться. Все ваши коммиты хранятся в вашем локальном **репозитории**, который можно синхронизировать с онлайн-копией и локальными копиями других участников проекта.

## Локальный репозиторий

Любую папку можно превратить в локальный репозиторий с помощью команды
```
git init
```

Тогда в ней появится папка .git, в которой будет храниться информация о всех ваших коммитах.

## Инициализация пользователя

Для первоначальной настройки Git существует утилита **git config**. Она  позволяет просматривать и устанавливать параметры, контролирующие все аспекты работы Git'а и его внешний вид.

В начале работы следует установить имя пользователя и email.
````
git config --global user.name "vasya pupkin"
git config --global user.email pupkin_v@epam.com
````

Флаг **--global** устанавливает credentials для **всех** репозиториев git на устройстве. Если для каких-то отдельных проектов нужно указать другое имя или электронную почту, можно выполнить эту же команду без параметра --global в каталоге с нужным проектом.


Посмотреть весь список настроек:
```
git config --list
```

Также легко проверить значение конкретного ключа, выполнив **git config {ключ}**:
```
git config user.name
```

## Коммиты

## Добавление файлов в индекс

Для того, чтобы git знал, изменения каких файлов надо сохранить в коммит, надо сначала добавить их в **индекс**. Для добавления файлов используется команда
```
git add <файл или папка>
```

Чтобы добавить все файлы сразу, можно использовать
```
git add .
```

Чтобы убрать ненужный файл из индекса, используется команда
```
git reset <файл или папка>
```

Чтобы посмотреть, какие файлы добавлены в индекс, используется команда
```
git status
```

## .gitignore

Существует специальный файл, где указываются шаблоны файлов/директорий, изменения которых не нужно отслеживать. К ним, как правило, относятся автоматически генерируемые файлы (различные логи, результаты сборки программ, папка node _ modules и т.п.). Пример .gitignore:

````
# комментарий — эта строка игнорируется

# не обрабатывать файлы, имя которых заканчивается на .a
*.a

# игнорировать только файл TODO находящийся в корневом каталоге
/TODO

# игнорировать все файлы в каталоге build/
build/

# игнорировать все .txt файлы в каталоге doc/
doc/**/*.txt

````

## Создание коммита

Чтобы создать коммит, используется команда
```
git commit -m "Комментарий"
```

В комментарии стоит дать краткое описание изменений коммита, чтобы при взгляде на историю сразу было видно, зачем он был нужен.

## История коммитов

После создания коммит будет сохранен в **историю коммитов**. Чтобы посмотреть ее, используется команда
```
git log
```

История состоит из записей в формате
```
commit 0f942b50adf4ff536cd0fb11094a7cf8be4a3804
Author: vasya_pupkin <pupkin_v@epam.com>
Date:   Thu Aug 23 23:08:18 2018 +0300

    Fix potential security vulnerability
```

В самой первой строчке находится **хеш** коммита. Зная его, мы можем в любой момент перейти на него, и проект примет то состояние, в котором он был на момент создания этого коммита. Чтобы перейти на другой коммит, используется команда
```
git checkout <хеш коммита>
```

## Ветви

## Создание ветвей

Для удобства работы в команде, git предлагает механизм **ветвей**. До сих пор мы говорили о создании коммитов по цепочке, друг за другом, то есть работали лишь с одной ветвью. git позволяет в любой момент отпочковаться от основной ветви и создать еще одну с помощью команды
```
git checkout -b <название новой ветви>
```

Это может быть полезно, если, например, у вас есть стабильная версия проекта, которую надо всегда поддерживать в рабочем состоянии, но вам надо добавить некую фичу, которая может повлечь за собой ошибки. Можно оставить стабильную версию в основной ветке *master*, и создать новую *feature* для экспериментальной фичи.

Чтобы переходить с ветки на ветку, используется команда
```
git checkout <название ветви>
```

## Слияние ветвей

Когда фича будет готова и протестирована, ее можно слить обратно в основную ветку. Для этого надо перейти на ветку *master* и выполнить команду
```
git merge feature
```

Обычно слияние проходит автоматически без проблем, но если ветка *master* продолжала развиваться, то при слиянии могут возникнуть **конфликты**.

## Разрешение конфликтов

Если на двух ветках есть изменения в одних и тех же местах в одних и тех же файлах, возникают конфликты, которые необходимо решить вручную.
```
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Для этого надо открыть файл с конфликтом и найти в нем место, помеченное гитом:
```
<<<<<<< HEAD
hello world
=======
>>>>>>> feature
hello sailor
```

Симоволами <<<<<<< HEAD помечена версия в текущей ветке (*master*), а >>>>>>> feature - входящей ветке (*feature*). Чтобы разрешить конфликт, надо удалить этот сегмент и вписать в код нужную вам версию этих изменений. Можно выбрать одну из двух или написать нечто среднее.
После разрешения конфликта надо добавить измененные файлы в индекс
```
git add hello.txt
```

и создать коммит
```
git commit -m "merged feature into master, resolved conflict in hello.txt"
```

## Онлайн-репозитории

## Создание репозитория

До сих пор мы работали только с локальной версией репозитория. Чтобы полностью использовать все преимущества git, стоит создать его онлайн-копию. Существует много сервисов, которые позволяют это сделать, мы будем использовать *github*.
После регистрации станет доступка кнопка **new repository**. Далее надо выбрать *пустой репозиторий*.
Чтобы добавить онлайн-версию репозитория, используется команда
```
git remote add <название> <адрес репозитория>
```

Название можно выбрать любое, например, *origin*.

## Синхронизация

Чтобы синхронизировать онлайн-репозиторий с локальной версией, используется команда
```
git push
```

Она загрузит все локальные коммиты, которых нет в онлайн-версии, в удаленный репозиторий.
Чтобы загрузить онлайн-изменения, которых у вас еще нет локально, используется команда
```
git pull
```

## Pull request

Для облегчения ранее описанной ситуации с веткой *feature*, когда несколько человек работают над проектом, сервисы наподобие github предлагают **пулл реквесты**. Когда работа над веткой *feature* будет завершена, человек, работавший над ней, создает пулл реквест в ветку *master*. Другие люди могут посмотреть список изменений, указать на ошибки, оставить комментарии. Когда все ошибки будут исправлены, пулл реквест одобряется и вливается в целевую ветку.

## SSH key

SSH key (Secure Shell Key) – код, позволяющий удаленному серверу распознать КТО этот пользователь и КАКИЕ ПРАВА он имеет, и организовать безопасное соединение (например, в случае передачи файлов).

При работе с удалёнными репозиториями, SSH — самый распространённый вариант настройки удалённого доступа, быстрый и удобный. Настроив авторизацию в SSH по ключам, пропадает необходимость постоянно вводить пароли для доступа к репозиторию, при этом сохраняя приемлемый уровень безопасности.

Состоит из двух частей – публичной (id _ rsa.pub) и приватной (id _ rsa). Публичную можно передавать другим пользователям, приватная должна храниться на локальном компьютере пользователя.

Создать ключ и добавить его в ssh-agent для аутентификации на github можно по [инструкции](https://help.github.com/en/enterprise/2.16/user/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)