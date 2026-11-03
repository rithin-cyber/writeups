# Day 3 — Broken Object Level Authorization (BOLA)

**Date:** November 3, 2026  
**Target:** OWASP Juice Shop — Local Docker Lab  
**Endpoint:** `GET /rest/basket/{basketId}`  
**Vulnerability:** Broken Object Level Authorization (BOLA / IDOR)  
**OWASP Classification:** API1:2023  
**Finding Status:** Demonstrated in an authorized local training environment

## 1. Executive Summary

During an authorization assessment of OWASP Juice Shop, I observed that Account A could retrieve shopping basket data belonging to Account B by modifying the basket identifier in an API request.

The application returned HTTP 200 and Account B's basket information instead of denying unauthorized access.

## 2. Test Environment

- Application: OWASP Juice Shop
- Target: `http://127.0.0.1:3000`
- Environment: Local Docker container
- Tools: Burp Suite
- Accounts: Two tester-controlled accounts

## 3. Steps to Reproduce

1. Create two independent test accounts: Account A and Account B.
2. Add Apple Juice to Account A's basket.
3. Add Pineapple Juice to Account B's basket.
4. Log in as Account A.
5. Capture Account A's basket request using Burp Suite.
6. Send the request to Burp Repeater.
7. Change the basket ID from `6` to `7`, keeping Account A's authentication unchanged.
8. Send the modified request.
9. Observe the HTTP response.

## 4. Evidence

**Account A basket identifier:** `6`

**Account B basket identifier:** `7`

Modified request:

```http
GET /rest/basket/7 HTTP/1.1
Host: 127.0.0.1:3000
Authorization: Bearer [REDACTED]
```

**Observed HTTP status:** `200 OK`

**Observed result:** Account B's basket data was returned to Account A.

The account-specific response was reported during the local lab test. Preserve a redacted Burp screenshot for the supporting evidence.

## 5. Expected vs Actual Behavior

**Expected:** The server should reject Account A's request for Account B's private basket unless Account A has explicit permission.

**Actual:** The server returned Account B's basket information.

## 6. Security Impact

An authenticated user may access another user's private shopping basket without authorization.

In a production shopping application, this could expose another customer's selected products and shopping behavior.

No ability to modify another user's basket was tested or established.

## 7. Root Cause

The behavior is consistent with missing or ineffective server-side object-level authorization for the basket retrieval endpoint.

The backend appears to accept a client-supplied basket identifier without adequately enforcing the requesting user's access rights.

## 8. Remediation

1. Require appropriate authentication for private basket operations.
2. Validate whether the authenticated account is authorized to access the requested basket.
3. Enforce authorization at the backend for every basket operation.
4. Return an appropriate access-denied response when the user lacks permission.
5. Add automated tests covering attempts by one user to access another user's basket.
6. Do not rely on unpredictable identifiers or frontend restrictions as substitutes for authorization.

## 9. Testing Limitations

- Only tester-controlled accounts were involved.
- Only read access was demonstrated.
- No modification, deletion, purchasing, or data exfiltration was attempted.
- No production systems were tested.

## 10. What I Learned

- Authentication and authorization are different security controls.
- Changing an object identifier can reveal missing authorization checks.
- HTTP 200 alone does not prove a vulnerability.
- Comparing two controlled user accounts provides stronger evidence.
- Proper vulnerability reports must include reproduction steps, impact and remediation.
