Under construction

Many applications send email to its users. For example, e-commerce sites usually send order confirmation emails to users, or an application used within a company might send confirmation e-mails to internal e-mail addressses.

In a typical case, an application typically uses SMTP to submit the e-mail content to the appropriate mail server (there are often SMTP relays that relay the email). The recipient picks up the message from their mail server using POP3 or IMAP.
https://en.wikipedia.org/wiki/Email

When sending email to addresses that are not under your control, you typically need to send the email from a trusted IP addresses, otherwise the e-mail will be marked as spam. If one's company does not have an SMTP relay service to service this purpose, then SendGrid (Pro Plan or Premier Plan for dedicated IP address, to minimize spam issues) may be an option:
https://docs.microsoft.com/en-us/azure/virtual-network/troubleshoot-outbound-smtp-connectivity

When designing an application that sends email, check what your company uses for users and applications to send email, and it might make sense for your application to leverage that. For example, if your company uses Microsoft 365, then the application could use Microsoft Graph:  https://docs.microsoft.com/en-us/graph/api/resources/mail-api-overview?view=graph-rest-1.0. Microsoft Exchange may be able to do this too. SendGrid could also be an option. This means that application teams should usually talk to the IT department that manages the company's email system.