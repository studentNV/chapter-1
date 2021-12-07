# Lesson 3

## Home work 5-6

### Awk /ɔːk/

#### 1. What is the most frequent browser?

> Самый простой и действенный способ это посмотреть сколько вхождений каждого браузера. Я пытался распирсить файл на столбцы, менял разделители, но удовлетворительного результата я не достиг. Учитывая что иногда в одной строке есть несколько браузеров я даже не представляю как это распарсить.

```bash
awk -F "Acoo" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Amigo" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Safari" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Arora" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Avant" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "BlackHawk" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Browzar" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Chromium" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "CoolNovo" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Coowon" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Dragon" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "IceDragon" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Dooble" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "DustyNet" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Privacy" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Chrome" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Goona" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "GreenBrowser" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Surfboard" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "K-Meleon" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Kylo" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Lunascape" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Maxthon" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Explorer" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Midori" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "CometBird" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Mozilla" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Flock" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "SeaMonkey" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Navigator" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "NetSurf" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Nuke" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Opera" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Orbitum" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Orca" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Moon" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "PirateBrowser" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "PlayFree" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "QIP" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "QtWeb" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "QupZilla" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "RockMelt" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Slepnir" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "SlimBrowser" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "SRWare" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Sundance" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Tor" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Torch" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Vivaldi" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Uran" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Yandex" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "Weblink" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
awk -F "AppleWebKit" 'NF>0 { count += NF-1 } END { print 0+count }' access.log
```
> Ответ Mozilla

> После некоторого времени гугления я нашел решения в интернете и модефицировав хочу его показать, но как по мне оно далеко от идеала (в плане все решения которые янашел выводят браузер с версией и считают бразуер с другой версией как другой браузер, но это же тот же браузер ...)
```bash
[sitis@localhost lesson3]$  awk -F [\"\(] '{print $6}' access.log | sort | uniq -c | sort -fr
```
> Пример работы
```bash
[sitis@localhost lesson3]$  awk -F [\"\(] '{print $6}' access.log | sort | uniq -c | sort -f
  17036 Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36
  42615 Mozilla/5.0 (Windows NT 6.1; Trident/7.0; rv:11.0) like Gecko
 340874 Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)
```
#### 2. Show number of requests per month for ip 216.244.66.230 (for example: Sep 2016 - 100500 reqs, Oct 2016 - 0 reqs, Nov 2016 - 2 reqs...)
> Не уверен что понял задание, написано в месяц то есть в один или в каждый. Может я не понял но вроде вот так. 
```bash
awk -F [/:] 'BEGIN {a=0} { if ($1~/216.244.66.230/ && $3~/2021/ &&  $2~/Dec/) a++} END{print $2 " " $3 " - " a " reqs"}' access.log
```
> Пример работы
```bash
[sitis@localhost lesson3]$ awk -F [/:] 'BEGIN {a=0} { if ($1~/216.244.66.230/ && $3~/2021/ &&  $2~/Dec/) a++} END{print $2 " " $3 " - " a " reqs"}' access.log
Dec 2021 - 2 reqs
```
> Если необходимо вывести все месяца
```bash
#!/usr/bin/awk -f
BEGIN {FS="/"}
{
if ($1~/216.244.66.230/ && $3~2020){
    !(($1) in d)
        {
            d[$2]
            c[$2]++
        }
    }
}
{
if ($1~/216.244.66.230/ && $3~2021){
    !(($1) in b)
        {
            b[$2]
            a[$2]++
        }
    }
}
END{
    for(i in c){
        print i" 2020  - " c[i] " reqs"
    }
    for(i in a){
        print i" 2021  - " a[i] " reqs"
    }
}
```
> Пример работы
```bash
[sitis@localhost lesson3]$ ./script access.log
Dec 2020  - 1 reqs
Feb 2021  - 14 reqs
Sep 2021  - 7 reqs
May 2021  - 27 reqs
Apr 2021  - 34 reqs
Jan 2021  - 43 reqs
Dec 2021  - 2 reqs
Nov 2021  - 106 reqs
Jul 2021  - 23 reqs
Mar 2021  - 2 reqs
Oct 2021  - 23 reqs
Aug 2021  - 108 reqs
Jun 2021  - 3 reqs
```
#### 3. Show total amount of data which server has provided for each unique ip (i.e. 100500 bytes for 1.2.3.4; 9001 bytes for 5.4.3.2 and so on)
> Поменял местами выводимый адрес и объем что бы выводилось аккуратнее. Так как запускал cкрипт на вертуалке на большом файле у меня он очень долго выполняет операцию (засекал примерно 2 минуты).
```bash
#!/usr/bin/awk -f
BEGIN{FS=OFS=" "}
!(($1) in b){
    b[$1]
    a[$1]++
        for(i in a){
        if ($1 == i){
            a[i] = a[i] + ($9$10)
        }
    }
}
END{
    for(i in a)
    {
        print i "\t\tcapacity\t " a[i]
    }
}
```
> Пример выполнения на небольшой части файла
```bash
185.136.116.142         capacity         20018957413
192.71.23.211           capacity         200305
46.167.202.170          capacity         20010467
188.163.104.73          capacity         20041683219
```

