# A proposal for labeling emails

This document is a proposal for the usage of an SMTP header to improve the user experience when handling certain types of email: one-time passcodes, password reset email, and receipts.

It is geared towards app and email client developers, to encourage them to support this header and provide some real-world feedback in order to improve the spec.

The original article introducing this idea is available here: [https://victormours.mataroa.blog/blog/an-idea-for-slightly-better-email/](https://victormours.mataroa.blog/blog/an-idea-for-slightly-better-email/)

# Specification

When sending an email, automated systems can add the `X-Message-Type` SMTP header with one of three possible values : `one_time_passcode`, `password_reset`, or `receipt`.

In each of these cases, the email client can handle the email in a specific way, for instance by auto-archiving them, labeling them, or automatically sorting them into specific folders. The exact behavior is at the discretion of the email client.


## Possible values

### `one_time_passcode`

#### When to use it

The email is sending a passcode which is meant to be used one time to authentify. This is done as a reaction to an action the user is taking on the app (web or mobile) that is sending the email, such as trying to log in or perform a sensitive action.
For example, an email sending a six-digit one time passcode to a user trying to log in should use this value.

#### Possible behavior for the email client

- Archive the message (maybe in a specific directory) an hour after receiving it
- If the user opens their inbox just after or just before receiving this email, it's probably because they're looking for it. Maybe the email can be opened automatically or displayed more prominently than other emails in the inbox.

### `password_reset`

#### When to use it

The email is sending the user a link to reset their password to an application. This is done as a reaction to the user asking for this while trying to log in.

#### Possible behavior for the email client

- Archive the message (maybe in a specific directory) after the user clicks a link on the email.
- If the user opens their inbox just after or just before receiving this email, it's probably because they're looking for it. Maybe it can be opened automatically or displayed more prominently than other emails in the inbox.

### `receipt`

#### When to use it

For any email that confirms an action that the recipient of the email just took, and that is intended to keep a papertrail. The typical use case is an order confirmation email on an e-commerce site, but it can also be used for other types of confirmations: booking an appointment, signing up for a newsletter, confirming that your support ticket has been received, etc…

#### Possible behavior for the email client:

Group these in a specific directory, and don't count them towards the total of unread messages in the main inbox. Maybe don't send a push notification when receiving them. Maybe make this behavior optional as some users might be confused that they don't get a push notification for these.


## Open discussions

#### Possible improvements and open questions

For receipts: should this be used if there is additional information, such as tracking information or a pdf of a train ticket? I think it should
For one time passcodes: should the passcode be available as metadata to allow copying it to the clipboard with just one click?
If we figure out other use cases, other values for the header can be added to this list. If it turns out that one email can serve multiple purposes, we could have a comma-separated list of values. These possibilities make the spec easy to extend.
- For email receipts: should the url to reset the email be available as metadata?


## Questions about the header name (because naming is hard):

- Is the `X-` necessary? If this ever becomes part of the official spec, it will be without the `X-`. Maybe email clients can be liberal in what they accept, and accept both `X-Message-Type` and `Message-Type`.
- Is the `Message` redundant? I don’t think so: some headers are about the sender or the recipient, it’s reasonable to make it explicit that this one is about the content of the email.

### Further improvements

Another possibility is that we figure out other improvements that we could make on certain workflows.
For example, should we add a `X-Most-Relevant-Before` header with an ISO8601 datetime, to indicate when the information sent by email is no longer useful? In the case of a one-time passcode, this could be the expiration time of the passcode, but for other use cases,


## FAQ

Should this be an RCF? Ultimately yes. We’re trying to get some real-world usage and feedback before writing it up.

Feel free to open an issue if you'd like to discuss any of these!
