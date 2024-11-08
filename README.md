# Домашнее задание к занятию "`Базы данных в облаке`" - `Маркин Алексей`

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)
  
*Приведите ответ в свободной форме.* 

---

### Решение 1

![Задание 1](https://github.com/Markin-AI/13-1/blob/main/img/1-1.png)

![Задание 1](https://github.com/Markin-AI/13-1/blob/main/img/1-2.png)

Обнаруженные уязвимости

- vsftpd 2.3.4 https://www.exploit-db.com/exploits/49757

- ProFTPd 1.3 https://www.exploit-db.com/exploits/32798

- RealVNC 3.3.7 https://www.exploit-db.com/exploits/16489


---

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

*Приведите ответ в свободной форме.*

---

### Решение 2

1. SYN сканирование (полуоткрытое сканирование или сканирование с использованием флага SYN):  

 - Трафик: Отправляет пакеты TCP с флагом SYN, пытаясь начать соединение с каждым портом.  
 - Ответ сервера:  
          - Открытый порт отвечает пакетом SYN-ACK, говоря о готовности установить соединение.  
          - Закрытый порт отвечает пакетом RST (reset), предотвращая установление соединения.  
- Nmap прекращает или завершает процесс рукопожатия, отправляя RST, если порт открыт. 

![Задание 2](https://github.com/Markin-AI/13-1/blob/main/img/2-1.png)

2. FIN сканирование:  

 - Трафик: Отправляет пакеты TCP с установленным FIN флагом, что обычно означает завершение соединения.  
 - Ответ сервера:  
          - По стандарту TCP, открытые порты игнорируют пакеты FIN в отсутствие активного соединения, и не отвечают на них.  
          - Закрытые порты должны отвечать пакетом RST.

![Задание 2](https://github.com/Markin-AI/13-1/blob/main/img/2-2.png)
		  

3. Xmas сканирование:

 - Трафик: Отправляет пакеты TCP с флагами FIN, PSH и URG, что создаёт "освещённый" или "рождественский" пакет (отсюда и название).  
 - Ответ сервера:  
        - Аналогично с FIN сканированием, открытые порты обычно не отвечают на такие пакеты.  
 - Закрытые порты отвечают пакетом RST.

![Задание 2](https://github.com/Markin-AI/13-1/blob/main/img/2-3.png)
 

4. UDP сканирование:  

 - Трафик: Отправляет UDP пакеты на разные порты.  
 - Ответ сервера:  
        - Если порт UDP открыт и прослушивается, то сервер может не ответить вообще, либо ответить со специфическими для приложения данными.  
        - Если UDP порт закрыт, то сервер отвечает ICMP пакетом "port unreachable".  
		
![Задание 2](https://github.com/Markin-AI/13-1/blob/main/img/2-4.png)

[Файл сеанса запросов](https://github.com/Markin-AI/13-1/blob/main/13-1.pcapng)

Основные различия между этими режимами связаны с типами флагов, устанавливаемых в заголовке TCP, и с поведением сервера при получении этих пакетов.   
SYN сканирование является наиболее спокойным и надежным методом для определения состояния портов, в то время как остальные методы могут быть неэффективны против современных межсетевых экранов и систем обнаружения вторжений, которые могут обнаружить и заблокировать такие сканирования.   
UDP сканирование отличается от TCP сканирования тем, что использует протокол UDP вместо TCP и в некоторых случаях может требовать больше времени для получения ответов, так как UDP — это протокол без установления соединения.

---