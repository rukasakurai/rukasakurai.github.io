Q: Is it possible to set an Azure Policy so that resources cannot be deployed if the current cost is over a specific threshold?
A: 
- As of 2022 July 6th, there does not seem to be a relevant Policy under the Policy samples in https://github.com/Azure/azure-policy
- It might be possible to set a cost alert that triggers an action that enables a policy that prevents deployment of new resources