# 7/12/2024
# SOURCE: 
https://github.com/pietronromano/dotnetcore-sqldb-ghactions

https://learn.microsoft.com/en-us/azure/app-service/app-service-sql-asp-github-actions?view=azure-devops


rg="rg-devops"
echo $rg

adapp=ghactionsapp
echo $adapp

## This command will output JSON with an appId that is your client-id. The id is APPLICATION-OBJECT-ID and it will be used for creating federated credentials with Graph API calls. Save the value to use as the AZURE_CLIENT_ID GitHub secret late
az group create --name $rg --location westeurope

## This command generates JSON output with a service principal id. The service principal id is used as the value of the --assignee-object-id argument in the az role assignment create command in the next step
az ad app create --display-name $adapp
{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#applications/$entity",
  "addIns": [],
  "api": {
    "acceptMappedClaims": null,
    "knownClientApplications": [],
    "oauth2PermissionScopes": [],
    "preAuthorizedApplications": [],
    "requestedAccessTokenVersion": null
  },
  "appId": "597d91c0-d9a7-4b99-afbc-9e508898179c",
  "appRoles": [],
  "applicationTemplateId": null,
  "certification": null,
  "createdDateTime": "2024-12-07T09:02:39.7238884Z",
  "defaultRedirectUri": null,
  "deletedDateTime": null,
  "description": null,
  "disabledByMicrosoftStatus": null,
  "displayName": "ghactionsapp",
  "groupMembershipClaims": null,
  "id": "a9a3fe3a-6362-4479-864a-d16a76fd06ec",
  "identifierUris": [],
  "info": {
    "logoUrl": null,
    "marketingUrl": null,
    "privacyStatementUrl": null,
    "supportUrl": null,
    "termsOfServiceUrl": null
  },
  "isDeviceOnlyAuthSupported": null,
  "isFallbackPublicClient": null,
  "keyCredentials": [],
  "nativeAuthenticationApisEnabled": null,
  "notes": null,
  "optionalClaims": null,
  "parentalControlSettings": {
    "countriesBlockedForMinors": [],
    "legalAgeGroupRule": "Allow"
  },
  "passwordCredentials": [],
  "publicClient": {
    "redirectUris": []
  },
  "publisherDomain": "pietronromanolive.onmicrosoft.com",
  "requestSignatureVerification": null,
  "requiredResourceAccess": [],
  "samlMetadataUrl": null,
  "serviceManagementReference": null,
  "servicePrincipalLockConfiguration": null,
  "signInAudience": "AzureADMyOrg",
  "spa": {
    "redirectUris": []
  },
  "tags": [],
  "tokenEncryptionKeyId": null,
  "uniqueName": null,
  "verifiedPublisher": {
    "addedDateTime": null,
    "displayName": null,
    "verifiedPublisherId": null
  },
  "web": {
    "homePageUrl": null,
    "implicitGrantSettings": {
      "enableAccessTokenIssuance": false,
      "enableIdTokenIssuance": false
    },
    "logoutUrl": null,
    "redirectUriSettings": [],
    "redirectUris": []
  }
}


appId="597d91c0-d9a7-4b99-afbc-9e508898179c"
echo $appId

objectId="a9a3fe3a-6362-4479-864a-d16a76fd06ec"
echo $objectId

