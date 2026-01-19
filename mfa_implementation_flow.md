**Multi-Factor Authentication (MFA) Implementation Flow**

This document details the secure user experience and backend logic flow for deploying MFA using Firebase Authentication. MFA was required for all administrative and moderation accounts accessing sensitive configuration data.

**Secure User Flow**

The process ensures the identity of an administrator is verified by two factors before granting access:

1. Initial Login Attempt: User enters email and password into the React frontend.

2. Firebase Auth Check: Firebase confirms credentials.

3. MFA Requirement Check: The backend (Flask/Cloud Function) checks the user's role metadata (role: 'admin').

4. MFA Trigger: If the user is an admin, the system initiates the second factor (SMS verification) via Firebase Identity Platform.

5. Verification Prompt: The frontend displays a prompt for the user to enter the 6-digit SMS code.

6. Token Issuance: Upon successful verification of the code, Firebase issues a JWT (JSON Web Token) with the administrative claims, granting access to protected API routes.

**Technical Components & Logic**

- The implementation focuses on hardening authentication and authorization checks:

- Identity Provider: Firebase Authentication handles core user management and robust SMS delivery and verification.

- Authorization Guard: The backend ensures that critical API routes (e.g., config modification) are inaccessible unless the user's JWT contains the validated MFA claim.

- Security Enhancement: This process prevents session takeover and credential stuffing attacks against privileged accounts by requiring a rotating, physical token (the SMS code).

- Data Protection: User roles are enforced by checking claims within the JWT against both Firebase Security Rules and backend route guards.
