# Lesson 3

## Home work 5-6

### Awk /ɔːk/

#### 1. What is the most frequent browser?
```bash
[sitis@localhost lesson3]$  awk -F [\"\(] '{print $6}' access.log | sort | uniq -c | sort -fr
```
> Пример работы
```bash
#!/usr/bin/awk -f
BEGIN {FS="\""}
{
    if ($6 == $6)
    User_Agent[$6]++
}
END{
    
    for(i in User_Agent)
    {
        if (User_Agent[i] > count_Popular)
        {
            count_Popular = User_Agent[i]
            mostFrequentBrowser = ("Count = " User_Agent[i] " most frequent browser: " i)
        }
    }
    print mostFrequentBrowser
}
```

#### 2. Show number of requests per month for ip 216.244.66.230 (for example: Sep 2016 - 100500 reqs, Oct 2016 - 0 reqs, Nov 2016 - 2 reqs...)
> Если необходимо вывести все месяца
```bash
#!/usr/bin/awk -f
BEGIN {FS="/"}
{
    year = substr($3,1,4)
    if ($1~/216.244.66.230/ && $3~year)
    {
        MonthAndCountOfReqs[year$2]++
    }
}
END{
    for(i in MonthAndCountOfReqs){
        print  substr(i,1,4)" " substr(i,5,6)  " - " MonthAndCountOfReqs[i] " reqs"
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
{
    arrayForIpAndCapacity[$1]  += ($10)
}
END{
    for(i in arrayForIpAndCapacity)
    {
        print i "\t\tcapacity\t " arrayForIpAndCapacity[i]
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
> Такого ужастного способа я не выдержал и смог придумать только через регулярное выражение совершить замену
```bash
[sitis@localhost lesson3]$ sed 's|"[A-Z].[a-z].[^"]*"|lynx|' access.log
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
            print "Error ====> " i " begin time = " startTime[$5$6] " finish time = " finishTime[$5$6]
            colliction[i] = 0
        }
    }
}
```
