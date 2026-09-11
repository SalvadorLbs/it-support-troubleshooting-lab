# SR-004: User Lifecycle (Joiners, Movers and Leavers) and Least Privilege

## Scenario

A series of Microsoft Entra ID support tasks were performed to practise user lifecycle management.

The objective was to manage user access appropriately as employees join the organisation, change roles or departments, and leave the organisation.

The lab also focused on least privilege, authorization and maintaining an audit trail for administrative changes.

## Joiner

A test user was created in Microsoft Entra ID to simulate a new employee.

Before assigning access, the user's account was reviewed for:

- Group membership
- Assigned licences
- Directory roles
- Required access

Group-based access was used where appropriate rather than assigning unnecessary individual permissions.

A user was also added to an authorised department group and the resulting membership change was verified.

The lab demonstrated how dynamic groups can automate access based on user attributes.

For example, an HR dynamic group used the following logic:

```text
user.department = HR
```

Users whose department attribute matched HR were automatically included in the group.

## Mover

A department change was simulated for an existing user.

The user's department attribute was changed from:

```text
HR
```

to:

```text
Finance
```

Because membership of the HR security group was dynamically controlled by the department attribute, the user was automatically removed from the HR dynamic group.

Another user whose department remained HR continued to be a member.

This demonstrated how identity attributes can be used to automatically adjust access when an employee changes role or department.

## Leaver

A user offboarding scenario was performed in Microsoft Entra ID.

The following actions were taken:

1. The user account was disabled.
2. Existing authentication sessions were revoked.
3. Group membership was reviewed.
4. Unnecessary group access was removed.
5. Directory roles and licence assignments were checked.
6. The account was verified as unable to sign in.
7. Administrative actions were reviewed through audit logs.

The account was not deleted.

Account deletion was treated as a separate action requiring appropriate authorization and consideration of organisational data-retention requirements.

## Least Privilege

A separate support account was used to practise delegated administration.

The account was assigned the:

```text
Helpdesk Administrator
```

role.

The delegated administrator could perform appropriate helpdesk activities, including resetting the password of a standard test user.

The user was required to change the temporary password at the next sign-in.

The delegated administrator did not receive Global Administrator privileges.

This demonstrated the principle of least privilege:

**Users and administrators should receive only the permissions required to perform their authorised tasks.**

After the administrative exercise was completed, the Helpdesk Administrator role was removed.

## Audit and Verification

Microsoft Entra audit logs were reviewed to verify administrative actions.

The logs provided evidence of activities such as:

```text
Account disablement
Group membership changes
Role assignment
Role removal
```

This demonstrated the importance of maintaining an audit trail for identity and access changes.

## Key Learning

User lifecycle management is not simply about creating and deleting accounts.

Access should change throughout the employee lifecycle:

**Joiner → provide authorised access**

**Mover → adjust access when responsibilities change**

**Leaver → block access and revoke active sessions**

Dynamic groups can help automate access changes when reliable identity attributes are maintained.

Least privilege reduces unnecessary administrative access and limits the impact of compromised or incorrectly used accounts.

The lab also reinforced an important support principle:

**Technical permission to perform an action does not automatically provide business authorization to perform it.**

A useful user lifecycle workflow is:

**Verify request → confirm authorization → review current access → make the minimum required change → verify the result → review audit evidence → document the action**
