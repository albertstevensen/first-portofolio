# Portofolio Penetration Testing — OWASP Juice Shop

Web Application Penetration Testing Lab menggunakan Kali Linux, Docker, metodologi OWASP Top 10, dan MITRE ATT&CK Mapping.

---

# Disclaimer

Project ini dilakukan pada environment lab terisolasi menggunakan OWASP Juice Shop untuk tujuan edukasi dan pengembangan portofolio cyber security. Tidak ada pengujian terhadap sistem produksi atau infrastruktur pihak ketiga tanpa izin.

---

# Informasi Project

| Informasi | Detail |
|---|---|
| Project | OWASP Juice Shop Security Assessment |
| Tester | Albert Stevensen |
| Environment | Kali Linux + Docker |
| Jenis Assessment | Web Application Penetration Testing |
| Metodologi | OWASP Top 10 & MITRE ATT&CK |
| Tools | Nmap, WhatWeb, Gobuster, Burp Suite, Browser DevTools |
| Scope | Local Dockerized Lab Environment |

---

# Objectives

| No | Objective |
|---|---|
| 1 | Melakukan reconnaissance dan service enumeration terhadap aplikasi web |
| 2 | Mengidentifikasi attack surface dan hidden endpoint |
| 3 | Menganalisis REST API dan session handling |
| 4 | Melakukan access control testing dan privilege escalation pada level aplikasi |
| 5 | Mendokumentasikan hasil pengujian menggunakan OWASP Top 10 dan MITRE ATT&CK |
| 6 | Membuat portofolio penetration testing dengan format professional reporting |

---

# Repository Structure

```text
cybersecurity-portfolio/
├── README.md
│
├── evidence/
│   ├── 01-docker-running.png
│   ├── 02-juice-shop-homepage.png
│   ├── 03-nmap-scan.png
│   ├── 04-whatweb-result.png
│   ├── 05-gobuster-result.png
│   ├── 06-login-page.png
│   ├── 07-login-request.png
│   ├── 08-login-response.png
│   ├── 09-session-storage.png
│   ├── 10-api-testing.png
│   ├── 11-access-control-testing.png
│   └── 12-privilege-escalation-testing.png
│
├── logs/
│   ├── nmap.txt
│   ├── gobuster.txt
│   ├── whatweb.txt
│   ├── curl-header.txt
│   └── api-testing.txt
│
└── appendix/
    ├── mitre-mapping.md
    ├── owasp-mapping.md
    ├── attack-storyline.md
    └── attack-surface-analysis.md
```

---

# Environment Setup

| Step | Activity | Command |
|---|---|---|
| 1 | Install Docker | `sudo apt update && sudo apt install docker.io -y` |
| 2 | Menjalankan Docker Service | `sudo systemctl enable docker && sudo systemctl start docker` |
| 3 | Pull Docker Image | `sudo docker pull bkimminich/juice-shop` |
| 4 | Menjalankan Container | `sudo docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop` |
| 5 | Verifikasi Container | `docker ps` |

---

# Environment Result

| Item | Result |
|---|---|
| Running Port | `0.0.0.0:3000` |
| Application URL | `http://127.0.0.1:3000` |
| Status | Running Successfully |

---

# Environment Evidence

| Evidence | Screenshot |
|---|---|
| Docker Container Running | ![Docker Running](evidence/01-docker-running.png) |
| Juice Shop Homepage | ![Juice Shop Homepage](evidence/02-juice-shop-homepage.png) |

---

# Reconnaissance & Enumeration

## Host Identification

| Item | Detail |
|---|---|
| Objective | Mengidentifikasi host environment |
| Command | `ip a` |
| Analysis | Host Kali Linux menggunakan interface aktif yang memungkinkan komunikasi dengan Docker container lokal |

---

## Service Enumeration

| Item | Detail |
|---|---|
| Objective | Mengidentifikasi service yang berjalan pada target |
| Command | `nmap -sV -p 3000 127.0.0.1` |
| Result | `3000/tcp open http` |
| Evidence | ![Nmap Scan](evidence/03-nmap-scan.png) |
| Analysis | Port 3000 ditemukan dalam keadaan terbuka dan menjalankan service HTTP sehingga menjadi entry point utama untuk enumeration dan attack surface analysis |

---

## Web Fingerprinting

| Item | Detail |
|---|---|
| Objective | Mengidentifikasi teknologi web dan security header |
| Command | `whatweb http://127.0.0.1:3000` |
| Evidence | ![WhatWeb Result](evidence/04-whatweb-result.png) |

### Result

```text
http://127.0.0.1:3000 [200 OK] Country[RESERVED][ZZ], HTML5, IP[127.0.0.1], Script[module], Title[OWASP Juice Shop], UncommonHeaders[access-control-allow-origin,x-content-type-options,feature-policy,x-recruiting], X-Frame-Options[SAMEORIGIN]
```

### Analysis

