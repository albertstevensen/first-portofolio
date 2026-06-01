# OWASP Top 10 Mapping

Dokumen ini berisi pemetaan vulnerability dan security issue yang ditemukan selama proses penetration testing terhadap kategori OWASP Top 10 untuk membantu klasifikasi risiko, technical reporting, dan remediation planning.

OWASP Top 10 merupakan salah satu industry-standard framework yang digunakan secara luas untuk mengidentifikasi kategori risiko keamanan paling kritikal pada modern web application.

Mapping dilakukan berdasarkan:
- hasil reconnaissance
- endpoint enumeration
- API analysis
- authentication testing
- access control testing
- session analysis
- privilege escalation testing

Tujuan utama mapping ini adalah untuk memberikan alignment terhadap best practice application security assessment serta membantu proses risk prioritization berdasarkan kategori vulnerability yang relevan.

---

# OWASP Top 10 Mapping Table

| Assessment Finding | OWASP Category | Risk Level | Endpoint / Area | Security Relevance |
|---|---|---|---|---|
| Broken Access Control | A01 Broken Access Control | High | `/restricted`, `/administration` | Authorization validation berpotensi tidak diterapkan secara konsisten |
| Administrative Endpoint Exposure | A01 Broken Access Control | High | `/administration` | Endpoint administratif dapat dijangkau melalui frontend route |
| Session Handling Weakness | A07 Identification and Authentication Failures | Medium | Authentication & Session | Session/token disimpan pada sisi client |
| REST API Exposure | A05 Security Misconfiguration | Medium | `/api`, `/rest` | Backend API dapat digunakan untuk enumeration dan analysis |
| Information Disclosure | A05 Security Misconfiguration | Medium | `/ftp`, `/robots.txt` | Public endpoint memberikan informasi tambahan mengenai aplikasi |
| Static File Exposure | A05 Security Misconfiguration | Medium | `/assets`, `/media` | Static files membantu frontend reconnaissance |
| Backend Error Exposure | A05 Security Misconfiguration | Low | `/api`, `/rest` | Backend menghasilkan `HTTP 500 Internal Server Error` |
| Potential Redirect Handling Issue | A01 Broken Access Control | Low | `/redirect` | Endpoint redirect perlu dianalisis lebih lanjut |
| Authentication Token Exposure | A07 Identification and Authentication Failures | Medium | Browser Storage | Authentication token tersimpan pada browser storage |
| Authorization Weakness | A01 Broken Access Control | High | API & Administrative Endpoint | User privilege rendah dapat melakukan access control testing terhadap endpoint administratif |

---

# OWASP Top 10 Analysis

## A01 — Broken Access Control

Broken Access Control menjadi salah satu area risiko utama yang berhasil diidentifikasi selama assessment.

Pengujian dilakukan terhadap:
- restricted endpoint
- administrative functionality
- authorization validation
- backend API communication

Endpoint seperti:

```text
/restricted
/administration
```

menunjukkan bahwa administrative route tetap dapat diakses pada level frontend routing meskipun backend kemungkinan masih melakukan authorization validation.

Risiko utama dari broken access control meliputi:
- unauthorized functionality access
- privilege escalation
- sensitive functionality exposure
- unauthorized administrative access

Broken access control merupakan salah satu vulnerability paling kritikal pada modern web application karena berdampak langsung terhadap confidentiality, integrity, dan availability data.

---

## A05 — Security Misconfiguration

Assessment juga mengidentifikasi beberapa area yang berkaitan dengan security misconfiguration, khususnya pada:
- REST API exposure
- public file access
- backend error response
- static file exposure

Endpoint seperti:

```text
/api
/rest
/ftp
```

menunjukkan bahwa aplikasi memiliki beberapa attack surface yang dapat membantu attacker melakukan reconnaissance dan backend analysis.

Response seperti:

```text
HTTP 500 Internal Server Error
```

juga dapat memberikan informasi tambahan mengenai backend behavior apabila error handling tidak dikonfigurasi secara aman.

Selain itu, keberadaan static content directory dapat membantu attacker memahami:
- frontend structure
- API route
- backend communication flow

---

## A07 — Identification and Authentication Failures

Authentication dan session analysis menunjukkan bahwa aplikasi menggunakan token/session-based authentication mechanism dengan token yang tersimpan pada browser storage.

Area yang dianalisis meliputi:
- login request
- authentication response
- session persistence
- browser storage

Weak session management dapat meningkatkan risiko:
- session hijacking
- session replay
- unauthorized persistent access

Pengamanan session dan authentication mechanism sangat penting pada modern web application karena sebagian besar komunikasi aplikasi dilakukan melalui authenticated API request.

---

# OWASP Risk Summary

Berdasarkan hasil assessment, area risiko utama aplikasi berada pada:
- broken access control
- authorization validation
- session handling
- REST API exposure
- backend misconfiguration

Attack surface utama yang berhasil diidentifikasi meliputi:
- administrative endpoint
- REST API communication
- authentication mechanism
- browser session storage
- public file access

Assessment menunjukkan bahwa kombinasi:
- weak authorization validation
- exposed API endpoint
- insecure session handling

dapat meningkatkan risiko privilege escalation dan unauthorized functionality access pada modern web application.

---

# Security Assessment Conclusion

Mapping terhadap OWASP Top 10 membantu mengklasifikasikan vulnerability berdasarkan kategori risiko yang relevan dan memberikan alignment terhadap industry-standard application security framework.

Pendekatan ini membantu:
- meningkatkan kualitas technical reporting
- mempermudah vulnerability prioritization
- mendukung remediation planning
- memberikan struktur assessment yang lebih profesional

Selain itu, OWASP mapping juga membantu menunjukkan bagaimana modern web application memiliki attack surface yang kompleks pada:
- API communication
- frontend routing
- authentication flow
- authorization mechanism
- session management

sehingga secure-by-design implementation dan security testing berkala menjadi sangat penting untuk mengurangi risiko exploitation dan privilege escalation.
