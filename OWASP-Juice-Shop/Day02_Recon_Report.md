# Day 2 — Web Application Reconnaissance Report

**Assessment Date:** November 2, 2026  
**Target:** http://127.0.0.1:3000  
**Environment:** Local OWASP Juice Shop Docker container  
**Tools:** Nmap 7.991, curl, Burp Suite  
**Assessment Type:** Authorized local-lab reconnaissance

## 1. Executive Summary

Performed initial network and web application reconnaissance against a locally hosted OWASP Juice Shop training application.

The assessment identified an accessible HTTP service, recorded response headers, and mapped five HTTP requests associated with application and API functionality.

**Confirmed security vulnerabilities: None.** Further testing would be required to establish whether any identified behavior creates a security risk.

## 2. Scope

In scope:
- `127.0.0.1`
- TCP port `3000`
- Local Juice Shop web application and its APIs

Out of scope:
- Public websites
- Other hosts or services
- Third-party systems

## 3. Network Reconnaissance

Command executed:

`nmap -sV -p 3000 127.0.0.1 -oN nmap-results.txt`

### Results

- Host: `127.0.0.1`
- TCP port: `3000`
- Port state: Open
- Nmap service label: `ppp?` (uncertain)
- Service fingerprint: HTTP responses identifying OWASP Juice Shop

Nmap did not confidently identify the service version. The HTTP response and HTML title provide additional evidence that the application is Juice Shop.

## 4. HTTP Reconnaissance

Tools: curl and Burp Suite.

Observed response headers included:

- `Access-Control-Allow-Origin: *`
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: SAMEORIGIN`
- `Content-Type`
- `ETag`
- `X-Recruiting: /#/jobs`

These observations alone do not establish a vulnerability.

## 5. HTTP Request Mapping

| Method | Endpoint | Status | Purpose |
|---|---|---|---|
| GET | `/` | 200 | Homepage |
| GET | `/api/Challenges/?name=Score%20Board` | 304 | Challenge information |
| GET | `/api/Quantitys/` | 304 | Quantity-related information |
| GET | `/rest/products/1/reviews` | 200 | Product reviews |
| GET | `/rest/products/search?q=pineapple` | 200 | Product search |

The final request was verified with curl. Other API requests were captured using Burp Suite.

## 6. API Observations

The product-search API accepted the query parameter `q=pineapple` and returned JSON.

Observed product:
- Product ID: 47
- Name: Pineapple Juice (1000ml)
- Price: 2.99

The product-reviews endpoint returned a JSON response containing review-related information.

No unauthorized data access has been established.

## 7. Security Assessment

- **Confirmed vulnerabilities:** None
- **Authorization testing:** Not performed
- **Authentication testing:** Not performed
- **CORS exploitation:** Not demonstrated
- **Sensitive information exposure:** Not confirmed

The captured HTTP traffic provides a baseline for later controlled security testing.

## 8. Evidence

Files:
- `nmap-results.txt`
- `headers.txt`
- `homepage.html`
- `burp-request-map.md`

Additional Burp Suite screenshots were reviewed during the exercise.

## 9. Next Steps

1. Review application functionality and API relationships.
2. Identify which endpoints require authentication.
3. Practise access-control testing in the authorized lab.
4. Document reproducible evidence for any confirmed security finding.

## 10. What I Learned

- How to perform service discovery with Nmap.
- How to collect HTTP headers using curl.
- How to inspect application requests with Burp Suite.
- How HTTP 200 and HTTP 304 responses differ.
- How to identify API endpoints and parameters.
- Why reconnaissance findings must not be confused with confirmed vulnerabilities.
