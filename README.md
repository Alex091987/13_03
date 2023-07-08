# Домашнее задание к занятию `"«Защита сети»"` - `Дьяконов Алексей`


### Подготовка.

##### Установка Suricata.

1. `Подключаем репозиторий и устанавливаем пакеты`
```
    sudo apt install software-properties-common
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt update
    sudo apt install suricata
    sudo suricata-update
```
2. ` Правим интерфейс`
```
    sudo nano /etc/default/suricata

    IFACE=enp0s3

    sudo nano /etc/suricata/suricata.yaml

    EXTERNAL_NET: "any"

    af-packet:
       - interface: enp0s3

    pcap:
       - interface: enp0s3

    pfring:
       - interface: enp0s3

```

3. `Подключаем роли`
```
    sudo suricata-update -o /etc/suricata/rules/
    sudo systemctl restart suricata
    sudo systemctl status suricata
```
![Status suricata](/img/0_1.jpg)


4. `Запускаем`
```
   sudo suricata -c /etc/suricata/suricata.yaml -i enp0s3
```
5. `Проверяем логи`
```
    sudo tail /var/log/suricata/suricata.log
    sudo tail /var/log/suricata/stats.log
    sudo tail -f /var/log/suricata/fast.log
```

##### Установка Fail2ban.

1. `Установка пакетов`
```
    sudo apt install fail2ban
```
2. `Если не правильно определяет лог ssh`
```
    sudo nano /etc/fail2ban/jail.d/defaults-debian.conf

    В [sshd] добавляем
    logpath = /var/log/auth.log
```
3. `Проверяем логи`
```
    tail /var/log/fail2ban.log
    tail /var/log/auth.log
```

![Status fail2ban](/img/0_2.jpg)


##### Задание 1. Отличия  полного, дифференциального и инкреметного резервного копирования.

1. `sudo nmap -sA 192.168.1.205`

2. `sudo nmap -sS 192.168.1.205`
```
    07/07/2023-21:14:27.550608  [**] [1:2002911:6] ET SCAN Potential VNC Scan 5900-5920 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:48148 -> 192.168.1.205:5901
    07/07/2023-21:14:27.586966  [**] [1:2010939:3] ET SCAN Suspicious inbound to PostgreSQL port 5432 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.1.136:48148 -> 192.168.1.205:5432

```
3. `sudo nmap -sT 192.168.1.205`
```
    07/07/2023-21:17:30.106528  [**] [1:2010937:3] ET SCAN Suspicious inbound to mySQL port 3306 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.1.136:55810 -> 192.168.1.205:3306
    07/07/2023-21:17:30.174650  [**] [1:2010939:3] ET SCAN Suspicious inbound to PostgreSQL port 5432 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.1.136:43572 -> 192.168.1.205:5432
    07/07/2023-21:17:30.206213  [**] [1:2010935:3] ET SCAN Suspicious inbound to MSSQL port 1433 [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 192.168.1.136:57896 -> 192.168.1.205:1433
    07/07/2023-21:17:30.218035  [**] [1:2002911:6] ET SCAN Potential VNC Scan 5900-5920 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:44130 -> 192.168.1.205:5911
```

