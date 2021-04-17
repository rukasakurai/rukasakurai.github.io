# Updating a QnA Maker Knowledge Base
## Problem statement
The situation is that I previous created a QnA Maker Knowledge Base, and I would like to update it with new knowledge that I have acquired. I have about 30 news question and answer pairs that I would like to add.

In doing this, there are several quesions that come to mind
- Q: The new question and answer pairs are in a new knowledge domain, as in they are questions which would likely be asked by people different from my original knowledge base. In this case, should I create a knew knowledge base or add to the existing knowledge base?
  - A: I've decided to create a new knowledge base for now. If there is a need to merge the knowledge bases, then I should be able to easily do that later by exporting the knowledge bases in Excel, merging them, and then importing the merged Excel into a knowledge base
- Q: How should I import the additonal 30 new question and answer pairs that I have stored in a text file?
  - A: Since [this documentation](https://docs.microsoft.com/azure/cognitive-services/qnamaker/reference-document-format-guidelines#structured-qna-document) says that QnA Maker should be able to import files "in the form of alternating Questions and Answers per line," I tried importing the text file. It did a pretty good job, but had trouble when a question did not end with a question mark. Some sentences without a question mark at the end were interpreted as answers to a previous sentence with a question mark. Somehow it was able to recognize that some sentences without a question mark were questions. To my pleasant surprise, it was able to recognize Japanese questions marks too (i.e., "？" as opposed to "?") 
  - A: I then downloaded the knowledge base as an Excel file, fixed errors, and uploaded the Excel file to the knowledge base
