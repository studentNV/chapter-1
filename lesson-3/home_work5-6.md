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

#### 2. Show number of requests per month for ip 216.244.66.230 (for example: Sep 2016 - 100500 reqs, Oct 2016 - 0 reqs, Nov 2016 - 2 reqs...)
> Не уверен что понял задание написано в месяц то есть в один или в каждый. Может я не понял но вроде вот так. 
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
    !(($1) in d){
            d[$2]
            c[$2]++
        }
    }
}
{
if ($1~/216.244.66.230/ && $3~2021){
    !(($1) in b){
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
> Пример выполнения на небольшой часте файла
```bash
191.102.166.217         capacity         20010143809
172.90.18.189           capacity         404271
177.36.159.34           capacity         20010467
63.32.57.155            capacity         20010440
34.215.26.49            capacity         20010440
3.234.221.132           capacity         200555473
170.83.179.107          capacity         2006529025
40.94.33.12             capacity         404229
129.20.233.138          capacity         20010480
46.223.163.117          capacity         20077071873
99.7.17.72              capacity         20020815093
191.102.143.21          capacity         20010467
199.47.87.140           capacity         200305
185.136.116.142         capacity         20018957413
192.71.23.211           capacity         200305
46.167.202.170          capacity         20010467
188.163.104.73          capacity         20041683219
```

### Sed
#### 1. Change all browsers to "lynx"
> Из-за того что распарсить данный файл не представляеться возможным так как в одной колонке "user-agent" может находиться несколько браузеров использую тупой перебор.
```bash
[sitis@localhost lesson3]$  sed -e 's/Amigo/lynx/; s/Safari/lynx/; s/Avant/lynx/; s/Chromium/lynx/; s/Privacy/lynx/; s/Chrome/lynx/; s/K-Meleon/lynx/; s/Maxthon/lynx/; s/Explorer/lynx/; s/Mozilla/lynx/; s/"SeaMonkey/lynx/; s/Opera/lynx/; s/Orca/lynx/; s/Moon/lynx/; s/QIP/lynx/; s/Moon/lynx/; s/QtWeb/lynx/; s/Vivaldi/lynx/; s/AppleWebKit/lynx/' access1.log
```
> Пример выполнения на небольшой часте файла
```bash
[sitis@localhost lesson3]$  sed -e 's/Amigo/lynx/; s/Safari/lynx/; s/Avant/lynx/; s/Chromium/lynx/; s/Privacy/lynx/; s/Chrome/lynx/; s/K-Meleon/lynx/; s/Maxthon/lynx/; s/Explorer/lynx/; s/Mozilla/lynx/; s/"SeaMonkey/lynx/; s/Opera/lynx/; s/Orca/lynx/; s/Moon/lynx/; s/QIP/lynx/; s/Moon/lynx/; s/QtWeb/lynx/; s/Vivaldi/lynx/; s/AppleWebKit/lynx/' access1.log
216.244.66.230 - - [19/Dec/2020:13:57:26 +0100] "GET /index.php?option=com_phocagallery&view=category&id=1:almhuette-raith&Itemid=53 HTTP/1.1" 200 32653 "-" "lynx/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" "-"
216.244.66.230 - - [19/Dec/2020:14:08:06 +0100] "GET /apache-log/access.log HTTP/1.1" 200 233 "-" "lynx/5.0 (Windows NT 6.3; Win64; x64) lynx/537.36 (KHTML, like Gecko) lynx/87.0.4280.88 lynx/537.36" "-"
216.244.66.230 - - [19/Dec/2020:14:08:08 +0100] "GET /favicon.ico HTTP/1.1" 404 217 "http://www.almhuette-raith.at/apache-log/access.log" "lynx/5.0 (Windows NT 6.3; Win64; x64) lynx/537.36 (KHTML, like Gecko) lynx/87.0.4280.88 lynx/537.36" "-"
216.244.66.230 - - [19/Dec/2020:14:14:26 +0100] "GET /robots.txt HTTP/1.1" 200 304 "-" "lynx/5.0 (compatible; DotBot/1.1; http://www.opensiteexplorer.org/dotbot, help@moz.com)" "-"
216.244.66.230 - - [19/Dec/2020:14:16:44 +0100] "GET /index.php?option=com_phocagallery&view=category&id=2%3Awinterfotos&Itemid=53 HTTP/1.1" 200 30662 "-" "lynx/5.0 (compatible; AhrefsBot/7.0; +http://ahrefs.com/robot/)" "-"
216.244.66.230 - - [19/Dec/2020:14:29:21 +0100] "GET /administrator/index.php HTTP/1.1" 200 4263 "" "lynx/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322)" "-"
216.244.66.230 - - [19/Dec/2020:14:58:59 +0100] "GET /apache-log/access.log HTTP/1.1" 200 1299 "-" "lynx/5.0 (Windows NT 10.0; Win64; x64) lynx/537.36 (KHTML, like Gecko) lynx/87.0.4280.101 lynx/537.36" "-"
73.166.162.225 - - [19/Dec/2020:14:58:59 +0100] "GET /favicon.ico HTTP/1.1" 404 217 "http://www.almhuette-raith.at/apache-log/access.log" "lynx/5.0 (Windows NT 10.0; Win64; x64) lynx/537.36 (KHTML, like Gecko) lynx/87.0.4280.101 lynx/537.36" "-"
```

#### 2. Masquerade all ip addresses. Rewrite file.
> Вместо ip-адреса ставим `*****` с помощью просто регулярного выражения находим все ip-адреса
```bash
[sitis@localhost lesson3]$ sed -i 's/\([0-9]\{1,3\}[\.]\)\{3\}[0-9]\{1,3\}/*****/' access1.log
```

> Пример выполнения на небольшой часте файла
```bash
[sitis@localhost lesson3]$ cat access1.log
***** - - [19/Dec/2020:15:23:13 +0100] "GET /images/stories/raith/almhuette_raith.jpg HTTP/1.1" 200 43300 "http://www.almhuette-raith.at/" "Mozilla/5.0 (Linux; U; Android 8.1.0; zh-CN; EML-AL00 Build/HUAWEIEML-AL00) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 baidu.sogo.uc.UCBrowser/11.9.4.974 UWS/2.13.1.48 Mobile Safari/537.36 AliApp(DingTalk/4.5.11) com.alibaba.android.rimet/10487439 Channel/227200 language/zh-CN" "-"
***** - - [19/Dec/2020:15:23:13 +0100] "GET /images/stories/raith/grillplatz.jpg HTTP/1.1" 200 55303 "http://www.almhuette-raith.at/" "Mozilla/5.0 (Linux; U; Android 8.1.0; zh-CN; EML-AL00 Build/HUAWEIEML-AL00) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 baidu.sogo.uc.UCBrowser/11.9.4.974 UWS/2.13.1.48 Mobile Safari/537.36 AliApp(DingTalk/4.5.11) com.alibaba.android.rimet/10487439 Channel/227200 language/zh-CN" "-"
```

Место для задачки под `*` если справлюсь