4. `sudo nmap -sV 192.168.1.205`
```
   07/07/2023-21:19:43.736900  [**] [1:2002911:6] ET SCAN Potential VNC Scan 5900-5920 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:60566 -> 192.168.1.205:5904
   07/07/2023-21:19:43.755665  [**] [1:2002910:6] ET SCAN Potential VNC Scan 5800-5820 [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:60566 -> 192.168.1.205:5802
   07/07/2023-21:19:50.495313  [**] [1:2010515:7] ET WEB_SERVER Possible HTTP 403 XSS Attempt (Local Source) [**] [Classification: Web Application Attack] [Priority: 1] {TCP} 192.168.1.205:8080 -> 192.168.1.136:34454
   07/07/2023-21:19:50.495313  [**] [1:2101201:11] GPL WEB_SERVER 403 Forbidden [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.205:8080 -> 192.168.1.136:34454
   07/07/2023-21:19:50.561552  [**] [1:2009358:6] ET SCAN Nmap Scripting Engine User-Agent Detected (Nmap Scripting Engine) [**] [Classification: Web Application Attack] [Priority: 1] {TCP} 192.168.1.136:34484 -> 192.168.1.205:8080
   07/07/2023-21:19:50.561552  [**] [1:2024364:4] ET SCAN Possible Nmap User-Agent Observed [**] [Classification: Web Application Attack] [Priority: 1] {TCP} 192.168.1.136:34484 -> 192.168.1.205:8080
   07/07/2023-21:19:50.561648  [**] [1:2101201:11] GPL WEB_SERVER 403 Forbidden [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.205:8080 -> 192.168.1.136:34484
   07/07/2023-21:19:50.581740  [**] [1:2010515:7] ET WEB_SERVER Possible HTTP 403 XSS Attempt (Local Source) [**] [Classification: Web Application Attack] [Priority: 1] {TCP} 192.168.1.205:8080 -> 192.168.1.136:34508
   07/07/2023-21:19:50.581740  [**] [1:2101201:11] GPL WEB_SERVER 403 Forbidden [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.205:8080 -> 192.168.1.136:34508

```

`В связи с вышеизложенными результатами сканирования можно сделать вывод, что самым скрытым вариантом сканирования является ack-сканирование. Вариант с распознаванием версий служб уже заставляет обратить внимание как на разведку перед атакой`


##### Задание 2. Установка bacula.

1. `Лог suricata`
```
    07/07/2023-21:26:26.700493  [**] [1:2260002:1] SURICATA Applayer Detect protocol only one direction [**] [Classification: Generic Protocol Command Decode] [Priority: 3] {TCP} 192.168.1.205:22 -> 192.168.1.136:48180
    07/07/2023-21:26:26.704584  [**] [1:2260002:1] SURICATA Applayer Detect protocol only one direction [**] [Classification: Generic Protocol Command Decode] [Priority: 3] {TCP} 192.168.1.205:22 -> 192.168.1.136:48238
    07/07/2023-21:26:30.062035  [**] [1:2003068:7] ET SCAN Potential SSH Scan OUTBOUND [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:48362 -> 192.168.1.205:22
    07/07/2023-21:26:30.426147  [**] [1:2210054:1] SURICATA STREAM excessive retransmissions [**] [Classification: Generic Protocol Command Decode] [Priority: 3] {TCP} 192.168.1.136:48124 -> 192.168.1.205:22
    07/07/2023-21:26:31.179111  [**] [1:2003068:7] ET SCAN Potential SSH Scan OUTBOUND [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:48374 -> 192.168.1.205:22
    07/07/2023-21:26:31.321615  [**] [1:2210054:1] SURICATA STREAM excessive retransmissions [**] [Classification: Generic Protocol Command Decode] [Priority: 3] {TCP} 192.168.1.136:48092 -> 192.168.1.205:22
    07/07/2023-21:26:32.086177  [**] [1:2210054:1] SURICATA STREAM excessive retransmissions [**] [Classification: Generic Protocol Command Decode] [Priority: 3] {TCP} 192.168.1.136:48282 -> 192.168.1.205:22
    07/07/2023-21:26:32.505705  [**] [1:2210054:1] SURICATA STREAM excessive retransmissions [**] [Classification: Generic Protocol Command Decode] [Priority: 3] {TCP} 192.168.1.136:48152 -> 192.168.1.205:22
    07/07/2023-21:26:33.409105  [**] [1:2003068:7] ET SCAN Potential SSH Scan OUTBOUND [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:48362 -> 192.168.1.205:22
    07/07/2023-21:26:37.457295  [**] [1:2003068:7] ET SCAN Potential SSH Scan OUTBOUND [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:48360 -> 192.168.1.205:22
    07/07/2023-21:26:45.538097  [**] [1:2003068:7] ET SCAN Potential SSH Scan OUTBOUND [**] [Classification: Attempted Information Leak] [Priority: 2] {TCP} 192.168.1.136:48360 -> 192.168.1.205:22
```
`Видны многократные попытки подключения по ssh, которые идентифицируются как возможный скан порта`

