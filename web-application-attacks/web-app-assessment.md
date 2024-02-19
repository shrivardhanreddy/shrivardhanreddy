# Web-app Assessment

### Fingerprinting Web Servers with Nmap

```
sudo nmap -p80  -sV 192.168.50.20
sudo nmap -p80 --script=http-enum 192.168.50.20
```

**nmap** service scan (**-sV**) to grab the web server (**-p80**) banner

_http-enum ->_ performs an initial fingerprinting of the web server

### Technology Stack Identification with Wappalyzer

(Wappalyzer, 2022), [https://www.wappalyzer.com/](https://www.wappalyzer.com/) [↩︎](https://portal.offsec.com/courses/pen-200/books-and-videos/modal/modules/introduction-to-web-application-attacks/web-application-assessment-tools/technology-stack-identification-with-wappalyzer#fnref1)

### Directory Brute Force with Gobuster

```
gobuster dir -u 192.168.50.20 -w /usr/share/wordlists/dirb/common.txt -t 5
```

[https://www.kali.org/tools/gobuster/](https://www.kali.org/tools/gobuster/) [↩︎](https://portal.offsec.com/courses/pen-200/books-and-videos/modal/modules/introduction-to-web-application-attacks/web-application-assessment-tools/directory-brute-force-with-gobuster#fnref1)

### Security Testing with Burp Suite

With the Burp Proxy tool, we can intercept any request sent from the browser before it is passed on to the server.

Interceptor

Repeater, we can craft new requests or easily modify the ones in History, resend them, and review the responses. To observe this in action, we can right-click a request from _Proxy_ > _HTTP History_ and select _Send to Repeater_.

send to intruder \
\
<mark style="color:yellow;">nmap(webpage)->gobuster:(loginpage)->burp: interceptor->intruder->payload->attack</mark>
