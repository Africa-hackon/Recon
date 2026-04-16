# Dorking Scripts

A collection of Google dork, Shodan dork, and GitHub dork queries for reconnaissance and bug bounty hunting.

> Use responsibly and only on authorized targets.

---

## 1. Google Dork Commands

Google Dorking uses advanced search operators to surface sensitive information indexed by search engines.

### 1.1 Core Search Filters

| Filter | Description | Example |
|--------|-------------|---------|
| `allintext:` | Searches for occurrences of ALL keywords given | `allintext:"keyword1" "keyword2"` |
| `intext:` | Searches for keyword occurrences (one at a time) | `intext:"keyword"` |
| `inurl:` | Searches for a URL matching a keyword | `inurl:"admin"` |
| `allinurl:` | Searches for URL matching ALL keywords | `allinurl:"admin login"` |
| `intitle:` | Searches for keywords in page title | `intitle:"index of"` |
| `allintitle:` | Searches for ALL keywords in page title | `allintitle:"login admin"` |
| `site:` | Restricts results to a specific site | `site:example.com` |
| `filetype:` | Searches for a specific file type | `filetype:pdf` |
| `ext:` | Alternative to filetype | `ext:pdf` |
| `link:` | Searches for external links to pages | `link:"keyword"` |
| `numrange:` | Locates specific numbers in searches | `numrange:321-325` |
| `before:/after:` | Search within a date range | `filetype:pdf before:2023-01-01` |
| `inanchor:` | Sites with keywords in links pointing to them | `inanchor:"login"` |
| `related:` | List pages similar to a specified URL | `related:www.google.com` |
| `cache:` | Shows Google's cached version of a page | `cache:www.example.com` |

### 1.2 Operators & Logic

| Operator | Usage | Example |
|----------|-------|---------|
| `"exact phrase"` | Match exact phrase in quotes | `"confidential salary"` |
| `OR \|` | Logical OR between terms | `site:facebook.com \| site:twitter.com` |
| `AND &` | Logical AND between terms | `site:facebook.com & site:twitter.com` |
| `-exclude` | Exclude results containing term | `site:facebook.* -site:facebook.com` |
| `~synonym` | Include synonyms of keyword | `~configure` |
| `* wildcard` | Matches any word in a query | `site:*.com` |
| `( ) grouping` | Group operators for complex queries | `(site:fb.com \| site:twitter.com) & intext:"login"` |

### 1.3 File & Data Exposure Dorks

**Sensitive File Types**
```
ext:(doc | pdf | xls | txt | rtf | odt | ppt) intext:("confidential")
site:*.gov filetype:xls intext:"password"
filetype:txt intext:"username" + "password"
site:ke filetype:xls salary
site:example.com ext:pdf
intext:@kplc.co.ke ext:pdf
intext:@kplc.co.ke ext:txt
```

**Credential / Account Dorks**
```
intext:@gmail.com OR @yahoo.com OR @hotmail.com AND account AND password AND netflix ext:txt
filetype:txt intext:"xxxx" + password | username
filetype:config inurl:web.config inurl:ftp
"Windows XP Professional" 94FBR
```

### 1.4 Directory Listing Dorks

```
intitle:"index of /"
intitle:"index.of" "parent directory" "size" "last modified"
parent directory MP3 -xxx -html -htm -php
"To Parent Directory" AND "dir>" AND "config"
```

### 1.5 Web App / Admin Panel Dorks

```
inurl:admin filetype:php intitle:"login page"
site:*/wp-admin/ -public intitle:WordPress
inurl:wp-content/uploads/ filetype:pdf
intitle:"phpMyAdmin" inurl:phpmyadmin
intitle:"Kibana" inurl:5601
```

---

## 2. Shodan Dork Commands

Shodan is an IoT search engine that indexes open ports, service banners, and device info.

### 2.1 Core Shodan Filters

| Filter | Description | Example |
|--------|-------------|---------|
| `port:` | Search by specific open port | `port:22` |
| `net:` | Search by IP address or CIDR range | `net:192.168.1.0/24` |
| `hostname:` | Locate devices by hostname | `hostname:example.com` |
| `os:` | Search by Operating System | `os:"windows 7"` |
| `city:` | Locate devices by city | `city:"Nairobi"` |
| `country:` | Locate devices by country code | `country:KE` |
| `geo:` | Locate devices by coordinates | `geo:"-1.2921,36.8219"` |
| `org:` | Search by organization/ISP name | `org:"Safaricom"` |
| `product:` | Search by software product | `product:"Apache"` |
| `version:` | Search by software version | `version:"2.4.7"` |
| `before:/after:` | Search by scan date range | `apache after:22/02/2023` |
| `has_screenshot:true` | Only show devices with screenshots | `has_screenshot:true port:3389` |
| `vuln:` | Find devices with known CVEs | `vuln:"CVE-2021-44228"` |
| `ssl.cert.subject.cn:` | Search by SSL cert common name | `ssl.cert.subject.cn:*.example.com` |
| `ssl.cert.expired:true` | Find expired SSL certificates | `ssl.cert.expired:true` |
| `http.title:` | Search by HTTP page title | `http.title:"Admin Panel"` |
| `http.html:` | Search within page HTML content | `http.html:"wp-config"` |
| `device:` | Search by device type | `device:webcam` |
| `tag:` | Search by service category | `tag:industrial` |