2. `Лог auth.log`
```
    Jul  7 21:26:26 vm1 sshd[1484]: Invalid user andrey from 192.168.1.136 port 48090
    Jul  7 21:26:26 vm1 sshd[1484]: Received disconnect from 192.168.1.136 port 48090:11: Bye Bye [preauth]
    Jul  7 21:26:26 vm1 sshd[1484]: Disconnected from invalid user andrey 192.168.1.136 port 48090 [preauth]
    Jul  7 21:26:26 vm1 sshd[524]: error: beginning MaxStartups throttling
    Jul  7 21:26:26 vm1 sshd[524]: drop connection #10 from [192.168.1.136]:48180 on [192.168.1.205]:22 past MaxStartups
    Jul  7 21:26:26 vm1 sshd[1488]: Invalid user andrey from 192.168.1.136 port 48108
    Jul  7 21:26:26 vm1 sshd[1496]: Invalid user maks from 192.168.1.136 port 48188
    Jul  7 21:26:26 vm1 sshd[1490]: Invalid user andrey from 192.168.1.136 port 48126
    Jul  7 21:26:26 vm1 sshd[1489]: Invalid user user from 192.168.1.136 port 48124
    Jul  7 21:26:26 vm1 sshd[1491]: Invalid user user from 192.168.1.136 port 48130
    Jul  7 21:26:26 vm1 sshd[1486]: Invalid user andrey from 192.168.1.136 port 48092
    Jul  7 21:26:26 vm1 sshd[1487]: Invalid user andrey from 192.168.1.136 port 48094
    Jul  7 21:26:26 vm1 sshd[1491]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:26 vm1 sshd[1491]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:26 vm1 sshd[1489]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:26 vm1 sshd[1489]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:26 vm1 sshd[1488]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:26 vm1 sshd[1488]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1486]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1486]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1490]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1490]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1496]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1496]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1487]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1487]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1494]: Invalid user user from 192.168.1.136 port 48154
    Jul  7 21:26:27 vm1 sshd[1500]: Invalid user maks from 192.168.1.136 port 48278
    Jul  7 21:26:27 vm1 sshd[1499]: Invalid user maks from 192.168.1.136 port 48254
    Jul  7 21:26:27 vm1 sshd[1493]: Invalid user user from 192.168.1.136 port 48152
    Jul  7 21:26:27 vm1 sshd[1500]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1500]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1499]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1499]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1492]: Invalid user andrey from 192.168.1.136 port 48140
    Jul  7 21:26:27 vm1 sshd[1494]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1494]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1492]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1492]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1493]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1493]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1515]: Invalid user maks from 192.168.1.136 port 48282
    Jul  7 21:26:27 vm1 sshd[1515]: pam_unix(sshd:auth): check pass; user unknown
    Jul  7 21:26:27 vm1 sshd[1515]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136
    Jul  7 21:26:27 vm1 sshd[1498]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136  user=root
    Jul  7 21:26:27 vm1 sshd[1497]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136  user=root
    Jul  7 21:26:27 vm1 sshd[1495]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.136  user=root
    Jul  7 21:26:28 vm1 sshd[1491]: Failed password for invalid user user from 192.168.1.136 port 48130 ssh2
    Jul  7 21:26:28 vm1 sshd[1488]: Failed password for invalid user andrey from 192.168.1.136 port 48108 ssh2
    Jul  7 21:26:28 vm1 sshd[1489]: Failed password for invalid user user from 192.168.1.136 port 48124 ssh2
    Jul  7 21:26:29 vm1 sshd[1498]: Failed password for root from 192.168.1.136 port 48218 ssh2
    Jul  7 21:26:29 vm1 sshd[1486]: Failed password for invalid user andrey from 192.168.1.136 port 48092 ssh2
    Jul  7 21:26:29 vm1 sshd[1490]: Failed password for invalid user andrey from 192.168.1.136 port 48126 ssh2
    Jul  7 21:26:29 vm1 sshd[1496]: Failed password for invalid user maks from 192.168.1.136 port 48188 ssh2
    Jul  7 21:26:29 vm1 sshd[1487]: Failed password for invalid user andrey from 192.168.1.136 port 48094 ssh2
    Jul  7 21:26:29 vm1 sshd[1500]: Failed password for invalid user maks from 192.168.1.136 port 48278 ssh2
    Jul  7 21:26:29 vm1 sshd[1499]: Failed password for invalid user maks from 192.168.1.136 port 48254 ssh2
    Jul  7 21:26:29 vm1 sshd[1494]: Failed password for invalid user user from 192.168.1.136 port 48154 ssh2
    Jul  7 21:26:29 vm1 sshd[1492]: Failed password for invalid user andrey from 192.168.1.136 port 48140 ssh2
    Jul  7 21:26:29 vm1 sshd[1493]: Failed password for invalid user user from 192.168.1.136 port 48152 ssh2
    Jul  7 21:26:29 vm1 sshd[1515]: Failed password for invalid user maks from 192.168.1.136 port 48282 ssh2
    Jul  7 21:26:29 vm1 sshd[1497]: Failed password for root from 192.168.1.136 port 48202 ssh2
    Jul  7 21:26:29 vm1 sshd[1495]: Failed password for root from 192.168.1.136 port 48168 ssh2
```
`После чего атакующий хост был заблокирован на 10 минут`

