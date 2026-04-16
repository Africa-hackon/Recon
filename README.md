# Recon

A collection of reconnaissance and security testing tools used for bug bounty and penetration testing.

## Tools

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
```bash
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```

#### findomain
```bash
curl -LO https://github.com/Findomain/Findomain/releases/latest/download/findomain-linux
chmod +x findomain-linux
sudo mv findomain-linux /usr/local/bin/findomain
```

#### assetfinder
```bash
go get -u github.com/tomnomnom/assetfinder
```

---

### HTTP Probing & WAF Detection

#### httpx
```bash
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
```

#### wafw00f
```bash
pip install wafw00f
```

---

### Vulnerability Scanning

#### nuclei
```bash
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
```

#### sqlmap
```bash
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
cd sqlmap-dev
python3 sqlmap.py --version
```

#### ghauri
```bash
git clone https://github.com/r0oth3x49/ghauri.git
cd ghauri
pip install -r requirements.txt
python3 ghauri.py --version
```

#### wpscan
```bash
gem install wpscan
```
> Requires Ruby. Install with: `sudo apt install ruby ruby-dev`

#### Metasploit
```bash
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall
chmod +x msfinstall
sudo ./msfinstall
```

---

### URL & Archive Discovery

#### waybackurls
```bash
go install github.com/tomnomnom/waybackurls@latest
```

---

### Network & SMB Enumeration

#### smbclient-ng
```bash
pip install smbclientng
```

#### enum4linux
```bash
sudo apt install enum4linux
```
> Or clone manually:
> ```bash
> git clone https://github.com/CiscoCXSecurity/enum4linux.git
> ```

---

### Utilities

#### anew
```bash
go install -v github.com/tomnomnom/anew@latest
```
