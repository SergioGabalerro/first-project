# Основные команды по работе с Git

## 1. Базовые команды

Узнать, где вы сейчас, поможет команда _pwd_ <br>
```bash 
$ pwd /c/Users/%USERNAME% 
```

Перейти в нужную директорию команда _cd_ а вот знак _~_ <br>
``` bash
$ cd ~ 
```

Вывести содержимое директории — _ls_ а флаг _-a_ говорит про все файлы и папки, даже скрытые <br> 
```bash 
$ ls # вывели список файлов file.txt photo.png ls -a # вывели список, в котором отображаются скрытые файлы ., .. и .git . .. .git file.txt photo.png 
```


Создание файлов и директорий _touch_ и _mkdir_, копирование файлов _cp_ <br> 
```bash
$ touch my-new-file.txt # создали файл my-new-file.txt  
$ mkdir new-dir # создали директорию new-dir
$ cp что_копируем куда_копируем
$ cp index.html src/ #скопировали index.html в папку src
```

Перемещение файлов и папок _mv_ <br>
```bash 
$ mv table.csv ./very-important-files # сначала указываем имя файла, который хотим переместить, потом путь — куда перемещаем 

$ cd very-important-files
$ ls
table.csv # перешли в папку very-important-files и проверили, что всё сработало
```

Удаление файлов и папок - rm, rmdir, rm -r
```bash
$ rm -r images # удалили папку images со всем её содержимым из текущей директории
```


Вы можете выделять текст в markdown с помощью символов `_` или `*`. Например:

Пример _курсива_ и **жирного** текста.

## 2. Работа с репозитотием
### 2.1 Инициализируем репозиторий
```bash
$ cd ~/dev/first-project # перешли в нужную папку
$ git init # создали репозиторий
```
«Разгитить» папку, если что-то пошло не так
```bash 
$ cd <папка с репозиторием> # перешли в папку
$ rm -rf .git # удалили подпапку .git
```

### 2.2 Проверить состояние репозитория и username
```bash
$ git status
```

### 2.3 Добавляем файлы в репозиторий
```bash
$ touch todo.txt
$ touch readme.txt # создали файлы todo.txt и readme.txt
$ git status # проверили статус
$ git add todo.txt
$ git add readme.txt
$ git status
$ git add --all
```
### 2.4 Выполнить коммит git commit
```bash
$ git commit -m ‘Мой первый коммит!’
$ git log #выводит коммиты в обратном хронологическом порядке
```

### 2.5 Соединяем удаленный и локальный репозиторий
```bash
$ cd ~ # перешли в домашнюю директорию
$ ls -la .ssh/ # вывели список созданных ключей
$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
$ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"
> Enter a file in which to save the key (C:\Users\<имя_пользователя>\.ssh\):[Press enter] #Укажите место хранения ключей. Простой вариант — сделать домашний каталог пользователя путём по умолчанию.
$ ls -a ~/.ssh
```
Теперь привяжем SSH-ключ к GitHub
```bash
# скопировать содержимое ключа в буфер обмена:
$ clip < ~/.ssh/id_rsa.pub
# для ed25519:
$ clip < ~/.ssh/id_ed25519.pub
```

Проверьте правильность ключа с помощью следующей команды.
```bash
$ ssh -T git@github.com
```
Привязать удалённый репозиторий к локальному — 'git remote add'
```bash
$ cd ~/dev/first-project
$ git remote add origin git@github.com:%ИМЯ_АККАУНТА%/first-project.git
$ git remote -v
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (fetch)
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (push)
```
### 2.6 Отправить изменения на удалённый репозиторий 
---
```bash
$ git push -u origin main # Если команда приведёт к ошибке, попробуйте 
                          # заменить main на master.
```
_В первый раз эту команду нужно вызвать с флагом -u и параметрами origin (имя удалённого репозитория) и main или master (название текущей ветки). Флаг -u свяжет локальную ветку с одноимённой удалённой. Как вы связывали локальный и удалённый репозитории в предыдущем уроке, так же и здесь нужно дополнительно связать ветки._


## Случай поломки файла на локальном репозитории
В результате работы через редактор `vi` файл README.md сломался и толком не восставился, но его удаленная копия осталась удаленном репозитории
 с помощью некоторых комманд сделали update файлов:
 1. сначала удалили swp file
 ```` bash
 $rm -rf .README.md.swp
 ````
 2. Создали новый файл README.md
 3. сделали обмен файлом из удаленого репозитория
````bash
git fetch
git checkout origin/master ~/filepath/README.md
````

## Навигация по коммитам
### Хеш — идентификатор коммита
Git хранит таблицу соответствий `хеш → информация о коммите`. Если вы знаете хеш, вы можете узнать всё остальное: 
автора и дату коммита и содержимое закоммиченных файлов. Можно сказать, что хеш — основной идентификатор коммита.

Все хеши, а также таблицу соответствий хеш → информация о коммите Git хранит в папке `.git`.

### Logs / логи
Элементы, из которых состоит лог:
* строка из цифр и латинских букв после слова commit — это хеш коммита;
* Author — имя автора и его электронная почта;
* Date — дата и время создания коммита;
* в конце находится сообщение коммита.

Получить сокращённый лог можно с помощью команды  `git log` с флагом `--oneline` (англ. «одной строкой»). 

### HEAD — всему голова
При вызове команды git log появляется надпись `(HEAD -> master)` 

Файл HEAD (англ. «голова», «головной») — один из служебных файлов папки .git. Он указывает на коммит, который сделан последним (то есть на самый новый).
В этом можно убедиться с помощью терминала. Перейдите в папку .git командой cd. Посмотрите содержимое файла HEAD командой cat.

````bash
$ pwd # посмотрели, где мы
/Users/user/dev/first-project

$ cd .git/
$ ls # посмотрели, какие есть файлы
COMMIT_EDITMSG  ORIG_HEAD  description  index  logs/     refs/
HEAD            config     hooks/       info/  objects/

$ cat HEAD # команда cat показывает содержимое файла
ref: refs/heads/master # в файле вот такая ссылка
````

Внутри HEAD — ссылка на служебный файл: refs/heads/master (или refs/heads/main в зависимости от названия ветки). Если заглянуть в этот файл, можно увидеть хеш последнего коммита.

Когда вы делаете коммит, Git обновляет refs/heads/master — записывает в него хеш последнего коммита. Получается, что HEAD тоже обновляется, так как ссылается на refs/heads/master.


### Как читать git status
Частая ошибка при использовании Git — закоммитить лишнее или, наоборот, забыть добавить важный файл в коммит. 
Этого легко избежать, если не забывать проверять статусы файлов с помощью команды `git status`.

В итоге `git status` показывает только следующие состояния файлов:
* staged (Changes to be committed в выводе git status);
* modified (Changes not staged for commit);
* untracked (Untracked files).

Типичные варианты вывода git status:
* Нет ни `staged-`, ни `modified-`, ни `untracked-файлов`
* Найдены неотслеживаемые файлы
* Найдены изменения, которые не войдут в коммит
* Файл добавлен в staging area, но после этого изменён
````bash
# изменили fileA.txt
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
          modified:   fileA.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
          modified:   fileA.txt
````


  "A: Пишем код" --> "B: Делаем add" --> C: Проверяем статус --> D: Делаем коммит --> Final: Делаем push для синхронизации

```mermaid
graph LR;
  %%"Пишем код"    -->    "Делаем add";%%
  Пишем код    -- "???"     --> tracked/comitted;
```





























