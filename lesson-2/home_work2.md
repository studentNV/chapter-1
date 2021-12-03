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
> Скрипт
```bash
#!/bin/bash
touch ../task2.txt 2>err.log && echo "File is created successfully" || echo "Error."
```
> Пример работы скрипта при успешном создании файла
```bash
[sitis@localhost scr]$ ./script
File is created successfully
[sitis@localhost scr]$ ll ..
total 0
drwxrwxr-x. 2 sitis sitis 35 Dec  2 22:07 scr
-rw-rw-r--. 1 sitis sitis  0 Dec  2 22:07 task2.txt
[sitis@localhost scr]$ ll
total 4
-rw-rw-r--. 1 sitis sitis   0 Dec  2 22:11 err.log
-rwxrwxr-x. 1 sitis sitis 205 Dec  2 22:12 script
```
> Пример работы скрипта при ошибке
```bash
[sitis@localhost ~]$ ./script
Error.
[sitis@localhost ~]$ ll ..
total 0
drwx------. 4 sitis sitis 180 Dec  2 22:09 sitis
drwxr-xr-x. 2 root  root    6 Dec  2 21:16 tmp
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
> Пример работы
```bash
[sitis@localhost scr]$ ./script
File is created successfully
```
> Пример работы скрипта при ошибке
```bash
[sitis@localhost scr]$ ./script
The file already exists
```
> Пример работы
```bash
[sitis@localhost ~]$ ./script
Permission denied
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
ls -la $NAME
```
> Содержимаое файла с путем `fileForDir`
```bash
/usr/share/man
```
> Пример работы
```bash
[sitis@localhost scr]$ ./script
Enter file name with absolute path
fileForDir
total 88
drwxr-xr-x. 13 root root   155 Nov 25 19:05 .
dr-xr-xr-x. 17 root root   224 Nov 25 19:08 ..
dr-xr-xr-x.  2 root root 20480 Dec  1 20:51 bin
drwxr-xr-x.  2 root root     6 Apr 11  2018 etc
drwxr-xr-x.  2 root root     6 Apr 11  2018 games
drwxr-xr-x.  3 root root    23 Nov 25 19:05 include
dr-xr-xr-x. 27 root root  4096 Nov 25 19:06 lib
dr-xr-xr-x. 38 root root 20480 Dec  1 20:51 lib64
drwxr-xr-x. 21 root root  4096 Nov 25 19:06 libexec
drwxr-xr-x. 12 root root   131 Nov 25 19:05 local
dr-xr-xr-x.  2 root root 12288 Nov 25 19:06 sbin
drwxr-xr-x. 76 root root  4096 Dec  1 20:51 share
drwxr-xr-x.  4 root root    34 Nov 25 19:05 src
lrwxrwxrwx.  1 root root    10 Nov 25 19:05 tmp -> ../var/tmp
```
#### 3*. Скрипт выводит отдельно количество файлов и количество директорий.
> Скрипт
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
find $NAME -maxdepth 1 -type d
echo
echo "----List of files:----"
find $NAME -maxdepth 1 -type f
```
> Содержимаое файла с путем `fileForDir`
```bash
/home/sitis
```
> Пример работы
```bash
[sitis@localhost scr]$ ./script
Enter file name with absolute path
fileForDir
----Directory listing:----
/home/sitis
/home/sitis/.ssh
/home/sitis/scr

----List of files----
/home/sitis/.bash_logout
/home/sitis/.bash_profile
/home/sitis/.bashrc
/home/sitis/.bash_history
/home/sitis/.swp
/home/sitis/script
/home/sitis/err.log
/home/sitis/task2.txt
/home/sitis/.viminfo
```
#### 3**. Скрипт принимает любое количество записей в первом файле и обрабатывает их последовательно.
> Скрипт
```bash
#!/bin/bash
function FshowErrorMessage {
echo "You entered a non-existent file"
exit
}
echo "Enter file name with absolute path "
read NAME
cat $NAME 2>/dev/null | while read TESA || FshowErrorMessage
do
echo "----Directory listing $TESA:----"
find $TESA -maxdepth 1 -type d
echo "----List of files $TESA:----"
find $TESA -maxdepth 1 -type f
done
```
> Содержимаое файла с путем `fileForDir`
```bash
/home/sitis
/home/sitis/scr
```
> Пример работы
```bash
[sitis@localhost scr]$ ./script
Enter file name with absolute path
fileForDir

----Directory listing /home/sitis:----
/home/sitis
/home/sitis/.ssh
/home/sitis/scr
----List of files /home/sitis----
/home/sitis/.bash_logout
/home/sitis/.bash_profile
/home/sitis/.bashrc
/home/sitis/.bash_history
/home/sitis/.swp
/home/sitis/script
/home/sitis/err.log
/home/sitis/task2.txt
/home/sitis/.viminfo


----Directory listing /home/sitis/scr:----
/home/sitis/scr
----List of files /home/sitis/scr----
/home/sitis/scr/err.log
/home/sitis/scr/fileForDir
/home/sitis/scr/script
```

#### 4. По желанию. Самостоятельно изучить экранные мультиплексоры (screen или tmux), установить один из них себе и ознакомиться с функционалом. На любые возникшие вопросы ответят преподаватели курса.
> Установил поигрался `tmux` понравился больше
