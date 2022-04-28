# Notes about Software Development with GitHub and Azure
This page is currently very work in progress

GitHub and Azure are great tools to be used for software development whether you are starting from scratch or building upon existing assets
- GitHub as a code repository is great because it enforces the use of git, which provide a great structure for developer collaboration and tracking changes 
- GitHub Actions can also be used to automate CI/CD (e.g., unit testing, integration testing, deployment)
- Azure is a great place to run your software because it is easy and quick to prepare the compute/storage/networking needed to server your software to users

## Demo
### VS Code, GitHub, GitHub Actions, App Service
### VS Code, GitHub, Azure Pipelines, App Service
### VS Code, GitHub, GitHub Actions, Azure Storage (Static Web Apps) (JavaScript)
https://docs.microsoft.com/learn/modules/publish-app-service-static-web-app-api/
From code to scale! Build a static web app in minutes | INT159A: https://www.youtube.com/watch?v=q-xRf4PW5K4
### VS Code, GitHub, Azure Pipelines, Azure Storage (Static Web Apps)
### VS Code, Azure Repos, Azure Pipelines, App Service
e.g., [This app](https://app-demoproduct.azurewebsites.net/) on App Serive is currently being developed using VS Code, the code is stored in Azure Repos, with automated deployment using Azure Pipelines
### VS Code, GitHub, Azure Storage (Static Web Apps)
Does not include CI/CD
https://docs.microsoft.com/azure/static-web-apps/getting-started
### ?
https://docs.microsoft.com/learn/modules/create-deploy-static-webapp-gatsby-app-service/

## Implications for ...
- Software development teams: Software development teams would likely prefer using a git repository like GitHub over other methods such as Subversion or CVS
- Software application prototyping teams: For software application prototyping teams with more of a focus on clarifying user needs and with less software development expertise, GitHub and Azure may be too complex, and tools such as PowerApps might be more suitable. However if more customization than available with tools like PowerApps is needed or if the software application grows in complexity, then it would be good to maintain code in GitHub and to run the code in Azure
- Entrepreneurs/intrapreneurs: If it's possible to automate common processes with software, then that can either become your core competiveness or can add to your competitiveness. Anyone you onboard on to your team in roles related to software will likely be competent or at least be aware of GitHub and Azure
- Existing business/service owners: If you already operate a business/service, then automation can likely help improve service delivery speed and quality. In many cases tools like PowerApps, Power Automate, Power BI, and Dynamics might be enough, but if you are willing to invest in developing and maintaining custom code, then you are likely hiring software engineers, and they are likely already using or would like to use GitHub/Azure. Your developers might appreciate it if you asked them if they'd like to use GitHub and Azure, which might improve their speed of delivery
- IT department: If custom code is an important asset of the business, then how that code is maintained and governed is likely important to you. GitHub and Azure both provide great ways to govern code
- Embedded software engineers: While the final place your code gets deployed may not be on Azure, you could probably benefit from using GitHub as a code repository. Also, Azure is a great place to simulate the behavior of hard and the usage scenarios.
- Engineers in fiels other than software: You may have noticed that there's more software in the things that you design and build. While you may rely on others for the software, it might be worthwhile to understand the key tools that software developers are using these days  
- Bloggers: The cotents of this page is stored and made available on GitHub
- Retired grandparents: Unless you are developing code after retirement, if your main focus is enjoying things like reading your favorite novel, gardening, or chatting with your family and friends, then GitHub and Azure is likely not something that you'd need to think about. That being said, GitHub and Azure help enable more and more of the things that you enjoy everyday in some indirect way, so it might be interesting to learn about it