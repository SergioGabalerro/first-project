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

Все хеши, а также таблицу соответствий хеш → информация о коммите Git хранит в папке .git.



