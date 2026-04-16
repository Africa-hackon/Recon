# Recon Tools

A collection of reconnaissance and security testing tools used for bug bounty and penetration testing.

---

### Prerequisites

#### Go

Required for most tools below.

1. Download the latest Go tarball:
   ```bash
   wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
   ```
2. Extract and install:
   ```bash
   sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
   ```
3. Add to PATH in `~/.bashrc` or `~/.zshrc`:
   ```bash
   export PATH=$PATH:/usr/local/go/bin
   export PATH=$PATH:$(go env GOPATH)/bin
   ```
4. Apply and verify:
   ```bash
   source ~/.bashrc
   go version
   ```

---

### Subdomain Enumeration

#### subfinder
**Install:**
```bash
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```
**Usage:**
```bash
subfinder -all -silent -d target.com | tee subdomains.txt
```

#### findomain
**Install:**
```bash
curl -LO https://github.com/Findomain/Findomain/releases/latest/download/findomain-linux
chmod +x findomain-linux
sudo mv findomain-linux /usr/local/bin/findomain
```
**Usage:**
```bash
findomain -q -t target.com | tee findomain.txt
```

#### assetfinder
**Install:**
```bash
go get -u github.com/tomnomnom/assetfinder
```
**Usage:**
```bash
assetfinder --subs-only target.com | tee assetfinder.txt
```

---

### Combining Results

After running subfinder, findomain, and assetfinder, merge all results into one deduplicated file using `anew`:
```bash
cat findomain.txt | anew subdomains.txt
cat subfinder.txt | anew subdomains.txt
cat assetfinder.txt | anew subdomains.txt
cat subdomains.txt | wc -l
```

---

### HTTP Probing & WAF Detection

#### httpx
**Install:**
```bash
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
```
**Usage:**
```bash
cat subdomains.txt | httpx -silent | tee active-subdomains.txt
cat active-subdomains.txt | wc -l
```

#### wafw00f
**Install:**
```bash
pip install wafw00f
```
**Usage:**
```bash
wafw00f -i subdomains.txt | tee wafw00f.txt
```

---

### Vulnerability Scanning

#### nuclei
**Install:**
```bash
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
```
**Usage:**
```bash
nuclei -l active-subdomains.txt -o nuclei-results.txt
```

#### sqlmap
**Install:**
```bash
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
cd sqlmap-dev
python3 sqlmap.py --version
```
**Usage:**
```bash
python3 sqlmap.py -u "https://target.com/page?id=1" --dbs
```

#### ghauri
**Install:**
```bash
git clone https://github.com/r0oth3x49/ghauri.git
cd ghauri
pip install -r requirements.txt
python3 ghauri.py --version
```
**Usage:**
```bash
python3 ghauri.py -u "https://target.com/page?id=1" --dbs
```

#### wpscan
**Install:**
```bash
gem install wpscan
```
> Requires Ruby. Install with: `sudo apt install ruby ruby-dev`

**Usage:**
```bash
wpscan --random-user-agent --url https://target.com
```

#### Metasploit
**Install:**
```bash
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall
chmod +x msfinstall
sudo ./msfinstall
```
**Usage:**
```bash
msfconsole
```

---

### URL & Archive Discovery

#### waybackurls
**Install:**
```bash
go install github.com/tomnomnom/waybackurls@latest
```
**Usage:**
```bash
echo target.com | waybackurls | tee wayback-urls.txt
```

---

### DNS & WHOIS Enumeration

```bash
whois target.com
nslookup target.com
dnsenum target.com
```

---

### Network & SMB Enumeration

#### smbclient-ng
**Install:**
```bash
pip install smbclientng
```
**Usage:**
```bash
smbclientng -d target.com -u username
```

#### enum4linux
**Install:**
```bash
sudo apt install enum4linux
```
> Or clone manually:
> ```bash
> git clone https://github.com/CiscoCXSecurity/enum4linux.git
> ```

**Usage:**
```bash
enum4linux -a target.com
```

---

### Utilities

#### anew
**Install:**
```bash
go install -v github.com/tomnomnom/anew@latest
```
**Usage:**
```bash
# Append unique lines from new results into an existing file
cat new-results.txt | anew existing-results.txt
```

---

### Online Recon Tools

| Tool | URL | Purpose |
|------|-----|---------|
| Wappalyzer | wappalyzer.com | Tech stack detection |
| WhatWeb | whatweb.net | Web fingerprinting |
| Web-Check | web-check.xyz | Website analysis |
| DNSDumpster | dnsdumpster.com | DNS recon & mapping |
| CriminalIP | search.criminalip.io | IP threat intelligence |
| HaveIBeenPwned | haveibeenpwned.com | Breach data check |
| HaveIBeenSquatted | haveibeensquatted.com | Typosquat detection |
| LeakRadar | members.leakradar.io | Leaked data search |
| ProxyNova | api.proxynova.com | Combo/credential search |
