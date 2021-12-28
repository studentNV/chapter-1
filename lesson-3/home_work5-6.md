# Lesson 3

## Home work 5-6

### Awk /ɔːk/

#### 1. What is the most frequent browser?
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
[sitis@localhost ~]$ ./script access.log
Count = 340874 most frequent browser: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)
```

#### 2. Show number of requests per month for ip 216.244.66.230 (for example: Sep 2016 - 100500 reqs, Oct 2016 - 0 reqs, Nov 2016 - 2 reqs...)
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
[sitis@localhost ~]$ ./script access.log
2021 Dec - 5 reqs
2021 Nov - 106 reqs
2021 Jul - 23 reqs
2021 Mar - 2 reqs
2021 Oct - 23 reqs
2021 Aug - 108 reqs
2021 Jun - 3 reqs
2021 Feb - 14 reqs
2021 Sep - 7 reqs
2021 May - 27 reqs
2021 Apr - 34 reqs
2021 Jan - 43 reqs
2020 Dec - 1 reqs
```

#### 3. Show total amount of data which server has provided for each unique ip (i.e. 100500 bytes for 1.2.3.4; 9001 bytes for 5.4.3.2 and so on)
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
[sitis@localhost ~]$ ./script access.log
109.146.137.238         capacity         668197537
31.40.226.9             capacity         2022488
157.55.39.145           capacity         75393
213.226.113.93          capacity         11863
5.183.60.18             capacity         11863
157.33.1.197            capacity         81982479
73.45.141.120           capacity         24897504
204.15.145.39           capacity         13020
157.55.39.148           capacity         10184644
159.89.187.19           capacity         34198293
94.154.220.93           capacity         10466
145.253.118.26          capacity         26209910
```

### Sed
#### 1. Change all browsers to "lynx"
```bash
[sitis@localhost lesson3]$ sed 's|"[A-Z].[a-z].[^"]*"|lynx|' access.log
[sitis@localhost ~]$ sed 's|"[A-Z].[a-z].[^"]*"|lynx|' access.log
193.106.31.130 - - [21/Jan/2021:02:17:44 +0100] "POST /administrator/index.php HTTP/1.0" 200 4481 "-" lynx "-"
193.106.31.130 - - [21/Jan/2021:02:17:44 +0100] "POST /administrator/index.php HTTP/1.0" 200 4481 "-" lynx "-"
193.106.31.130 - - [21/Jan/2021:02:17:44 +0100] "POST /administrator/index.php HTTP/1.0" 200 4481 "-" lynx "-"
193.106.31.130 - - [21/Jan/2021:02:17:45 +0100] "POST /administrator/index.php HTTP/1.0" 200 4481 "-" lynx "-"
193.106.31.130 - - [21/Jan/2021:02:17:45 +0100] "POST /administrator/index.php HTTP/1.0" 200 4481 "-" lynx "-"
193.106.31.130 - - [21/Jan/2021:02:17:45 +0100] "POST /administrator/index.php HTTP/1.0" 200 4481 "-" lynx "-"
193.106.31.130 - - [21/Jan/2021:02:17:45 +0100] "POST /administrator/index.php HTTP/1.0" 200 4481 "-" lynx "-"
```

#### 2. Masquerade all ip addresses. Rewrite file.
> Вместо ip-адреса ставим `*****` с помощью просто регулярного выражения находим все ip-адреса
```bash
[sitis@localhost lesson3]$ sed -i 's/\([0-9]\{1,3\}[\.]\)\{3\}[0-9]\{1,3\}/*****/' access1.log
[sitis@localhost lesson3]$ cat access1.log
***** - - [19/Dec/2020:15:23:13 +0100] "GET /images/stories/raith/almhuette_raith.jpg HTTP/1.1" 200 43300 "http://www.almhuette-raith.at/" "Mozilla/5.0 (Linux; U; Android 8.1.0; zh-CN; EML-AL00 Build/HUAWEIEML-AL00) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 baidu.sogo.uc.UCBrowser/11.9.4.974 UWS/2.13.1.48 Mobile Safari/537.36 AliApp(DingTalk/4.5.11) com.alibaba.android.rimet/10487439 Channel/227200 language/zh-CN" "-"
***** - - [19/Dec/2020:15:23:13 +0100] "GET /images/stories/raith/grillplatz.jpg HTTP/1.1" 200 55303 "http://www.almhuette-raith.at/" "Mozilla/5.0 (Linux; U; Android 8.1.0; zh-CN; EML-AL00 Build/HUAWEIEML-AL00) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/57.0.2987.108 baidu.sogo.uc.UCBrowser/11.9.4.974 UWS/2.13.1.48 Mobile Safari/537.36 AliApp(DingTalk/4.5.11) com.alibaba.android.rimet/10487439 Channel/227200 language/zh-CN" "-"
```