### 2.2 Common Port Quick Reference

| Port | Service | Shodan Dork |
|------|---------|-------------|
| 21 | FTP | `port:21` |
| 22 | SSH | `port:22` |
| 23 | Telnet | `port:23` |
| 80 | HTTP | `port:80` |
| 443 | HTTPS | `port:443` |
| 3306 | MySQL | `port:3306` |
| 3389 | RDP (Remote Desktop) | `port:3389` |
| 5432 | PostgreSQL | `port:5432` |
| 6379 | Redis | `port:6379` |
| 8080 | HTTP Alternate / Jenkins | `port:8080` |
| 27017 | MongoDB | `port:27017` |
| 9200 | Elasticsearch | `port:9200` |

### 2.3 Device Type Dorks

| Device | Dork |
|--------|------|
| Webcams (US) | `webcamxp country:"US"` |
| IP Cameras with default creds | `device:"webcam" http.title:"IP Camera"` |
| Routers | `device:router` |
| Firewalls | `device:firewall` |
| VoIP Phones | `device:"voip phone"` |
| Printers | `device:printer` |
| Industrial Control Systems | `tag:industrial port:502` |
| Android devices (ADB) | `port:5555 product:adb` |
| Telnet access (no auth) | `port:23 -authentication` |

### 2.4 Service & Vulnerability Dorks

**Exposed Databases**
```
product:"MongoDB" port:27017 -authentication
"MongoDB Server Information" port:27017 -authentication
product:"MySQL" port:3306 -authentication
port:9200 product:"Elasticsearch"
port:6379 product:"Redis" -authentication
```

**Exposed Remote Access**
```
port:3389 has_screenshot:true country:KE
port:22 os:"Linux" country:KE
"Authentication: disabled" port:445
```

**Web App Vulnerabilities**
```
http.html:"* The wp-config.php creation script uses this file"
html:"def_wirelesspassword"
http.title:"Dashboard [Jenkins]" port:8080
http.component:"WordPress" vuln:CVE-2021-44228
```

**SSL / Certificate Research**
```
ssl.cert.subject.cn:"example.com"
ssl.cert.issuer.cn:"Let's Encrypt"
ssl.cert.expired:true org:"YourOrg"
```

### 2.5 Combined / Advanced Queries

| Goal | Combined Dork |
|------|---------------|
| Linux SSH servers in Japan | `os:"Linux" port:"22" country:"JP"` |
| Apache 2.4.7 not returning 200 OK | `product:"Apache" version:"2.4.7" -"200 OK"` |
| Windows RDP in Nairobi | `city:"Nairobi" os:"Windows" port:"3389"` |
| Expired SSL on Google infra | `org:"Google" ssl.cert.expired:true` |
| MySQL in Kenya with default password | `country:"KE" product:"MySQL" "default password"` |
| Apache servers in Brazil | `"Server: Apache" -"Apache-Coyote" country:"BR"` |
| Edu institutions with CVE exposure | `hostname:"*.edu" vuln:"CVE-2019-11510"` |
| OpenSSH on Ubuntu in US | `country:US port:22 os:"Ubuntu" product:"OpenSSH"` |

---

## 3. GitHub Dork Commands

GitHub dorks use the GitHub search interface to uncover accidentally exposed credentials, API keys, config files, and other sensitive data in public repositories.

> Use only on public, authorized repositories. Report findings responsibly.

### 3.1 GitHub Search Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `filename:` | Search for a specific filename | `filename:.env` |
| `extension:` | Search by file extension | `extension:pem` |
| `path:` | Search within a specific directory path | `path:/config/` |
| `language:` | Filter by programming language | `language:python` |
| `org:` | Limit search to a GitHub organization | `org:targetcompany` |
| `user:` | Limit search to a specific user | `user:targetuser` |
| `in:file` | Search within file content | `"AWS_SECRET_ACCESS_KEY" in:file` |
| `in:path` | Search within file paths | `"config" in:path` |
| `size:` | Filter by file size (KB) | `size:<1000` |

### 3.2 Credential & Secret Dorks

**SSH Keys & Certificates**
```
extension:pem private
filename:id_rsa
filename:id_ed25519
"BEGIN RSA PRIVATE KEY"
"BEGIN OPENSSH PRIVATE KEY"
```

