# INC-003: Entra ID Account and Sign-In Troubleshooting

## Scenario

A user reports that they are unable to sign in to their Microsoft 365 account.

The objective was to investigate the authentication failure using Microsoft Entra ID rather than immediately resetting the password or making account changes.

## Investigation

The user's account was first reviewed in the Microsoft Entra admin center.

The investigation included checking:

- Account enabled or disabled status
- Recent sign-in activity
- Authentication failure details
- Group membership
- Assigned licences
- Directory roles

Microsoft Entra sign-in logs were used to investigate failed authentication attempts.

One failed sign-in returned:

```text
Error Code: 50126
```

This indicated that the authentication attempt failed because of an invalid username or password.

A separate lab scenario involved a disabled user account.

The user attempted to sign in and Microsoft Entra recorded:

```text
Error Code: 50057
```

The sign-in failure showed that the user account was disabled.

This allowed the cause of the authentication problem to be identified from evidence rather than making unnecessary account changes.

## Root Cause

Two different authentication failures were investigated during the lab:

```text
50126 - Invalid username or password
50057 - User account disabled
```

Although both incidents appeared to the user as sign-in problems, they had different root causes and therefore required different responses.

## Resolution

For the disabled-account scenario, the account status was reviewed and the simulated support request confirmed that the account had been disabled accidentally.

After authorization was confirmed, the user account was re-enabled.

For password-related support, the lab also demonstrated the appropriate password reset process using Microsoft Entra ID.

The user's identity should be verified before performing a password reset, and the user can be required to change the temporary password at the next sign-in.

Active sessions can also be revoked when appropriate:

```text
Revoke sessions
```

This forces existing authentication sessions to reauthenticate and can be useful after account or security-related changes.

## Verification

After the disabled account was restored, the user successfully authenticated again.

Microsoft Entra sign-in activity was reviewed to confirm successful authentication.

Audit logs were also used to verify administrative actions performed against the account.

This provided both technical verification and an administrative audit trail.

## Key Learning

A sign-in failure should be investigated before performing a password reset.

Microsoft Entra sign-in logs can provide specific evidence explaining why authentication failed.

The lab demonstrated the difference between:

```text
Authentication failure
Account disabled
Password issue
Licence or service entitlement issue
Authorization or permission issue
```

A useful Microsoft 365 user-access troubleshooting sequence is:

**Identify user → verify account status → review sign-in logs → identify error code → check groups and licences → make the smallest authorized change → verify sign-in → document the incident**

The incident also reinforced that technical ability to modify an account does not automatically provide business authorization to make that change.
