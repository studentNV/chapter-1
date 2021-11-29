# Lesson 1

## Home work 1

#### 0) Установить вторую ВМ, настроить на ней только внутренний сетевой интерфейс и подключиться с первой машины. Все команды выполняются от имени, созданного во время инсталляции пользователя (не root).

![image](https://user-images.githubusercontent.com/95025513/143766550-f5acc128-12ae-4d20-a1e5-bd17747d72fb.png)

> Подключение по ssh с первого хоста на второй.
```bash
[sitis$localhost ~]$ ssh sitis2@10.107.10.3 
```
![image](https://user-images.githubusercontent.com/95025513/143766531-ea796435-bf52-4910-832f-9d7ae1ba6aa3.png)

#### 1) Внутри директории /usr/share/man (хранилище встроенной документации) находятся каталоги, разбитые по секциям разделов помощи (man1, man2, man3) и по языкам (es, fr, ru).
#### Используя команду ls, необходимо вывести на экран все файлы, которые расположены в секционных директориях /usr/share/man/manX и содержат слово "config" в имени.
```bash
[sitis$localhost man]$ ls /usr/share/man/man* | grep config 
```
![image](https://user-images.githubusercontent.com/95025513/143767937-c319a6bc-992a-4f7a-b7b0-7d298938bd2e.png)

#### Одним вызовом ls найти все файлы, содержащие слово "system" в каталогах /usr/share/man/man1 и /usr/share/man/man7
```bash
[sitis$localhost man]$ ls /usr/share/man/man[1,7] | grep system 
```
![image](https://user-images.githubusercontent.com/95025513/143767967-e3ff237e-ad3a-478a-b6c1-b50e75150aa7.png)

#### 2) Самостоятельно изучить команду find, предназначенную для поиска файлов/папок по заданным условиям (man find, find --help).
#### Найти в директории /usr/share/man все файлы, которые содержат слово "help" в имени, найти там же все файлы, имя которых начинается на "conf".
```bash
[sitis$localhost man]$ find /usr/share/man -name "*help*"
```
![image](https://user-images.githubusercontent.com/95025513/143767983-0d66ca72-520d-41c1-9776-ccc26a5e0ebd.png)

#### Какие действия мы можем выполнить с файлами, найденными командой find (не запуская других команд)? Приведите любой пример с комментарием.
```bash
[sitis$localhost man]$ find . -empty –delete	//удаление всех пустых файлов в директории, которой вы сейчас находитесь
```
#### 3) При помощи команд head и tail, выведите последние 2 строки файла /etc/fstab и первые 7 строк файла /etc/yum.conf
```bash
[sitis$localhost man]$ head -n 7 /etc/fstab
[sitis$localhost man]$ tail -n 2 /etc/fstab
```
![image](https://user-images.githubusercontent.com/95025513/143766684-8ae1e79f-9e87-4661-8721-b25db449bb65.png)
#### Что произойдёт, если мы запросим больше строк, чем есть в файле? Попробуйте выполнить это на примере, используя команду wc (word cound) для подсчёта количества строк в файле.
```bash
[sitis$localhost man]$ wc -l /etc/fstab
[sitis$localhost man]$ tail -n 20 /etc/fstab
```
![image](https://user-images.githubusercontent.com/95025513/143767030-c5802ea3-c724-4f92-ab41-6017e7bfa4c6.png)

> С помощью команды wc –l мы узнали, что файл содержит 11 строк. При попытке задать большее кол-во строк командами tail и head, команды выводят на экран весь файл.

#### 4) Создайте в домашней директории файлы file_name1.md, file_name2.md и file_name3.md. 
```bash
[sitis$localhost ~]$ mkdir file_name{1..3}.md
```
![image](https://user-images.githubusercontent.com/95025513/143768015-d47582e7-2e37-486e-97e7-741c538734b2.png)

#### Используя {}, переименуйте: mv filename{.md,}

#### file_name1.md в file_name1.textdoc

#### file_name2.md в file_name2
```bash
[sitis$localhost home2]$ mv file_name1{.md,textdoc}
[sitis$localhost home2]$ mv file_name2{.md,}
```
![image](https://user-images.githubusercontent.com/95025513/143768029-f5a123e3-f318-4a57-9d9e-036d5db85b22.png)

#### file_name3.md в file_name3.md.latest
```bash
[sitis$localhost home2]$ mv file_name3{.md,.md.latest}
```
![image](https://user-images.githubusercontent.com/95025513/143768066-db3d0093-6bf6-41ec-bc1e-77cc959645cc.png)
#### file_name1.textdoc в file_name1.txt
```bash
[sitis$localhost home2]$ mv file_name1{.textdoc,.txt}
```
![image](https://user-images.githubusercontent.com/95025513/143768102-8f03cd79-bb3c-46d5-8dc7-3aaecbbea0b4.png)

#### 5) Перейдите в директорию /mnt. Напишите, как можно больше различных вариантов команды cd, с помощью которых вы можете вернуться обратно в домашнюю директорию вашего пользователя. Различные относительные пути также считаются разными вариантами.
```bash
-----------------1--------------------
[sitis$localhost mnt]$ cd ~
-----------------2--------------------
[sitis$localhost mnt]$ cd /home/siti2/
-----------------3--------------------
[sitis$localhost mnt]$ cd ..
[sitis$localhost /]$ cd /home
[sitis$localhost home]$ cd /siti2
--------------------------------------
```
![image](https://user-images.githubusercontent.com/95025513/143767095-1e3c59ce-3dcd-462d-b884-f45706f34741.png)
```bash
-----------------4--------------------
[sitis$localhost mnt]$ cd -
-----------------5--------------------
[sitis$localhost mnt]$ cd ~siti2
--------------------------------------
```
![image](https://user-images.githubusercontent.com/95025513/143767121-c4d6b099-00c9-4bd0-a587-ad5045903549.png)

#### 6) Создайте одной командой в домашней директории 3 папки new, in-process, processed. При этом in-process должна содержать в себе еще 3 папки tread0, tread1, tread2.
```bash
[sitis$localhost ~]$ mkdir -p new in-process/tread{0..2} processed 
```
![image](https://user-images.githubusercontent.com/95025513/143767250-bba922a8-63eb-4f8b-8662-695d4113a9e7.png)

#### Далее создайте 100 файлов формата data[[:digit:]][[:digit:]] в папке new
```bash
[sitis$localhost ~]$ touch data{00..99}
```
![image](https://user-images.githubusercontent.com/95025513/143767295-32cf5e05-c024-4573-bfbb-c7b395b6f48d.png)

#### Скопируйте 34 файла в tread0 и по 33 в tread1 и tread2 соответственно. Выведете содержимое каталога in-process одной командой
```bash
[sitis$localhost new]$ cp data{00..99} ~/in-process/tread0
[sitis$localhost new]$ cp data{34..66} ~/in-process/tread1
[sitis$localhost new]$ cp data{67..99} ~/in-process/tread2
```
![image](https://user-images.githubusercontent.com/95025513/143767452-76439b8c-5406-4ae4-b7d6-d9034c7fc98c.png)
```bash
[sitis$localhost new]$ ls -la ~/in-process/
```
![image](https://user-images.githubusercontent.com/95025513/143767518-c19b3aa0-7ede-47fe-b450-689f6f2c517f.png)

#### После этого переместите все файлы из каталогов tread в processed одной командой. Выведете содержимое каталога in-process и processed опять же одной командой
```bash
[sitis$localhost new]$ mv ~/in-process/tread0/* ~/in-process/tread1/* ~/in-process/tread2/* ~/in-processed/
[sitis$localhost new]$ ls ~/in-process ~/in-processed/
```
![image](https://user-images.githubusercontent.com/95025513/143767585-792979c8-0748-49a9-b047-81223dddd535.png)

#### Сравните количество файлов в каталогах new и processed при помощи изученных ранее команд, если они равны удалите файлы из new
```bash
[sitis$localhost new]$ find ~/new -type f | wc -l
[sitis$localhost new]$ find ~/processed -type f | wc -l
```
![image](https://user-images.githubusercontent.com/95025513/143767648-a996f27a-383a-4592-94a9-ae4e5f70bf1e.png)

#### ** Сравнение количества и удаление сделано при помощи условия
```bash
[sitis$localhost new]$ ./script
```
![image](https://user-images.githubusercontent.com/95025513/143767714-c724d3c7-39d5-4f34-8f87-1b9d7b0e3650.png)

![image](https://user-images.githubusercontent.com/95025513/143767730-f54f079c-f2cc-4c3d-ae4d-a9906a1132de.png)

```bash
#!/bin/bash
a=`find ~/new -type f |wc -l`
b=`find ~/processed/ -type f |wc -l`
echo "Count files in directory new equals $a"
echo "Count files in directory processed equals $b"
if [ $a -eq $b ]; then
        printf "Are you sure want to delete all files from the directory\n1 = yes\n2 = no\n"
        read userEnter
        case $userEnter in
                1)
                        rm ~/new/*
                        echo "All files from the ~/new have beed deleted!";;
                2)
                        echo "You refused to delete files!";;
                *)
                        echo "You enteres incorrect values!";;
        esac
else
        echo "The number of files does not match"
fi
```
#### 7)* Получить разворачивание фигурных скобок для выражения. Согласно стандартному поведению bash, стандартного для CentOS 7, скобки в приведённом ниже выражении развёрнуты не будут. Необходимо найти способ получить ожидаемый вывод.
#### a=1; b=3
#### echo file{$a..$b}
#### Необходимо предоставить модифицированную команду, результатом которой является следующий вывод: 
#### file1 file2 file3
```bash
[sitis$localhost new]$ eval echo file{$a..$b}
```
https://www.unix.com/man-page/posix/1posix/eval/

![image](https://user-images.githubusercontent.com/95025513/143767878-538fa5eb-2e45-41ea-9807-2e4f1eb58c2e.png)

новое изменение.
