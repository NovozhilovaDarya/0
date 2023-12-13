## git push 

Это отправка данных на сервер, в удаленный репозиторий, на github. 

*Данные* - это коммиты и ветки.

Зачем ***push*** на сервер
- для работы в команде, чтобы делиться своим кодом с коллегами
- чтобы иметь резервную копию на случай потери данных на своей машине


Когда выполнять команду ***push***

Когда сделали новый коммит или несколько коммитов

### Как узнать, что есть незапушенные коммиты

В командной строке набрать ***git status***


    $ git status
    
    On branch master
    $ git status
    On branch master
    Your branch is ahead of 'origin/master' by 5 commits.
      (use "git push" to publish your local commits)
    (use "git push" to publish your local commits)

Ключевая фраза здесь ***"Your branch is ahead of 'origin/master' by 5 commits."***. Это значит, что у нас есть 5 неотправленных на сервер коммитов.

 Если незапушенных коммитов нет, то картина будет такая


    $ git status
    
    On branch master
    Your branch is up-to-date with 'origin/master'.


***"is up-to-date"*** означает, что у нас нет незапушенных коммитов

***master и origin/master***

Это ветки: локальная и удаленная (на сервере, в github). По умолчанию мы находимся в ветке master. 
 master - это то, что на нашей машине, а origin/master - в удаленном репозитории, на github.

git push в терминале

    $ git push origin master

push - что сделать, отправить
origin - куда, на сервер
master - что, ветку master

Что такое pull (пулл)

Это скачивание данных с сервера. Похоже на клонирование репозитория, но с той разницей, что скачиваются не все коммиты, а только новые.

Зачем пулиться с сервера
Чтобы получать изменения от ваших коллег. Или от себя самого, если работаете на разных машинах

***git pull*** в терминале

    $ git pull origin master

pull - что сделать, получить данные
origin - откуда, с сервера
master - а точнее, с ветки master

Когда что-то пошло не так...
Иногда при работе в команде git push и git pull могут вести себя не так, как пишут в учебниках. Рассмотрим примеры

***git push rejected***

Вы сделали новый коммит, пытаетесь запушить его, а **gi**t в ответ выдает такое


    $ git push origin master
    To git@github.com:Webdevkin/site-git.git
     ! [rejected]        master -> master (fetch first)
    error: failed to push some refs to 'git@github.com:Webdevkin/site-git.git'
    hint: Updates were rejected because the remote contains work that you do
    hint: not have locally. This is usually caused by another repository pushing
    hint: to the same ref. You may want to first integrate the remote changes
    hint: (e.g., 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.


**Git** устроен так, что локально мы можем коммитить сколько угодно. Но прежде чем отправить свои коммиты на сервер, то есть запушить, нужно подтянуть новые коммиты с сервера. Те самые, которые успели сделать наши коллеги. То есть сделать ***git pull***.

Когда мы делаем ***git push***, **git** сначала проверяет, а нет ли на сервере новых коммитов. Если они есть, то **git** выдает то самое сообщение - ***git push rejected***. Значит, нам нужно сначала сделать **git pull,** а затем снова запушить


    $ git pull origin master
    remote: Enumerating objects: 4, done.
    remote: Counting objects: 100% (4/4), done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
    Unpacking objects: 100% (3/3), done.
    From github.com:Webdevkin/site-git
     * branch            master     -> FETCH_HEAD
       239892a..383b385  master     -> origin/master
    Merge made by the 'recursive' strategy.
     README.md | 3 +++
     1 file changed, 3 insertions(+)
     create mode 100644 README.md

Здесь нас подстерегает неожиданность - в терминале выскакивает окно редактирования коммита. Пока просто сохраним, что предложено по умолчанию и закроем редактор. Вот теперь можно пушить свои коммиты


    $ git push origin master 
    Counting objects: 19, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (17/17), done.
    Writing objects: 100% (19/19), 1.98 KiB | 0 bytes/s, done.
    Total 19 (delta 4), reused 0 (delta 0)
    remote: Resolving deltas: 100% (4/4), completed with 1 local object.
    To git@github.com:Webdevkin/site-git.git
       383b385..f32b91e  master -> master

Все, наши коммиты на сервере. При этом появится странный коммит *"Merge branch 'master' of github.com:Webdevkin/site-git"*. Это так называемый мердж-коммит, о нем чуть ниже.

Если же при попытке пуша новых коммитов на сервере нет, то ***git push*** пройдет сразу и отправит наши коммиты на сервер.

Как избавиться от мердж-коммита
Мердж-коммит появляется, когда вы сделали локальный коммит, а после этого подтянули новые коммиты с сервера. Мердж-коммит не несет смысловой информации, кроме самого факта мерджа. Без него история коммитов выглядит чище.

Чтобы избавиться от него, подтягивайте изменения командой ***git pull*** с флажком *--rebase*


    $ git pull --rebase origin master 

При этом ваш локальный коммит окажется "поверх" нового коммита с сервера, а мердж-коммита не будет. 
