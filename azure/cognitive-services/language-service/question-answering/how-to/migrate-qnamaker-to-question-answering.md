# Migrate from QnA Maker to Question Answering
## Tried on 2022-09-22
The reason for the migration were the following:
- To migrate to a new Azure Subscription in another Azure AD tenant
- To migrate from QnA maker to Cognitive Services with Custom Answering
All I wanted to do is to migrate the knowledge-base, so followed these steps:
https://learn.microsoft.com/en-us/azure/cognitive-services/language-service/question-answering/how-to/migrate-qnamaker#steps-to-migrate-knowledge-bases

I created a new Azure Cognitive Service with Custom Answering in my new Azure Subscription.

I tried to start the migration process by clicking "Start migration", but I had trouble figuring out how to specify the the Azure Cognitive Service resource in the other Azure Subscription on a different Azure AD tenant. The QnA maker UI displays the message "You have 1 knowledge base(s) pending migration to the improved Question answering." but I could not figure out how to access the pending migration.

Decided to export as Excel (kb-powerplatform.xlsx), and save it in my OneDrive under my Cogntive Services folder.

Exported and uploaded ARM template here:
https://github.com/rukasakurai/qna-powerplatform