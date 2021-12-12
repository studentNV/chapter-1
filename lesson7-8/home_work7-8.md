# Lesson 4

## Home work 4

### Task 1: Users and groups
#### Используйте команды: groupadd, useradd, passwd, chage и другие. Создайте группу sales с GID 4000 и пользователей bob, alice, eve c основной группой sales. 
> создание группы с GID 4000. Просмотр информации о группе sales
```bash
[sitis@localhost ~]$ sudo groupadd -g 4000 sales
[sitis@localhost ~]$ getent group sales
sales:x:4000:
```
> Cоздание пользователя с основной группой sales. Проверка с какими параметрами создали пользователя.
```bash
[sitis@localhost ~]$ sudo useradd -g sales bob
[sitis@localhost ~]$ sudo useradd -g sales alice
[sitis@localhost ~]$ sudo useradd -g sales eve
[sitis@localhost ~]$ groups bob
bob : sales
[sitis@localhost ~]$ groups alice
alice : sales
[sitis@localhost ~]$ groups eve
eve : sales
```
> Измените пользователям пароли.
```bash
[sitis@localhost ~]$ sudo passwd bob
Changing password for user bob.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
[sitis@localhost ~]$ sudo passwd alice
Changing password for user alice.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
[sitis@localhost ~]$ sudo passwd eve
Changing password for user eve.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```
#### Все новые аккаунты должны обязательно менять свои пароли каждый 30 дней.
> Для новый пользователей редактироем файл `/etc/login.defs` меняем значение `PASS_MAX_DAYS` на 30
> Cоздал нового пользователя `testUser` для проверки
```bash
[sitis@localhost ~]$ sudo adduser testUser
[sudo] password for sitis:
[sitis@localhost ~]$ sudo chage -l testUser
Last password change                                    : Dec 09, 2021
Password expires                                        : Jan 08, 2022
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 30
Number of days of warning before password expires       : 7
```
#### Новые аккаунты группы sales должны истечь по окончанию 90 дней срока, а bob должен изменять его пароль каждые 15 дней.
```bash
[sitis@localhost ~]$ sudo chage -E 2022-02-09 alice
[sitis@localhost ~]$ sudo chage -E 2022-02-09 eve
[sitis@localhost ~]$ sudo chage -M 15 bob
[sitis@localhost ~]$ sudo chage -l alice
Last password change                                    : Dec 09, 2021
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Feb 09, 2022
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
[sitis@localhost ~]$ sudo chage -l eve
Last password change                                    : Dec 09, 2021
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Feb 09, 2022
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
[sitis@localhost ~]$ sudo chage -l bob
Last password change                                    : Dec 09, 2021
Password expires                                        : Dec 24, 2021
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 15
Number of days of warning before password expires       : 7
```
#### Дополнительно: Заставьте пользователей сменить пароль после первого логина.
```bash
[sitis@localhost ~]$ su - testUser
Password:
Last failed login: Thu Dec  9 01:38:41 EST 2021 on pts/0
There were 4 failed login attempts since the last successful login.
[testUser@localhost ~]$ exit
logout
[sitis@localhost ~]$ sudo passwd --expire testUser
Expiring password for user testUser.
passwd: Success
[sitis@localhost ~]$ su - testUser
Password:
You are required to change your password immediately (root enforced)
Changing password for testUser.
(current) UNIX password:
```
#### Предварительный шаг:
#### Исследуйте файл /etc/login.defs.
#### Исследуйте, как работает команда date и как её использовать совместно с chage.

### Task 2: Controlling access to files with Linux file system permissions

#### Используйте команды: su, mkdir, chown, chmod и другие.
#### Создайте трёх пользователей glen, antony, lesly.
> Создаем группу `students`. Присваевываю пользователям основную группу `students`.
```bash
[sitis@localhost ~]$ sudo groupadd students 
[sitis@localhost ~]$ sudo useradd -g students glen
[sitis@localhost ~]$ sudo useradd -g students antony
[sitis@localhost ~]$ sudo useradd -g students lesly
```
#### У вас должна быть директория /home/students, где эти три пользователя могут работать совместно с файлами.
#### Должен быть возможен только пользовательский и групповой доступ, создание и удаление файлов в /home/students.
#### Файлы, созданные в этой директории, должны автоматически присваиваться группе студентов students. 
> делаем владельцем каталога группу `students`
```bash
[sitis@localhost home]$ sudo chown -R :students /home/students/
[sitis@localhost home]$ ll | grep students
drwxr-xr-x. 2 root     students   6 Dec  9 01:49 students
```
> добавляем пользователям из группы `students` все необходимы права и забираем их у `others`
```bash
[sitis@localhost home]$ sudo chmod g+w,o-rx -R students/
[sitis@localhost home]$ ll | grep students
drwx------. 4 antony   students  91 Dec  9 02:13 antony
drwx------. 4 glen     students 107 Dec  9 02:14 glen
drwx------. 2 lesly    students  62 Dec  9 02:11 lesly
drwxrwx---. 2 root     students   6 Dec  9 01:49 students
```
> Перед тем как создавать какие либо файлы пользователям в папке `students` необходимо отредактировать файл `vi ~/.bashrc` у каждого пользователя или глобально и установить `umask 002`
> При создании файла или католога разными пользователями мы увидим следубщие разрешения на группу
```bash
[glen@localhost students]$ ll
total 0
-rw-rw-r--. 1 glen students 0 Dec  9 01:15 testFile
```
#### Предварительный шаг:
#### Исследуйте, для чего нужны файлы .bashrc и .profile.