```
    root@vm1:/home/alexey# sudo fail2ban-client status sshd
    Status for the jail: sshd
    |- Filter
    |  |- Currently failed: 0
    |  |- Total failed:     30
    |  `- File list:        /var/log/auth.log
    `- Actions
        |- Currently banned: 1
        |- Total banned:     1
        `- Banned IP list:   192.168.1.136

    cat /var/log/fail2ban.log

    2023-07-07 21:26:26,672 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:26,849 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:26,893 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:26,905 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:26,906 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:26,912 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:26,948 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:26,950 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:26
    2023-07-07 21:26:27,046 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:27
    2023-07-07 21:26:27,062 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:27
    2023-07-07 21:26:27,074 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:27
    2023-07-07 21:26:27,078 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:27
    2023-07-07 21:26:27,114 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:27
    2023-07-07 21:26:27,319 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:27
    2023-07-07 21:26:27,435 fail2ban.actions        [493]: NOTICE  [sshd] Ban 192.168.1.136
    2023-07-07 21:26:28,683 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:28
    2023-07-07 21:26:28,709 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:28
    2023-07-07 21:26:28,711 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:28
    2023-07-07 21:26:29,177 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,222 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,228 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,228 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,236 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,286 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,298 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,327 fail2ban.actions        [493]: NOTICE  [sshd] 192.168.1.136 already banned
    2023-07-07 21:26:29,390 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,407 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,407 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,528 fail2ban.actions        [493]: NOTICE  [sshd] 192.168.1.136 already banned
    2023-07-07 21:26:29,575 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,663 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,679 fail2ban.filter         [493]: INFO    [sshd] Found 192.168.1.136 - 2023-07-07 21:26:29
    2023-07-07 21:26:29,729 fail2ban.actions        [493]: NOTICE  [sshd] 192.168.1.136 already banned
 
```
`Попыти подобрать пароль через hydra во время бана приводят к следующему`
```
    ─[✗]─[parrot@parrot]─[~]
    └──╼ $sudo hydra -L user.txt -P pass.txt 192.168.1.205 ssh
    Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

    Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-07-07 19:27:25
    [WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
    [DATA] max 16 tasks per 1 server, overall 16 tasks, 35 login tries (l:7/p:5), ~3 tries per task
    [DATA] attacking ssh://192.168.1.205:22/
    [ERROR] could not connect to ssh://192.168.1.205:22 - Connection refused

```