**AWS & Cloud Credentials**
```
"AWS_ACCESS_KEY_ID"
"AWS_SECRET_ACCESS_KEY"
filename:.bash_profile aws
"AWSSecretKey"
language:json HEROKU_API_KEY
"digitalocean" extension:env
```

**Database Credentials**
```
extension:sql mysql dump
extension:sql mysql dump password
"SQLALCHEMY_DATABASE_URI"
filename:config.php dbpasswd
"mongodb://" in:file
"mysql://root:" in:file
"postgres://" in:file
```

**API Keys & Tokens**
```
"api_key" in:file
"api_secret" in:file
"client_secret" language:python
"GITHUB_TOKEN=" in:file
shodan_api_key language:python
extension:json mongolab.com
"slack_token" in:file
```

**Configuration Files**
```
filename:.env
filename:config.yml
filename:database
filename:.aws/credentials
filename:.htpasswd
filename:.git-credentials
filename:wp-config.php
filename:filezilla.xml Pass
filename:recentservers.xml Pass
```

**Shell History Files**
```
filename:.bashrc password
filename:.bash_history
filename:.sh_history
```

### 3.3 Environment Variable Dorks

| Token/Secret | GitHub Dork |
|--------------|-------------|
| Docker credentials | `"DOCKER_PASSWORD=" in:file` |
| NPM tokens | `"NPM_TOKEN=" in:file` |
| GitHub OAuth secrets | `"GH_OAUTH_CLIENT_SECRET=" in:file` |
| Sauce Labs keys | `"SAUCE_ACCESS_KEY=" in:file` |
| Coveralls tokens | `"COVERALLS_REPO_TOKEN=" in:file` |
| Surge login | `"SURGE_TOKEN=" in:file` |
| Cloudinary URL | `"CLOUDINARY_URL=" in:file` |
| Pivotaltracker token | `"PIVOTAL_TRACKER_TOKEN=" in:file` |

### 3.4 Combined GitHub Dork Examples

```
filename:.env DB_PASSWORD
"api_key" extension:js "client_secret"
"mongodb://" "mysql://root:" in:file language:python
filename:.npmrc _auth
org:targetcompany filename:.env password
user:targetuser extension:pem private
```

### 3.5 Automation Tools

| Tool | Usage |
|------|-------|
| github-dorks | `github-dork.py -u <username>` OR `-r <repo>` |
| trufflehog | `trufflehog github --org Target --results=verified` |
| gitleaks | `gitleaks detect --source ./repo -v` |
| gitdorks | `gitdorks -q 'filename:.env password' -o targetcompany` |
| noseyparker | `noseyparker scan --datastore np.db org/` |
| ggshield | `ggshield secret scan repo <path-or-url>` |

---

## 4. Quick Reference

| Category | Tool / Command | Purpose |
|----------|----------------|---------|
| Recon CLI | `subfinder -d target.com \| tee subdomains.txt` | Subdomain enumeration |
| Recon CLI | `findomain -q -t target.com \| tee findomain.txt` | Alternate subdomain enum |
| Recon CLI | `cat subdomains.txt \| httpx -silent` | Find live subdomains |
| Recon CLI | `wafw00f -i subdomains.txt \| tee wafw00f.txt` | WAF detection |
| Recon CLI | `wpscan --random-user-agent --url https://target.com` | WordPress scan |
| Recon CLI | `whois target.com` | WHOIS lookup |
| Recon CLI | `nslookup target.com` | DNS lookup |
| Recon CLI | `dnsenum target.com` | DNS enumeration |
| Online Tools | shodan.io | IoT/device search engine |
| Online Tools | dnsdumpster.com | DNS recon & mapping |
| Online Tools | web-check.xyz | Website analysis |
| Online Tools | haveibeenpwned.com | Breach data check |
| Online Tools | haveibeensquatted.com | Typosquat detection |
| Online Tools | intelx.io | OSINT data intelligence |
| Online Tools | hunter.io | Email discovery |
| Online Tools | whatsmyname.app | Username recon |
| Online Tools | osint.industries | OSINT aggregator |
| Online Tools | tineye.com | Reverse image search |
| Online Tools | pimeyes.com | Face search engine |
| Online Tools | phonebook.cz | Email/domain OSINT |
| Online Tools | lookups.io | Username/email lookup |
| Online Tools | socialeye.net | Social media OSINT |
| Online Tools | metadata2go.com | File metadata extractor |
| Online Tools | search.criminalip.io | IP threat intelligence |
| Online Tools | members.leakradar.io | Leaked data search |
| Online Tools | app.osintnova.com | OSINT aggregation |
| Wappalyzer | wappalyzer.com | Tech stack detection |
| JS Console | `document.designMode = 'on'` | Edit any webpage in browser |
