# SR-010: Exchange Online Shared Mailbox Troubleshooting

## Scenario

An HR team requires a shared mailbox that authorised staff can access and use to send email.

The objective was to configure the mailbox, assign appropriate permissions and troubleshoot access and sending behaviour using Microsoft 365 and Exchange Online.

## Configuration

A shared mailbox named **HR Support** was created in Microsoft 365.

Mailbox access was configured using Exchange Online permissions.

The permissions investigated during the lab included:

```text
Full Access
Send As
Send on Behalf
```

A licensed test user was used to verify the configuration end to end.

## Investigation

### Full Access

The user was assigned **Full Access** permission to the shared mailbox.

The mailbox did not immediately appear automatically in Outlook on the web.

Instead of removing and recreating permissions, the mailbox was tested manually using:

```text
Open another mailbox
```

The shared mailbox opened successfully.

This confirmed that Full Access permission was functioning and suggested that the original problem was related to automapping or permission propagation rather than a failure of mailbox access itself.

### Send As

The user was also assigned **Send As** permission.

A test message was sent from the shared mailbox.

The recipient received the message showing the sender as:

```text
HR Support
```

This verified that Send As permission was functioning.

### Permission Propagation

Send As permission was later removed as part of the troubleshooting exercise.

The change did not take effect immediately.

For a period of time, messages could still be sent using the previously effective permission.

Rather than repeatedly changing the configuration, the existing administrative state was reviewed and time was allowed for Exchange Online permission changes to propagate.

This demonstrated that the configuration visible in the administration portal and the behaviour experienced by the user may temporarily differ while cloud changes are propagating.

### Send on Behalf

**Send on Behalf** permission was then configured.

After propagation, a test message displayed the sender in the following form:

```text
User on behalf of HR Support
```

This confirmed that Send on Behalf was functioning and demonstrated the difference between the two sending permission models.

## Root Cause

The main access issue was not a complete permissions failure.

Full Access was working because the mailbox could be opened manually.

The mailbox's failure to appear automatically was therefore consistent with an automapping or propagation delay.

During the sending-permission tests, Exchange Online also required time for permission changes to become effective across the service.

## Resolution

The existing permissions were verified rather than repeatedly removed and recreated.

Manual mailbox access was used to confirm Full Access.

Test messages were used to verify Send As and Send on Behalf behaviour.

Where configuration changes had not yet propagated, the administrative state was confirmed and the service was allowed time to converge before retesting.

## Verification

The following behaviours were successfully verified during the lab:

```text
Full Access       → Shared mailbox could be opened
Send As           → Message appeared directly from HR Support
Send on Behalf    → Message showed the user on behalf of HR Support
```

At the end of the lab, temporary access permissions were removed and automatic replies used during testing were disabled.

## Key Learning

Shared mailbox troubleshooting requires understanding that different permissions provide different capabilities.

**Full Access** allows a user to open and manage mailbox content.

**Send As** allows a user to send a message that appears to come directly from the shared mailbox.

**Send on Behalf** identifies both the delegated user and the shared mailbox to the recipient.

The incident also demonstrated an important cloud-support principle:

**A configuration change being saved does not always mean that every Microsoft 365 service has applied the change immediately.**

When the configured administrative state is correct, unnecessary repeated changes can make troubleshooting more difficult.

A useful shared mailbox troubleshooting sequence is:

**Confirm user → verify licence/mailbox prerequisites → check Full Access → test manual mailbox access → check sending permissions → send test message → allow for propagation → verify behaviour → document and clean up**
