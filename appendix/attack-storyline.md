# Attack Storyline

Dokumen ini menjelaskan attack flow dan attacker movement selama proses penetration testing terhadap OWASP Juice Shop. Attack storyline digunakan untuk menggambarkan bagaimana seorang attacker dapat bergerak secara sistematis dari tahap reconnaissance hingga post-exploitation analysis menggunakan kombinasi service enumeration, endpoint discovery, API analysis, dan authorization testing.

Pendekatan attack storyline membantu memvisualisasikan:
- attacker behavior
- attack progression
- attack surface utilization
- privilege escalation path
- potential business impact

Assessment dilakukan pada environment lab terisolasi menggunakan OWASP Juice Shop yang dijalankan melalui Docker pada Kali Linux.

---

# Attack Scenario Overview

Pada skenario ini attacker diasumsikan sebagai external user tanpa privilege awal terhadap aplikasi.

Target utama assessment meliputi:
- web application endpoint
- REST API communication
- authentication mechanism
- session handling
- administrative functionality
- authorization validation

Tujuan utama attacker adalah:
- memahami struktur aplikasi
- mengidentifikasi attack surface
- melakukan access control testing
- mengevaluasi kemungkinan privilege escalation pada level aplikasi

---

# Phase 1 — Reconnaissance

Tahap awal dimulai dengan reconnaissance untuk mengidentifikasi target environment dan service yang berjalan pada aplikasi.

## Activity

- Host identification
- Service enumeration
- Web fingerprinting
- HTTP header analysis

## Tools

- Nmap
- WhatWeb
- Curl

## Objective

Mengidentifikasi:
- open port
- active service
- web technology
- backend architecture
- security header
- initial attack surface

## Result

Port `3000/tcp` berhasil diidentifikasi dalam keadaan terbuka dan menjalankan service HTTP yang mengarah ke aplikasi OWASP Juice Shop.

Fingerprinting juga menunjukkan bahwa aplikasi menggunakan:
- HTML5 architecture
- JavaScript module
- REST/API communication
- Single Page Application (SPA)

---

# Phase 2 — Attack Surface Discovery

Setelah target berhasil diidentifikasi, attacker melakukan endpoint enumeration untuk mencari hidden path dan backend communication endpoint.

## Activity

- Directory enumeration
- Endpoint discovery
- REST API discovery
- Public file access identification

## Tools

- Gobuster

## Objective

Mengidentifikasi:
- hidden endpoint
- administrative route
- API endpoint
- public file access
- restricted functionality

## Result

Enumeration berhasil menemukan beberapa endpoint penting seperti:

```text
/api
/rest
/ftp
/restricted
/administration
```

Endpoint tersebut menunjukkan keberadaan:
- REST API architecture
- administrative functionality
- public file access
- restricted endpoint
- backend communication surface

Administrative endpoint kemudian menjadi high-value target untuk authorization testing.

---

# Phase 3 — Authentication & Session Analysis

Pada tahap ini attacker mulai menganalisis bagaimana aplikasi menangani authentication request dan session persistence.

## Activity

- Login analysis
- Authentication request inspection
- Authentication response analysis
- Session storage analysis

## Tools

- Browser Developer Tools
- Burp Suite

## Objective

Memahami:
- authentication flow
- session mechanism
- token handling
- API communication
- authorization behavior

## Result

Assessment menunjukkan bahwa aplikasi menggunakan token/session-based authentication mechanism dengan token yang disimpan pada browser storage setelah login berhasil dilakukan.

Authentication request dilakukan melalui endpoint:

```text
POST /rest/user/login
```

sedangkan session persistence dilakukan menggunakan authentication token pada sisi client.

---

# Phase 4 — API Analysis & Backend Communication

Setelah memahami authentication mechanism, attacker mulai menganalisis komunikasi antara frontend dan backend API.

## Activity

- API request inspection
- REST API analysis
- Backend response analysis
- Parameter observation

## Tools