| Finding | Analysis |
|---|---|
| HTML5 & Script[module] | Menunjukkan aplikasi menggunakan modern JavaScript-based architecture dan kemungkinan SPA |
| X-Frame-Options | Membantu mengurangi risiko clickjacking |
| X-Content-Type-Options | Membantu mencegah MIME sniffing |
| Feature-Policy | Membatasi fitur browser tertentu |
| access-control-allow-origin | Menunjukkan penggunaan CORS yang perlu dianalisis lebih lanjut |
| SPA Behavior | Invalid route tetap menghasilkan `HTTP 200 OK` sehingga enumeration membutuhkan filtering response length |

---

## HTTP Header Enumeration

| Item | Detail |
|---|---|
| Command | `curl -I http://127.0.0.1:3000` |
| Objective | Menganalisis HTTP response header |
| Analysis | Header digunakan untuk mengidentifikasi server behavior, security header, backend response, dan kemungkinan information disclosure |

---

## Directory & Endpoint Enumeration

| Item | Detail |
|---|---|
| Tool | Gobuster |
| Objective | Mengidentifikasi hidden endpoint dan public attack surface |

### Initial Command

```bash
gobuster dir -u http://127.0.0.1:3000 -w /usr/share/wordlists/dirb/common.txt
```

### Initial Result

```text
the server returns a status code that matches the provided options for non existing urls
```

### Revised Command

```bash
gobuster dir -u http://127.0.0.1:3000 -w /usr/share/wordlists/dirb/common.txt --exclude-length 9903
```

### Evidence

![Gobuster Result](evidence/05-gobuster-result.png)

### Enumeration Result

| Endpoint | Status | Security Relevance |
|---|---|---|
| `/ftp` | 200 | Public file access |
| `/robots.txt` | 200 | Potential hidden path disclosure |
| `/assets` | 301 | Static file directory |
| `/media` | 301 | Media directory |
| `/api` | 500 | Potential backend API endpoint |
| `/rest` | 500 | REST API attack surface |
| `/profile` | 500 | User-related functionality |
| `/redirect` | 500 | Potential redirect handling |
| `/restricted` | 500 | Potential authorization testing target |
| `/video` | 200 | Public media endpoint |

### Analysis

| Finding | Analysis |
|---|---|
| SPA Behavior | Invalid route menghasilkan `HTTP 200 OK` sehingga perlu filtering response length |
| `/ftp` | Menunjukkan kemungkinan public file access |
| `/api` & `/rest` | Menjadi prioritas untuk API testing |
| `/restricted` | Menjadi target access control testing |
| HTTP 500 Response | Menunjukkan request mencapai backend application |

---

# Authentication & Session Analysis

## Login Analysis

| Evidence | Description |
|---|---|
| ![Login Page](evidence/06-login-page.png) | Halaman login aplikasi |
| ![Login Request](evidence/07-login-request.png) | Request authentication `POST /rest/user/login` |
| ![Login Response](evidence/08-login-response.png) | Response authentication dan token |
| ![Session Storage](evidence/09-session-storage.png) | Authentication token pada browser storage |

### Authentication Analysis

| Finding | Analysis |
|---|---|
| Login Endpoint | Aplikasi menggunakan REST API authentication |
| Token-Based Authentication | Backend mengirim authentication token setelah login berhasil |
| Session Storage | Token tersimpan pada sisi client/browser |
| Security Risk | Weak session management dapat meningkatkan risiko session hijacking dan session replay |

---

# API Testing

| Item | Detail |
|---|---|
| Evidence | ![API Testing](evidence/10-api-testing.png) |
| Objective | Menganalisis komunikasi REST API |

### API Analysis

| Finding | Analysis |
|---|---|
| REST API Architecture | Aplikasi menggunakan komunikasi berbasis REST API |
| Attack Surface | API dapat digunakan untuk authorization testing dan parameter manipulation |
| Information Disclosure | API response dapat membantu reconnaissance lanjutan |

---

# Access Control Testing

| Item | Detail |
|---|---|
| Evidence | ![Access Control Testing](evidence/11-access-control-testing.png) |
| Objective | Menguji authorization validation menggunakan akun privilege rendah |

### Access Control Analysis

| Finding | Analysis |
|---|---|
| Administrative Route Exposure | Route administratif tetap dapat dijangkau melalui frontend |
| Backend Communication | Endpoint administratif tetap melakukan backend request |
| Security Risk | Administrative endpoint menjadi high-value attack surface |

---

# Application-Level Privilege Escalation Testing

| Item | Detail |
|---|---|
| Evidence | ![Privilege Escalation Testing](evidence/12-privilege-escalation-testing.png) |
| Objective | Menguji kemungkinan privilege escalation pada level aplikasi |

### Privilege Escalation Analysis

