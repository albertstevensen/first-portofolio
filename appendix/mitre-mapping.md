# MITRE ATT&CK Mapping

Dokumen ini berisi pemetaan aktivitas penetration testing terhadap framework MITRE ATT&CK untuk mengidentifikasi attacker behavior, attack technique, serta tahapan serangan yang relevan selama proses assessment terhadap OWASP Juice Shop.

Mapping dilakukan untuk memberikan gambaran bagaimana aktivitas reconnaissance, enumeration, API analysis, access control testing, hingga privilege escalation dapat dikaitkan dengan teknik dan tactic yang umum digunakan pada real-world attack scenario.

Framework MITRE ATT&CK digunakan secara luas dalam industri cyber security untuk:
- threat modeling
- attacker behavior analysis
- detection engineering
- security monitoring
- incident response
- security assessment standardization

Dengan melakukan mapping terhadap MITRE ATT&CK, hasil assessment menjadi lebih terstruktur, mudah dipahami, dan memiliki alignment terhadap industry-standard security framework.

---

# MITRE ATT&CK Mapping Table

| Assessment Activity | MITRE ATT&CK Tactic | Technique ID | Technique Name | Assessment Relevance |
|---|---|---|---|---|
| Service Enumeration menggunakan Nmap | Reconnaissance | TA0043 | Reconnaissance | Attacker melakukan identifikasi service dan port terbuka untuk memahami target environment |
| Directory Enumeration menggunakan Gobuster | Discovery | TA0007 | Discovery | Attacker melakukan endpoint discovery untuk mengidentifikasi hidden path dan attack surface |
| REST API Discovery | Discovery | TA0007 | Discovery | API endpoint ditemukan dan dianalisis sebagai backend communication surface |
| Authentication Testing | Credential Access | T1078 | Valid Accounts | Penggunaan akun valid untuk melakukan authentication dan access control testing |
| Session Analysis | Credential Access | T1078 | Valid Accounts | Authentication token dan session mechanism dianalisis untuk memahami session handling behavior |
| Access Control Testing | Privilege Escalation | TA0004 | Privilege Escalation | Pengujian authorization dilakukan untuk mengevaluasi kemungkinan unauthorized functionality access |
| Administrative Endpoint Testing | Privilege Escalation | TA0004 | Privilege Escalation | Endpoint administratif diuji menggunakan akun privilege rendah |
| API Request Manipulation | Exploitation | T1068 | Exploitation for Privilege Escalation | Request API dianalisis dan dimanipulasi untuk menguji authorization validation |
| Post-Exploitation Analysis | Discovery | TA0007 | Discovery | Session, API response, dan backend behavior dianalisis setelah authentication berhasil dilakukan |

---

# MITRE ATT&CK Analysis

## Reconnaissance Phase

Tahap reconnaissance dilakukan menggunakan tools seperti Nmap dan WhatWeb untuk mengidentifikasi:
- service aktif
- open port
- web technology
- backend architecture

Aktivitas ini termasuk dalam kategori:
```text
TA0043 — Reconnaissance
```

karena attacker mengumpulkan informasi awal mengenai target environment sebelum melakukan eksploitasi lebih lanjut.

---

## Discovery Phase

Setelah target berhasil diidentifikasi, attacker melakukan endpoint discovery menggunakan Gobuster dan API analysis untuk memahami:
- hidden endpoint
- REST API structure
- backend communication
- administrative functionality

Aktivitas tersebut dipetakan ke:
```text
TA0007 — Discovery
```

karena bertujuan memahami struktur internal aplikasi dan memperluas attack surface visibility.

---

## Credential & Session Analysis

Authentication testing dan session analysis dilakukan menggunakan akun valid untuk memahami:
- authentication mechanism
- session handling
- token storage
- authorization behavior

Aktivitas ini relevan dengan:
```text
T1078 — Valid Accounts
```

karena attacker menggunakan credential atau authenticated session untuk melakukan pengujian lanjutan terhadap aplikasi.

---

## Privilege Escalation Analysis

Pengujian access control dan administrative endpoint dilakukan untuk mengevaluasi kemungkinan:
- broken access control
- weak authorization validation
- unauthorized functionality access

Aktivitas tersebut dipetakan ke:
```text
TA0004 — Privilege Escalation
```

karena attacker mencoba memperoleh akses terhadap functionality yang seharusnya hanya tersedia untuk privileged user.

---

## Exploitation Analysis

API manipulation dan backend request analysis dilakukan untuk memahami bagaimana aplikasi memvalidasi request terhadap endpoint tertentu.

Aktivitas ini berkaitan dengan:
```text
T1068 — Exploitation for Privilege Escalation
```

karena attacker melakukan eksploitasi terhadap authorization weakness pada level aplikasi.

---

# MITRE ATT&CK Summary

Berdasarkan hasil assessment, aktivitas penetration testing menunjukkan bahwa modern web application seperti OWASP Juice Shop memiliki attack surface utama pada:
- REST API communication
- authentication mechanism
- session handling
- authorization validation
- administrative functionality

Mapping terhadap MITRE ATT&CK membantu menggambarkan bagaimana attacker dapat bergerak secara sistematis dari reconnaissance hingga privilege escalation melalui kombinasi:
- endpoint discovery
- API analysis
- authentication testing
- authorization testing
- backend communication analysis

Pendekatan ini membantu meningkatkan kualitas technical reporting serta memberikan alignment terhadap industry-standard threat modeling framework yang umum digunakan dalam cyber security assessment dan security operation.
