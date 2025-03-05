# email-plus-addressing

**Observation:**
**This vulnerability arises due to inconsistent email normalization between the authentication system and database checks. When users sign up for a trial, the application verifies whether an email has been used before. However, it fails to properly normalize email addresses, allowing variations with symbols such as "+" (e.g., user@example.com vs. user+1@example.com) to be treated as different users.**

1. We registered for a trial account using a business or student email.
2. When an already-used email was entered, the system rejected the request with a "trial was not approved" message.
3. We then modified the same email by appending a "+" alias (e.g., user+1@example.com) and completed the registration process.
4. The system accepted the modified email and sent a confirmation email.
5. Upon verifying the email, we successfully gained access to the trial.
6. Using the provided credentials, we logged into the web application and downloaded the trial license key.
7. We tested the key and confirmed that it unlocked the full pro version of the software.



**Root Cause Analysis:**
- The email verification system normalizes email addresses inconsistently between the authentication process and the database check.
- While the email server correctly identifies aliases (e.g., user+1@example.com as user@example.com), the application treats them as unique accounts.
- This inconsistency allows users to circumvent trial restrictions by manipulating email addresses, creating multiple trial accounts under a single email identity.

**Impact:**
The vulnerability has multiple security and financial implications, including:
- Financial Loss: Attackers can continuously access premium services for free, leading to revenue loss as legitimate customers may opt to abuse the trial instead of purchasing a valid license.
- Unauthorised Redistribution: Since the trial keys can be obtained multiple times, an attacker may distribute or resell the keys on third-party platforms, further impacting the organisation’s sales.
- Reputation Damage: If trial keys are widely shared or sold on the black market, it could undermine the perceived value of the premium offering, eroding customer trust.
- Increased Operational Costs: Abuse of the trial system could result in increased server loads and unnecessary customer support inquiries, affecting business operations.

**Recommended:**
To mitigate this issue, we strongly recommend implementing strict email validation to prevent aliasing techniques such as adding +1 to existing emails. Normalising email inputs before processing registrations can effectively block this method of abuse.
- Enforce strict email normalization before storing and verifying emails, ensuring that aliases (such as + tags) are removed before checking for duplicates.
- Use secondary verification methods such as OTP, CAPTCHA, or payment details to prevent abuse.
- Monitor for abnormal registration behavior using logging and rate-limiting mechanisms.