az ad sp create --id $appId
{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#servicePrincipals/$entity",
  "accountEnabled": true,
  "addIns": [],
  "alternativeNames": [],
  "appDescription": null,
  "appDisplayName": "ghactionsapp",
  "appId": "597d91c0-d9a7-4b99-afbc-9e508898179c",
  "appOwnerOrganizationId": "599fd2f6-80be-4f0d-9b03-b3e74fdcf211",
  "appRoleAssignmentRequired": false,
  "appRoles": [],
  "applicationTemplateId": null,
  "createdDateTime": "2024-12-07T09:07:43Z",
  "deletedDateTime": null,
  "description": null,
  "disabledByMicrosoftStatus": null,
  "displayName": "ghactionsapp",
  "homepage": null,
  "id": "a383d509-f577-4a59-8562-ed65fe73a003",
  "info": {
    "logoUrl": null,
    "marketingUrl": null,
    "privacyStatementUrl": null,
    "supportUrl": null,
    "termsOfServiceUrl": null
  },
  "keyCredentials": [],
  "loginUrl": null,
  "logoutUrl": null,
  "notes": null,
  "notificationEmailAddresses": [],
  "oauth2PermissionScopes": [],
  "passwordCredentials": [],
  "preferredSingleSignOnMode": null,
  "preferredTokenSigningKeyThumbprint": null,
  "replyUrls": [],
  "resourceSpecificApplicationPermissions": [],
  "samlSingleSignOnSettings": null,
  "servicePrincipalNames": [
    "597d91c0-d9a7-4b99-afbc-9e508898179c"
  ],
  "servicePrincipalType": "Application",
  "signInAudience": "AzureADMyOrg",
  "tags": [],
  "tokenEncryptionKeyId": null,
  "verifiedPublisher": {
    "addedDateTime": null,
    "displayName": null,
    "verifiedPublisherId": null
  }
}

spId="a383d509-f577-4a59-8562-ed65fe73a003"
echo $spId
tenantId="599fd2f6-80be-4f0d-9b03-b3e74fdcf211"
echo $tenantId

subscriptionId="a2401ab8-bd17-453c-a13b-ae728a0271e9"
echo $subscriptionId

## NOTE: removed first forward slash / after scope for this to work
az role assignment create --role contributor --subscription $subscriptionId --assignee-object-id  $spId --assignee-principal-type ServicePrincipal --scope subscriptions/$subscriptionId/resourceGroups/$rg

{
  "condition": null,
  "conditionVersion": null,
  "createdBy": null,
  "createdOn": "2024-12-07T09:24:21.669057+00:00",
  "delegatedManagedIdentityResourceId": null,
  "description": null,
  "id": "/subscriptions/a2401ab8-bd17-453c-a13b-ae728a0271e9/resourceGroups/rg-devops/providers/Microsoft.Authorization/roleAssignments/9239caf1-c3ca-4c95-8e49-6298942a2a07",
  "name": "9239caf1-c3ca-4c95-8e49-6298942a2a07",
  "principalId": "a383d509-f577-4a59-8562-ed65fe73a003",
  "principalType": "ServicePrincipal",
  "resourceGroup": "rg-devops",
  "roleDefinitionId": "/subscriptions/a2401ab8-bd17-453c-a13b-ae728a0271e9/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
  "scope": "/subscriptions/a2401ab8-bd17-453c-a13b-ae728a0271e9/resourceGroups/rg-devops",
  "type": "Microsoft.Authorization/roleAssignments",
  "updatedBy": "11fd3ee4-4270-468c-b818-ad197363dc36",
  "updatedOn": "2024-12-07T09:24:21.881557+00:00"
}

## Run the following command to create a new federated identity credential for your Microsoft Entra ID application.
## my subject:  "subject": "repo:pietronromano/dotnetcore-sqldb-ghactions:ref:refs/heads/main"
az ad app federated-credential create --id $objectId --parameters credential.json
{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#applications('a9a3fe3a-6362-4479-864a-d16a76fd06ec')/federatedIdentityCredentials/$entity",
  "audiences": [
    "api://AzureADTokenExchange"
  ],
  "description": "Testing",
  "id": "f3891466-0fdf-411e-9954-65440f79f1ce",
  "issuer": "https://token.actions.githubusercontent.com",
  "name": "myCred",
  "subject": "repo:pietronromano/dotnetcore-sqldb-ghactions:ref:refs/heads/main"      
}


# Create two new secrets in your GitHub repository for SQLADMIN_PASS and SQLADMIN_LOGIN.
SQLADMIN_LOGIN pietronromano
SQLADMIN_PASS Axml-xsl0123
AZURE_SUBSCRIPTION_ID a2401ab8-bd17-453c-a13b-ae728a0271e9


# Running the workflow
## had to specify the project
dotnet build DotNetCoreSqlDb.csproj