### Task3: ACL

#### Детективное агентство Бейкер Стрит создает коллекцию совместного доступа для хранения файлов дел, в которых члены группы bakerstreet будут иметь права на чтение и запись.
#### Ведущий детектив, Шерлок Холмс, решил, что члены группы scotlandyard также должны иметь возможность читать и писать в общую директорию. Тем не менее, Холмс считает, что инспектор Джонс является достаточно растерянным, и поэтому он должен иметь доступ только для чтения. 
#### Миссис Хадсон только начала осваивать Linux и смогла создать общую директорию и скопировать туда несколько файлов. Но сейчас время чаепития, и она попросила вас закончить работу.
#### Ваша задача - завершить настройку директории общего доступа. 
#### Директория и всё её содержимое должно принадлежать группе bakerstreet, при этом файлы должны обновляться для чтения и записи для владельца и группы (bakerstreet). У других пользователей не должно быть никаких разрешений.
#### Вам также необходимо предоставить доступы на чтение и запись для группы scotlandyard, за исключением Jones, который может только читать документы.
#### Убедитесь, что ваша настройка применима к существующим и будущим файлам. После установки всех разрешений в директории проверьте от каждого пользователя все его возможные доступы.
#### Используйте команды: touch, mkdir, chgrp, chmod, getfacl, setfacl и другие. 
#### Создайте общую директорию /share/cases.
#### Создайте группу bakerstreet с пользователями holmes, watson.
#### Создайте группу scotlandyard с пользователями lestrade, gregson, jones.
#### Задайте всем пользователям безопасные пароли
#### Предварительный шаг:
#### От суперпользователя создайте папку /share/cases и создайте внутри 2 файла murders.txt и moriarty.txt.
> создание папки и 2 файлов
```bash
[root@localhost ~]# mkdir -p /share/cases
[root@localhost ~]# > /share/cases/murders.txt | >/share/cases/moriarty.txt
[root@localhost ~]# ll /share/cases
total 0
-rw-r--r--. 1 root root 0 Dec  9 00:56 moriarty.txt
-rw-r--r--. 1 root root 0 Dec  9 00:56 murders.txt
```
> Создание пользователей `holmes`, `watson` в группе `bakerstreet`
```bash
[sitis@localhost ~]$ sudo groupadd bakerstreet
[sitis@localhost ~]# sudo useradd -g bakerstreet holmes
[sitis@localhost ~]# sudo useradd -g bakerstreet watson
[sitis@localhost ~]$ groups holmes
holmes : bakerstreet
[sitis@localhost ~]$ groups watson
watson : bakerstreet
```
> Создание пользователей `lestrade`, `gregson`, `jones` в группе `scotlandyard`
```bash
[sitis@localhost ~]$ sudo groupadd scotlandyard
[sitis@localhost ~]# sudo useradd -g scotlandyard lestrade
[sitis@localhost ~]# sudo useradd -g scotlandyard gregson
[sitis@localhost ~]# sudo useradd -g scotlandyard jones
[sitis@localhost ~]$ groups lestrade
lestrade : scotlandyard
[sitis@localhost ~]$ groups gregson
gregson : scotlandyard
[sitis@localhost ~]$ groups jones
jones : scotlandyard
```
- Задание паролей пользователяи на примере `holmes`
```bash
[sitis@localhost ~]$ sudo passwd holmes
```
> смотрим какие сейчас права на директории
```bash
[sitis@localhost ~]$ getfacl /share/cases
getfacl: Removing leading '/' from absolute path names
# file: share/cases
# owner: root
# group: root
user::rwx
group::r-x
other::r-x
```
> Меняем владельца группы и предоставляем все права группе `bakerstreet` забирая у `others`
```bash
[sitis@localhost ~]$ sudo chown -R :bakerstreet /share/cases
[sitis@localhost ~]$ sudo chmod 770 -R /share/cases
```
> Устанавливаем липкий бит к папке рекурсивно для текущий файлов и для последующих группе `bakerstreet`
```bash
sudo setfacl -R -m g:bakerstreet:rwx /share/cases
sudo setfacl -R -m d:g:bakerstreet:rwx /share/cases
```
> Cмотрим какие сейчас права на директории
```bash
[sitis@localhost ~]$ getfacl /share/cases
getfacl: Removing leading '/' from absolute path names
# file: share/cases
# owner: root
# group: bakerstreet
user::rwx
group::rwx
group:bakerstreet:rwx
mask::rwx
other::---
default:user::rwx
default:group::rwx
default:group:bakerstreet:rwx
default:mask::rwx
default:other::---
```
> Предоставим доступ на чтение и запись группе `scotlandyard`
```bash
[sitis@localhost ~]$ sudo setfacl -R -m g:scotlandyard:rwx /share/cases
[sitis@localhost ~]$ sudo setfacl -R -m d:g:scotlandyard:rwx /share/cases
[sitis@localhost ~]$ getfacl /share/cases
getfacl: Removing leading '/' from absolute path names
# file: share/cases
# owner: root
# group: bakerstreet
user::rwx
group::rwx
group:bakerstreet:rwx
group:scotlandyard:rwx
mask::rwx
other::---
default:user::rwx
default:group::rwx
default:group:bakerstreet:rwx
default:group:scotlandyard:rwx
default:mask::rwx
default:other::---
```
> Задаем права пользователю `jones` только на чтение
```bash
sudo setfacl -R -m u:jones:rx /share/cases
sudo setfacl -R -m d:u:jones:rx /share/cases
[sitis@localhost ~]$ getfacl /share/cases
getfacl: Removing leading '/' from absolute path names
# file: share/cases
# owner: root
# group: bakerstreet
user::rwx
user:jones:r-x
group::rwx
group:bakerstreet:rwx
group:scotlandyard:rwx
mask::rwx
other::---
default:user::rwx
default:user:jones:r-x
default:group::rwx
default:group:bakerstreet:rwx
default:group:scotlandyard:rwx
default:mask::rwx
default:other::---
```
> Проверяем что получилось сначало пользователь `holmes` создадим файл и запишем в него информацию 
```bash
[holmes@localhost cases]$ echo "Text holmes" > testFileHolmesBakerstreet
```
> далее пользователь из группы `scotlandyard` а именно `lestrade`
```bash
[lestrade@localhost cases]$ echo "Text lestrade" > testFileLestradeScotlandyard
```
> Так же проверим могут ли они читать из тех файлов и записывать в них которые создали пользователи из разных групп
```bash
[lestrade@localhost cases]$ cat testFileHolmesBakerstreet
Text holmes
[lestrade@localhost cases]$ echo "Add some text lestrade in file holmes" >> testFileHolmesBakerstreet
[lestrade@localhost cases]$ cat testFileHolmesBakerstreet
Text holmes
Add some text lestrade in file holmes
[lestrade@localhost cases]$ echo "Add some text lestrade in file moriarty.txt" >> moriarty.txt
[lestrade@localhost cases]$ cat moriarty.txt
Add some text lestrade in file moriarty.txt
```
```bash
[holmes@localhost cases]$ cat testFileLestradeScotlandyard
Text lestrade
[holmes@localhost cases]$ echo "Add some text holmes in file lestrade" >> testFileLestradeScotlandyard
[holmes@localhost cases]$ cat testFileLestradeScotlandyard
Text lestrade
Add some text holmes in file lestrade
[lestrade@localhost cases]$ echo "Add some text holmes in file moriarty.txt" >> moriarty.txt
[lestrade@localhost cases]$ cat moriarty.txt
Add some text lestrade in file moriarty.txt
Add some text holmes in file moriarty.txt
[lestrade@localhost cases]$
```
> Oшибок нет со старыми и новыми файлами. Теперь проверим права для `jones`
```bash
[jones@localhost cases]$ cat testFileLestradeScotlandyard
Text lestrade
Add some text holmes in file lestrade
[jones@localhost cases]$ cat testFileHolmesBakerstreet
Text holmes
Add some text lestrade in file holmes
[jones@localhost cases]$ cat moriarty.txt
Add some text lestrade in file moriarty.txt
Add some text holmes in file moriarty.txt
```
> Файлы прочитал без проблем пропробуем добавить к ним текст
```bash
[jones@localhost cases]$ echo "Add some text jones in file testFileLestradeScotlandyard" >> testFileLestradeScotlandyard
bash: testFileLestradeScotlandyard: Permission denied
[jones@localhost cases]$ echo "Add some text jones in file testFileHolmesBakerstreet" >> testFileHolmesBakerstreet
bash: testFileHolmesBakerstreet: Permission denied
[jones@localhost cases]$ echo "Add some text jones in file moriarty.txt" >> moriarty.txt
bash: moriarty.txt: Permission denied
```
> Во всех трёх случаях вывелась ошибка. Как и требовалось.
