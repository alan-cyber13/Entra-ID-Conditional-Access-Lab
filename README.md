# Microsoft Entra ID: Conditional Access and MFA Lab

## Goal
Harden identity security in an Azure tenant by enforcing MFA and blocking legacy authentication with Conditional Access, then verify enforcement using sign-in logs.

## Tools
Microsoft Entra ID (P2 trial), Conditional Access, Microsoft Authenticator

## What I did
1. Created a test Azure tenant and a test user
2. Disabled Security Defaults so Conditional Access could be used
3. Created a policy requiring MFA for all users and all resources, excluding one admin account to avoid lockout
4. Created a policy blocking legacy authentication clients (Exchange ActiveSync and other legacy clients) for all users and all resources
5. Ran the policies in report-only mode first, then switched them on
6. Signed in as the test user and registered Microsoft Authenticator
7. Reviewed sign-in logs to confirm the policies behaved as expected

## Evidence

### Require MFA policy
screenshots/01-mfa-policy.png)

### Block legacy authentication policy
screenshots/02-legacy-auth-policy.png)

### Both policies enabled
screenshots/03-policy-overview.png)

### MFA challenge for the test user
screenshots/04-mfa-prompt.png)

### Conditional Access result in sign-in logs
Require MFA applied successfully. Block Legacy auth shows "Not applied" because the test sign-in used modern authentication, which is the expected result.

screenshots/05-conditional-access-result.png)

### Sign-in log history
The log shows the sign-in being interrupted by the MFA challenge, then succeeding once MFA was completed. IP addresses redacted.

screenshots/06-signin-logs.png)

## Why it matters
- MFA blocks most password-based attacks such as phishing and password spray
- Legacy authentication cannot do MFA, so attackers use it to bypass controls
- Conditional Access supports Zero Trust: verify explicitly and apply least privilege

## What I would add next
- Risk-based policies (sign-in risk and user risk)
- Privileged Identity Management (PIM) for admin roles
- Forwarding sign-in logs to Microsoft Sentinel for alerting
- Emergency "break glass" accounts excluded from policies and monitored

## What I learned
- Reviewing a policy's scope matters as much as creating it. My legacy auth policy initially targeted the wrong resources, so it would not have blocked anything until I checked the policy details and set it to all resources.
- Sign-in logs show exactly which Conditional Access policies applied. My test sign-in was interrupted by the MFA challenge and then succeeded, and the block policy showed "Not applied" because I used modern authentication, which confirmed each policy only triggers under its own conditions.