| Finding | Analysis |
|---|---|
| Authorization Testing | Pengujian dilakukan terhadap endpoint administratif |
| Administrative Exposure | Endpoint administratif tetap dapat diakses pada frontend |
| Security Risk | Weak authorization validation dapat meningkatkan risiko privilege escalation |

---

# Visual Attack Surface

```text
                    +----------------------+
                    |     External User    |
                    |      / Attacker      |
                    +----------+-----------+
                               |
                               v
                  +-------------------------+
                  | Browser / Burp Suite    |
                  +-----------+-------------+
                              |
                              v
                  +-------------------------+
                  | OWASP Juice Shop        |
                  | Docker Container        |
                  +-----------+-------------+
                              |
        -------------------------------------------------
        |               |               |               |
        v               v               v               v

+---------------+ +---------------+ +---------------+ +----------------+
| Authentication| | REST API      | | Admin Endpoint| | File Endpoint  |
| & Session     | | /api /rest    | | /administration| | /ftp           |
+---------------+ +---------------+ +---------------+ +----------------+
```

### Attack Surface Analysis

| Attack Surface | Description |
|---|---|
| REST API | Digunakan untuk komunikasi backend |
| Authentication & Session | Menangani login dan session management |
| Administrative Endpoint | Menjadi target authorization testing |
| Public File Access | Potensi information disclosure |

---

# Post-Exploitation Analysis

### Post-Exploitation Findings

| Finding | Analysis |
|---|---|
| Session Storage | Authentication token tersimpan pada browser |
| API Response | REST API digunakan untuk authentication dan functionality aplikasi |
| Administrative Endpoint | Endpoint administratif tetap aktif pada backend communication |
| Security Impact | Potensi unauthorized access dan privilege escalation |

### Potential Security Impact

| Risk | Impact |
|---|---|
| Unauthorized Administrative Access | Access terhadap privileged functionality |
| Sensitive Information Exposure | Exposure terhadap informasi internal |
| Session Abuse | Session replay atau hijacking |
| Privilege Escalation | Unauthorized functionality access |

---

# Vulnerability Assessment

| Vulnerability | Severity | OWASP Mapping | Endpoint / Area | Potential Impact |
|---|---|---|---|---|
| Broken Access Control | High | A01 | `/restricted`, `/administration` | Privilege escalation |
| Information Disclosure | Medium | A05 | `/ftp`, `/robots.txt` | Sensitive information exposure |
| REST API Exposure | Medium | A05 | `/api`, `/rest` | API abuse |
| Session Handling Weakness | Medium | A07 | Authentication & Session | Session hijacking |
| Static File Exposure | Medium | A05 | `/assets`, `/media` | Frontend reconnaissance |
| Backend Error Exposure | Low | A05 | `/api`, `/rest` | Backend behavior disclosure |

---

# MITRE ATT&CK Mapping

| Aktivitas | Technique |
|---|---|
| Reconnaissance | TA0043 |
| Discovery | TA0007 |
| Privilege Escalation | TA0004 |
| Valid Accounts | T1078 |
| Exploitation | T1068 |

---

# Business Impact Analysis

| Risk | Business Impact |
|---|---|
| Unauthorized Administrative Access | Access terhadap privileged functionality |
| Sensitive Information Exposure | Kebocoran data internal |
| API Abuse | Penyalahgunaan backend API |
| Data Manipulation | Perubahan data tanpa otorisasi |
| Compliance Risk | Pelanggaran compliance/security policy |
| Reputational Damage | Penurunan reputasi organisasi |

---

# Recommendations & Remediation

| Risk Area | Identified Issue | Risk Level | Recommendation |
|---|---|---|---|
| Broken Access Control | Endpoint administratif dapat diakses melalui frontend route | High | Implementasi RBAC dan backend authorization validation |
| REST API Exposure | API endpoint dapat digunakan untuk enumeration | Medium | Implementasi authorization enforcement dan rate limiting |
| Session Handling Weakness | Session/token disimpan pada sisi client | Medium | Secure cookie, token expiration, dan secure session management |
| Information Disclosure | Public endpoint memberikan informasi tambahan | Medium | Hardening public endpoint dan file exposure |
| Backend Error Exposure | Endpoint menghasilkan `HTTP 500` | Low | Implementasi custom error handling |
| Static File Exposure | Static files membantu reconnaissance | Medium | Minimalkan informasi sensitif pada static file |

---

# Conclusion

| Finding | Summary |
|---|---|
| Reconnaissance | Service enumeration berhasil mengidentifikasi web application pada port 3000 |
| Enumeration | Hidden endpoint dan REST API berhasil ditemukan |
| Authentication Analysis | Aplikasi menggunakan token/session-based authentication |
| API Testing | REST API menjadi attack surface utama |
| Access Control Testing | Administrative endpoint menjadi high-value target |
| Privilege Escalation Testing | Authorization validation perlu diperkuat |
| Overall Security Posture | Modern web application membutuhkan secure authorization, API security, dan proper session management |