- Browser Developer Tools
- Burp Suite

## Objective

Mengidentifikasi:
- API structure
- backend communication flow
- request/response behavior
- sensitive endpoint
- authorization validation

## Result

Assessment menunjukkan bahwa aplikasi heavily dependent terhadap REST API communication untuk:
- authentication
- product management
- user interaction
- administrative functionality

REST API kemudian menjadi salah satu attack surface utama selama proses penetration testing.

---

# Phase 5 — Access Control Testing

Pada tahap ini attacker mulai melakukan pengujian terhadap authorization mechanism aplikasi menggunakan akun privilege rendah.

## Activity

- Administrative endpoint testing
- Restricted endpoint access
- Authorization testing
- Backend communication analysis

## Target Endpoint

```text
/administration
/restricted
/profile
```

## Objective

Mengevaluasi:
- authorization validation consistency
- administrative endpoint exposure
- backend access control enforcement
- potential privilege escalation path

## Result

Administrative route tetap dapat diakses melalui frontend routing aplikasi dan menghasilkan backend communication request meskipun user tidak memiliki administrative privilege penuh.

Assessment menunjukkan bahwa administrative functionality merupakan high-value attack surface yang perlu mendapatkan backend authorization validation secara konsisten.

---

# Phase 6 — Application-Level Privilege Escalation

Setelah administrative endpoint berhasil diidentifikasi, attacker mulai mengevaluasi kemungkinan privilege escalation pada level aplikasi.

## Activity

- Authorization manipulation
- Administrative functionality testing
- Backend request analysis
- Session analysis

## Objective

Mengidentifikasi kemungkinan:
- broken access control
- unauthorized functionality access
- privilege escalation
- weak authorization validation

## Result

Walaupun assessment tidak menunjukkan keberhasilan memperoleh hak akses administrator secara penuh, proses testing berhasil menunjukkan bahwa:
- administrative endpoint aktif
- backend communication tetap berjalan
- authorization validation menjadi area kritikal aplikasi

Authorization weakness pada modern web application dapat meningkatkan risiko:
- unauthorized administrative access
- sensitive functionality abuse
- data manipulation
- privilege escalation

---

# Phase 7 — Post-Exploitation Analysis

Tahap terakhir dilakukan untuk mengevaluasi dampak lebih lanjut apabila vulnerability berhasil dieksploitasi pada production environment.

## Activity

- Session analysis
- API response analysis
- Administrative endpoint review
- Sensitive information exposure analysis

## Objective

Mengidentifikasi:
- session persistence behavior
- backend response exposure
- potential information disclosure
- business impact

## Result

Post-exploitation analysis menunjukkan bahwa aplikasi memiliki beberapa area penting yang perlu diamankan lebih lanjut, khususnya pada:
- authorization validation
- REST API security
- session management
- administrative functionality exposure

---

# Attack Chain Visualization

```text
Reconnaissance
      |
      v
Service Enumeration
      |
      v
Technology Fingerprinting
      |
      v
Directory Enumeration
      |
      v
REST/API Discovery
      |
      v
Authentication Analysis
      |
      v
Session Analysis
      |
      v
Administrative Endpoint Discovery
      |
      v
Access Control Testing
      |
      v
Privilege Escalation Testing
      |
      v
Post-Exploitation Analysis
```

---

# Attack Storyline Summary

Assessment menunjukkan bagaimana attacker dapat bergerak secara sistematis dari reconnaissance hingga privilege escalation menggunakan kombinasi:
- service enumeration
- endpoint discovery
- REST API analysis
- authentication testing
- authorization testing
- backend communication analysis

Attack storyline ini menunjukkan bahwa modern web application memiliki attack surface yang luas pada:
- REST API communication
- frontend routing
- session handling
- authentication mechanism
- authorization validation
- administrative functionality

Pendekatan secure-by-design, backend authorization enforcement, serta security testing berkala sangat penting untuk mengurangi risiko exploitation dan privilege escalation pada modern web application.
