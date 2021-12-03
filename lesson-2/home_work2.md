# Lesson 2

## Home work 2

#### 1. Открыть инструкцию по пользованию приложением awk. Найти секцию про использование переменных окружения. Сохранить эту секцию в отдельный файл.

> Для удобства копирования, используем лучший редактор `vi` с помощью следующий команды открываем man с помощью `vi`
```bash
[sitis@localhost ~]$ man awk | vi -
```
> Далее нажимаем `v` (переход в режим выделения) выделяем необходимый текст 
> Нажимаем `Shift + :` вписываем `w newFileName` (тем самым копируем выделенный текст в файл с названием `newFileName`)
> Выходим из `vi` и проверяем результат с помощью команды `cat`
```bash
[sitis@localhost ~]$ cat  newFileName
ENVIRONMENT VARIABLES
       The  AWKPATH  environment  variable  can be used to provide a list of directories that gawk searches
       when looking for files named via the -f and --file options.

       For socket communication, two special environment variables can be used to  control  the  number  of
       retries (GAWK_SOCK_RETRIES), and the interval between retries (GAWK_MSEC_SLEEP).  The interval is in
       milliseconds. On systems that do not support usleep(3), the value is rounded up to an integral  num‐
       ber of seconds.

       If POSIXLY_CORRECT exists in the environment, then gawk behaves exactly as if --posix had been spec‐
       ified on the command line.  If --lint has been specified, gawk issues  a  warning  message  to  this
       effect.
```

#### 2. Написать скрипт, который создаёт файл "task2.txt" директорией выше своего местоположения. В случае ошибки текст ошибки записывается в err.log а пользователю выдаётся сообщение "Error."

```bash
#!/bin/bash
touch ../task2.txt 2>err.log && echo "File is created successfully" || echo "Error."
```

#### 2*. Если файл уже существует, выдаётся одна ошибка, а если нет прав для его создания - другая.
> Решение не очень элегантное, но всё же.

```bash
#!/bin/bash
FILE="task2.txt"
if [ -e "../$FILE" ]; then
        echo "The file already exists"
        exit
fi
touch ../$FILE 2>/dev/null && echo "File is created successfully" || echo "Permission denied"
```

#### 3. Создать 2 файла: 1-й - текстовый с указанием абслютного пути до директории. 2-й - скрипт, который при выполнении выводит содержимое директории по указанному в первом файле.
> Без функции реализовать не смог одновременного вывода сообщения и выход из скрипта 
```bash
#!/bin/bash
function FshowErrorMessage {
echo "You entered a non-existent file"
exit
}

echo "Enter file name with absolute path "
read FILE
NAME=$(cat "$FILE" 2>/dev/null) || FshowErrorMessage
echo $NAME

ls -la $NAME
```

#### 3*. Скрипт выводит отдельно количество файлов и количество директорий.
```bash
#!/bin/bash

function FshowErrorMessage {
echo "You entered a non-existent file"
exit
}

echo "Enter file name with absolute path "
read FILE
NAME=$(cat "$FILE" 2>/dev/null) || FshowErrorMessage
echo "----Directory listing:----"
find $NAME -maxdepth 1 -type f
echo "----List of files----"
find $NAME -maxdepth 1 -type f
```


#### 3**. Скрипт принимает любое количество записей в первом файле и обрабатывает их последовательно.

```bash
#!/bin/bash
function FshowErrorMessage {
echo "You entered a non-existent file"
exit
}
echo "Enter file name with absolute path "
#read FILE
#NAME=$(cat "$FILE" 2>/dev/null) || FshowErrorMessage
NAME=fileWithDir
cat $NAME | while read TESA
do
echo "----Directory listing $TESA:----"
find $TESA -maxdepth 1 -type d
echo "----List of files $TESA----"
find $TESA -maxdepth 1 -type f
done

```

#### 4. По желанию. Самостоятельно изучить экранные мультиплексоры (screen или tmux), установить один из них себе и ознакомиться с функционалом. На любые возникшие вопросы ответят преподаватели курса.
