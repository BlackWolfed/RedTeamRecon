[![rootx](https://img.shields.io/website?label=root-X.dev&style=for-the-badge&url=https://root-x.dev/)](https://root-x.dev/)
[![Linkedin](https://img.shields.io/website?label=Linkedin&style=for-the-badge&url=https://www.linkedin.com/in/mostafa-bn-tamam-96308216a/)](https://www.linkedin.com/in/mostafa-bn-tamam-96308216a/)

> Written by Mostafa Tamam --> Red Team analyst || Bug Hunter 👋
### Connect with me:
[<img align="left" alt="codeSTACKr.com" width="22px" src="https://raw.githubusercontent.com/iconic/open-iconic/master/svg/globe.svg" />][rootx]
[<img align="left" alt="codeSTACKr | Twitter" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/twitter.svg" />][twitter]
[<img align="left" alt="codeSTACKr | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]
<br>

# Description
 🔭 This is a Recon & Inoformation Garhering Methodology In Bug Hunting Process

![Recon](Recon.png)

# 👀 Look at Program
1. First, read the scope policy for this program
2. Check site tools, versions, library, and what is website do, you should understand the service introduced by the website 
3. What CMS of the program (Version and Tools) It's important to note, however, CMS do much more than help manage the text and image content displayed on webpages.
> Check CMS by : 
- Wappalyzer [Extenstion]
- What is CMS [Website]
4. Finally in this step is learning tech you don't have, for example how to perform SQL injection without learning what is SQL query is, you should learn every service introduced by the website 

# 🔭 Recon & Info Gathering 
## 1. Perform Subdomain enumeration to your target , in my case gitlab.com is my target

### Sublist3r
 This is a great tool to enumerate subdomains of websites using OSINT using many search engines such as Google, Yahoo, Bing, Baidu and Ask.
 ```
$ python3 sublist3r.py -d gitlab.com -o /root/Desktop/subdomain
```
### Subfinder
Subfinder also is a subdomain discovery tool that discovers valid subdomains for websites by using passive online sources. It has a simple modular architecture and is optimized for speed.

```
$ subfinder -d gitlab.com -all -silent
```
### dmitry
DMitry (Deepmagic Information Gathering Tool) is a UNIX/(GNU)Linux Command Line program coded purely in C with the ability to gather as much information as possible about a host.

```
$ dmitry -wnse gitlab.com -o /root/Desktop/dmitry 
```
### VirusTotal
Also you can check virustotal website to get more information to your target and get more subdomain details


## 2. Check IPs used for your target by [shodan.io], [censys.io], [ipinfo.io]

### nslookup
 nslookup is a web based DNS client that queries DNS records for a given domain name. It allows you to view all the DNS records for a website
```
$ nslookup gitlab.com 
```

## 3. Check SSL/TLS certfication [crt.sh], [censys.io]
### sslscan
sslscan is a great tool to enumeration of server signature algorithms, scanSSLv2 and SSLv3 protocol 
```
$ sslscan  gitlab.com:80 > /root/Desktop/sslscan.txt
$ sslscan  gitlab.com:80:6061 > /root/Desktop/sslscan.txt
$ sslscan  gitlab.com:80:443 > /root/Desktop/sslscan.txt
```

## 4. Check open/closed/filtered ports & DNS record & OS version by : 
### nmap 
If you don't know about nmap close this methodology and go to your bed
```
$ nmap -sC -sV -p- -A -oN /root/Desktop/nmap gitlab.com

```
### masscan
This is an Internet-scale port scanner, It can scan the entire Internet in under 5 minutes, transmitting 10 million packets per second, from a single machine.
```
$ masscan 5.134.6.214 --ports 0-10000 
```
### rustscan
RustScan is a modern take on the port scanner. Sleek & fast. All while providing extensive extendability to you.
```
$ rustscan -T 1500 -b 500 13.58.194.87 -A -sC 
```
### nikto
Nikto is a web server scanner to get some inforamation may be useful for you
```
$ nikto -h gitlab.com 
```
## 5. Bruteforce directory to get more possible API endpoint don't forget follow this rule **more paths = more files, parameters -> more vulnerabilty** 

### Gobuster
Gobuster is a tool used to brute-force: URIs (directories and files) in web sites, DNS subdomains (with wildcard support), Virtual Host names on target web servers.

```
$ gobuster dir -u https://gitlab.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o /root/Desktop/endpoint -x php,txt,js

```
### LinkFinder
This tool is great, i usually use it to search paths,links
```
$ cat EndJS.txt|xargs -n2 -I@ bash -c "echo -e '\n[URL]: @\n'; python3 linkfinder.py -i @ -o cli" >> Endpoint.txt  
```
### TheHarvester
Theharvester is a tool for gathering e-mail accounts and subdomain names from public sources
```
$ theharvester -d gitlab.com -l 500 -b google 
```
### dirb
same thing in gobuster 
```
$ dirb https://gitlab.com -X php,js,txt 
```
### Seclist
It's a collection of multiple types of lists used during security assessments, collected in one place. List types include usernames, passwords, URLs, sensitive data patterns, fuzzing payloads, web shells, and many more. 

## 6. Screenshotting for server IPs, DNS record, Error message 

### HttpScreenShot
HTTPScreenshot is a tool for grabbing screenshots and HTML of large numbers of websites. The goal is for it to be both thorough and fast which can sometimes oppose each other.
```
$ ./httpscreenshot.py -i \<gnmapFile\> -p -w 40 -a -vH
```

## 7. Using recon-ng tool to enumerate and get more information about target
### recon-ng
Recon-ng is a reconnaissance tool with an interface similar to Metasploit. Running recon-ng from the command line, you enter a shell like environment where you can configure options, perform recon and output results to different report types.
- Check this article to know about this tool : https://hackertarget.com/recon-ng-tutorial/

## 8. Find collect possibly several javascript files
### jsfinder
I'm recommend this tool you can crawl useful Endpoints and we can also do BLH discovery.
```
$ python3 JSFinder.py -u https://gitlab.com -d -j -ou /root/Desktop/Endpoint
```
### gau
This tool is great, i usually use it to search for as many javascript files as possible, many companies host their files on third parties, this thing is very for important for a bughunter because then really enumerate a lot js files!
```
$  gau paypalobjects.com |grep -iE '\.js'|grep -ivE '\.json'|sort -u  >> paypalJS.txt
```
## 9. Check Google dorks and Github resource
### do-search
This is a tool by using google dorks for advanced searching in google and other google applications to find security holes in the configuration and computer code that used in the websites
```
$  python3 do-search.py 
```

## 10.  Happy Hacking 🔥

============================================================================================
# ⚡ URLs For Tools

- wappalyzer: https://www.wappalyzer.com
- what CMS: https://whatcms.org/
- Sublist3r: https://github.com/aboul3la/Sublist3r
- Subfinder: https://github.com/projectdiscovery/subfinder
- dmitry: https://github.com/jaygreig86/dmitry
- VirusTotal: https://www.virustotal.com/gui/home/search
- nslookup: --> apt install dnsutils
- shodan.io
- censys.io
- ipinfo.io
- sslscan: https://github.com/rbsec/sslscan
- nmap: https://github.com/nmap/nmap
- masscan: https://github.com/robertdavidgraham/masscan
- rustscan: https://github.com/RustScan/RustScan
- nikto: https://github.com/sullo/nikto
- Gobuster: https://github.com/OJ/gobuster
- LinkFinder: https://github.com/GerbenJavado/LinkFinder
- TheHarvester: https://github.com/laramies/theHarvester
- dirb: https://github.com/v0re/dirb
- Seclist: https://github.com/danielmiessler/SecLists
- HttpScreenShot: https://github.com/breenmachine/httpscreenshot
- recon-ng: https://github.com/lanmaster53/recon-ng
- jsfinder:https://github.com/Threezh1/JSFinder
- gau: https://github.com/lc/gau
- do-search:https://github.com/BlackWolfed/do-search
- GHDB: https://www.exploit-db.com/google-hacking-database


==============================================
# Useful Links
- https://www.triaxiomsecurity.com/our-external-penetration-testing-methodology/ **External Penetration Testing Methodology**
- https://www.triaxiomsecurity.com/our-internal-penetration-testing-methodology/  **Internal Penetration Testing Methodology**
- https://github.com/arch3rPro/PentestTools  **All Pentest tool**
- https://github.com/swisskyrepo/PayloadsAllTheThings  **All Payload and Tech to find vulnerability**


[rootx]: https://root-x.dev
[Website]: https://whatcms.org/
[Extenstion]: https://www.wappalyzer.com/
[Linkedin]: https://www.linkedin.com/in/mostafa-bn-tamam-96308216a/
[twitter]: https://twitter.com/BlackWo50331384
