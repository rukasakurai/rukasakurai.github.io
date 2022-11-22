# Steps to create demo
https://learn.microsoft.com/en-us/microsoftteams/teams-recording-policy#compliance-recording-policy-assignment-and-provisioning
## 00. Workbook
```
$SUB_ID=(Get-AzSubscription -SubscriptionName "xxx").Id
```

## 0. Connect an authenticated account for use with cmdlets from the MicrosoftTeams module
```
Connect-MicrosoftTeams
```

## 1. Create an application instance in Azure Active Directory
```
$UserPrincipalName="cr.instance@contoso.onmicrosoft.com"
$DisplayName="ComplianceRecordingBotInstance"
$ApplicationId="fcc88ff5-a42d-49cf-b3d8-f2e1f609d511"
$ObjectId=(New-CsOnlineApplicationInstance -UserPrincipalName $UserPrincipalName -DisplayName $DisplayName-ApplicationId $ApplicationId).ObjectId
```
This example creates a new application instance for an Auto Attendant with UserPrincipalName "appinstance01@contoso.com", ApplicationId "ce933385-9390-45d1-9512-c8d228074e07", DisplayName "AppInstance01" for the tenant.

The application ID's that you need to use while creating the application instances are:

Auto Attendant: ce933385-9390-45d1-9512-c8d228074e07 Call Queue: 11cd3e2e-fccb-42ad-ad00-878b93575e07

https://learn.microsoft.com/en-us/powershell/module/skype/new-csonlineapplicationinstance?view=skype-ps

### Plan for Teams auto attendants and call queues
https://learn.microsoft.com/en-us/microsoftteams/plan-auto-attendant-call-queue

## 2. Sync the application instance from Azure Active Directory into Agent Provisioning Service
This is needed because the mapping between application instance and application needs to be stored in Agent Provisioning Service. If an application ID was provided at the creation of the application instance, you need not run this cmdlet.

```
Sync-CsOnlineApplicationInstance -ObjectId $ObjectId
```

## 3. Create a new Teams recording policy for governing automatic policy-based recording in your tenant
Automatic policy-based recording is only applicable to Microsoft Teams users

```
$Description="Test policy created by tenant admin"
New-CsTeamsComplianceRecordingPolicy -Identity TestComplianceRecordingPolicy -Enabled $true -Description $Description
```

## 4. Modify an existing Teams recording policy for governing automatic policy-based recording in your tenant
Automatic policy-based recording is only applicable to Microsoft Teams users.
```
$Id="5069aae5-c451-4983-9e57-9455ced220b7"
Set-CsTeamsComplianceRecordingPolicy -Identity TestComplianceRecordingPolicy `
-ComplianceRecordingApplications @(New-CsTeamsComplianceRecordingApplication -Id $Id -Parent TestComplianceRecordingPolicy)

## 5. Assign a per-user Teams recording policy to one or more users
This policy is used to govern automatic policy-based recording in your tenant. Automatic policy-based recording is only applicable to Microsoft Teams users.
```
$Identity="testuser@contoso.onmicrosoft.com"
Grant-CsTeamsComplianceRecordingPolicy -Identity $Identity -PolicyName TestComplianceRecordingPolicy
```