### Sed
#### 1. Change all browsers to "lynx"
> Использую тупой перебор.
```bash
[sitis@localhost lesson3]$  sed -e 's/Amigo/lynx/; s/Safari/lynx/; s/Avant/lynx/; s/Chromium/lynx/; s/Privacy/lynx/; s/Chrome/lynx/; s/K-Meleon/lynx/; s/Maxthon/lynx/; s/Explorer/lynx/; s/Mozilla/lynx/; s/"SeaMonkey/lynx/; s/Opera/lynx/; s/Orca/lynx/; s/Moon/lynx/; s/QIP/lynx/; s/Moon/lynx/; s/QtWeb/lynx/; s/Vivaldi/lynx/; s/AppleWebKit/lynx/' access1.log
```
> Пример выполнения на небольшой части файла
```bash
[sitis@localhost lesson3]$  sed -e 's/Amigo/lynx/; s/Safari/lynx/; s/Avant/lynx/; s/Chromium/lynx/; s/Privacy/lynx/; s/Chrome/lynx/; s/K-Meleon/lynx/; s/Maxthon/lynx/; s/Explorer/lynx/; s/Mozilla/lynx/; s/"SeaMonkey/lynx/; s/Opera/lynx/; s/Orca/lynx/; s/Moon/lynx/; s/QIP/lynx/; s/Moon/lynx/; s/QtWeb/lynx/; s/Vivaldi/lynx/; s/AppleWebKit/lynx/' access1.log
216.244.66.230 - - [19/Dec/2020:13:57:26 +0100] "GET /index.php?option=com_phocagallery&view=category&id=1:almhuette-raith&Itemid=53 HTTP/1.1" 200 32653 "-" "lynx/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" "-"
```
> Такого ужастного способа я не выдержал и смог придумать только через регулярное выражение совершить замену
```bash
[sitis@localhost lesson3]$ sed 's|"[A-Z].[a-z]*/|lynx|' access.log
```
> Пример выполнения на небольшой части файла
```bash
95.52.42.202 - - [04/Dec/2021:13:53:55 +0100] "HEAD /apache-log/access.log HTTP/1.1" 200 0 "-" lynx1.14 (linux-gnu)" "-"
95.52.42.202 - - [04/Dec/2021:13:54:20 +0100] "GET /apache-log/access.log HTTP/1.1" 200 131514477 "-" lynx1.14 (linux-gnu)" "-"
```

#### 2. Masquerade all ip addresses. Rewrite file.
> Вместо ip-адреса ставим `*****` с помощью просто регулярного выражения находим все ip-адреса
```bash
[sitis@localhost lesson3]$ sed -i 's/\([0-9]\{1,3\}[\.]\)\{3\}[0-9]\{1,3\}/*****/' access1.log
```

> Пример выполнения на небольшой части файла
```bash
[sitis@localhost lesson3]$ cat access1.log
***** - - [19/Dec/2020:15:23:13 +0100] "GET /images/stories/raith/almhuette_raith.jpg HTTP/1.1" 200 43300 "http://www.almhuette-raith.at/" "Mozilla/5.0 (Linux; U; Android 8.1.0; zh-CN; EML-AL00 Build/HUAWEIEML-AL00) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 baidu.sogo.uc.UCBrowser/11.9.4.974 UWS/2.13.1.48 Mobile Safari/537.36 AliApp(DingTalk/4.5.11) com.alibaba.android.rimet/10487439 Channel/227200 language/zh-CN" "-"
***** - - [19/Dec/2020:15:23:13 +0100] "GET /images/stories/raith/grillplatz.jpg HTTP/1.1" 200 55303 "http://www.almhuette-raith.at/" "Mozilla/5.0 (Linux; U; Android 8.1.0; zh-CN; EML-AL00 Build/HUAWEIEML-AL00) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 baidu.sogo.uc.UCBrowser/11.9.4.974 UWS/2.13.1.48 Mobile Safari/537.36 AliApp(DingTalk/4.5.11) com.alibaba.android.rimet/10487439 Channel/227200 language/zh-CN" "-"
```

Место для задачки под `*` если справлюсь

### Extra (*)
Show list of unique ips, who made more then 50 requests to the same url within 10 minutes (for example too many requests to "/")
> Сидел очень долго много вариантов перепробовал. не думаю что работает идеально.
```bash
#!/usr/bin/awk -f
BEGIN{FS=OFS=" |:"}
{
    !(($1$10) in colliction)
    {
        colliction[$1$10]
        colliction[$1$10]++
        finishTime[$5$6] = $5$6
        if (colliction[$1$10] == 1)
        {
            startTime[$5$6] = $5$6 
        }
    }
}

{
    for(i in colliction)
    {
        if (colliction[i] > 50 && (finishTime[$5$6]-startTime[$5$6]<10))
        {
            print "Error ====> " i " begin tome = " startTime[$5$6] " finish time = " finishTime[$5$6]
            colliction[i] = 0
        }
    }
}
```
