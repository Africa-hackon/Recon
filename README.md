# Recon

A collection of reconnaissance and security testing tools used for bug bounty and penetration testing.

## Tools

### Prerequisites

#### Installing Go on Linux

1. Download the latest Go tarball from [https://go.dev/dl/](https://go.dev/dl/):
   ```bash
   wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
   ```

2. Remove any previous Go installation and extract the tarball:
   ```bash
   sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
   ```

3. Add Go to your PATH by appending to `~/.bashrc` or `~/.zshrc`:
   ```bash
   export PATH=$PATH:/usr/local/go/bin
   export PATH=$PATH:$(go env GOPATH)/bin
   ```

4. Apply the changes:
   ```bash
   source ~/.bashrc
   ```

5. Verify the installation:
   ```bash
   go version
   ```

---

### Subdomain Enumeration

| Tool | Install |
|------|---------|
| **subfinder** | `go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest` |
| **findomain** | [https://github.com/Findomain/Findomain](https://github.com/Findomain/Findomain) |
| **assetfinder** | `go get -u github.com/tomnomnom/assetfinder` |

---

### HTTP Probing & WAF Detection

| Tool | Install |
|------|---------|
| **httpx** | `go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest` |
| **wafw00f** | [https://github.com/enablesecurity/wafw00f](https://github.com/enablesecurity/wafw00f) |

---

### Vulnerability Scanning

| Tool | Install |
|------|---------|
| **nuclei** | `go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest` |
| **sqlmap** | `git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev` |
| **ghauri** | [https://github.com/r0oth3x49/ghauri](https://github.com/r0oth3x49/ghauri) |
| **wpscan** | `gem install wpscan` |
| **Metasploit** | [https://docs.metasploit.com/docs/using-metasploit/getting-started/nightly-installers.html#installing-metasploit-on-linux--macos](https://docs.metasploit.com/docs/using-metasploit/getting-started/nightly-installers.html#installing-metasploit-on-linux--macos) |

---

### URL & Archive Discovery

| Tool | Install |
|------|---------|
| **waybackurls** | `go install github.com/tomnomnom/waybackurls@latest` |

---

### Network & SMB Enumeration

| Tool | Install |
|------|---------|
| **smbclient-ng** | [https://github.com/p0dalirius/smbclient-ng](https://github.com/p0dalirius/smbclient-ng) |
| **enum4linux** | [https://github.com/CiscoCXSecurity/enum4linux](https://github.com/CiscoCXSecurity/enum4linux) |

---

### Utilities

| Tool | Install |
|------|---------|
| **anew** | `go install -v github.com/tomnomnom/anew@latest` |
