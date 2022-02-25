Under construction
# Context
Many applications send email to its users. For example, e-commerce sites usually send order confirmation emails to users, or an application used within a company might send confirmation e-mails to internal e-mail addressses.

In a typical case, an application typically uses SMTP to submit the e-mail content to the appropriate mail server (there are often SMTP relays that relay the email). The recipient picks up the message from their mail server using POP3 or IMAP.
https://en.wikipedia.org/wiki/Email

When sending email to addresses that are not under your control, you typically need to send the email from a trusted IP addresses, otherwise the e-mail will be marked as spam. If one's company does not have an SMTP relay service to service this purpose, then SendGrid (Pro Plan or Premier Plan for dedicated IP address, to minimize spam issues) may be an option:
https://docs.microsoft.com/en-us/azure/virtual-network/troubleshoot-outbound-smtp-connectivity

When designing an application that sends email, check what your company uses for users and applications to send email, and it might make sense for your application to leverage that. For example, if your company uses Microsoft 365, then the application could use Microsoft Graph:  https://docs.microsoft.com/en-us/graph/api/resources/mail-api-overview?view=graph-rest-1.0. Microsoft Exchange may be able to do this too. SendGrid could also be an option. This means that application teams should usually talk to the IT department that manages the company's email system.

# SendGrid
I followed the following manuals on 2021 September 21 and was successfully able to send e-mail from a .NET 5 console app to my work e-mail address. The e-mail got sorted into spam. When I tried to send an e-mail to my personal e-mail address, it got blocked with the reason "not accepted due to domain's DMARC policy."
https://docs.sendgrid.com/for-developers/partners/microsoft-azure-2021
https://github.com/sendgrid/sendgrid-csharp

Some things to consider when actually using SendGrid for a product (incomplete list)
- How to store the SendGrid API key in a secure manner (as opposed to for example saving it in code)
- How to avoid the email being flagged as spam/junk
- How to avoid the email being blocked from being sent

# .eml file
In Outlook and other applications, emails can be saved as .eml files. 
https://en.wikipedia.org/wiki/Email#Filename_extensions

When manually archiving emails to a file system (e.g., in your computer) saving the email as a .eml file can be useful. While the UX to do this is relatively simple in the desktop version of Outlook (https://support.microsoft.com/en-us/office/save-a-message-as-a-file-4821bcd4-7687-4d6d-a486-b89a291a56e2), as of 2022 Feb the UX for this on OWA is not straight-forward: 
"To save email attachments to your desktop, start a new message > click to edit the message in a new window > select all emails you would like to download > drag the emails to the new message > click the down arrow on the attachment and download the email message, or send the message to yourself and then download all attachments." (https://www.quora.com/How-do-I-save-emails-and-attachments-from-Outlook-Web-App-to-my-desktop)