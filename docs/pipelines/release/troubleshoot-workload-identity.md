---
title: Troubleshoot workload identity service connections
ms.custom: devx-track-arm-template, arm2024
description: Learn how to troubleshoot an Azure Resource Manager workload identity service connection in Azure Pipelines, one of the services in Azure DevOps.
ms.topic: troubleshooting-general
ms.author: jukullam
author: juliakm
ms.date: 06/07/2024
monikerRange: '>= azure-devops'
"recommendations": "true"
---


# Troubleshoot an Azure Resource Manager workload identity service connection

Get help debugging common issues with workload identity service connections. You also learn how to manually create a service connection if you need to.

## Troubleshooting checklist

Use the following checklist to troubleshoot issues with workload identity service connections:

- Review pipeline tasks to ensure that they support workload identity.
- Verify that workload identity federation is active for the tenant.
- Check the issuer URL and federation subject for accuracy.

The following sections describe the issues and how to resolve them.

### Review pipeline tasks

Not all pipelines tasks support workload identity. Specifically, only Azure Resource Manager service connection properties on tasks use workload identity federation. The table below lists workload identity federation support for [tasks included with Azure DevOps](/azure/devops/pipelines/tasks/reference/?view=azure-pipelines&preserve-view=true). For tasks installed from the [Marketplace](https://marketplace.visualstudio.com/search?target=AzureDevOps&category=Azure%20Pipelines&visibilityQuery=all&sortBy=Installs), contact the extension publisher for support.

| Task                                     | Workload identity federation support                                                                                            |
|------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| AutomatedAnalysis@0                      | Y |
| AzureAppServiceManage@0                  | Y |
| AzureAppServiceSettings@1                | Y |
| AzureCLI@1                               | Y |
| AzureCLI@2                               | Y |
| AzureCloudPowerShellDeployment@1         | Use AzureCloudPowerShellDeployment@2 |
| AzureCloudPowerShellDeployment@2         | Y |
| AzureContainerApps@0                     | Y |
| AzureContainerApps@1                     | Y |
| AzureFileCopy@1                          | Use AzureFileCopy@6 |
| AzureFileCopy@2                          | Use AzureFileCopy@6 |
| AzureFileCopy@3                          | Use AzureFileCopy@6 |
| AzureFileCopy@4                          | Use AzureFileCopy@6 |
| AzureFileCopy@5                          | Use AzureFileCopy@6 |
| AzureFileCopy@6                          | Y |
| AzureFunctionApp@1                       | Y |
| AzureFunctionApp@2                       | Y |
| AzureFunctionAppContainer@1              | Y |
| AzureFunctionOnKubernetes@0              | Use AzureFunctionOnKubernetes@1 |
| AzureFunctionOnKubernetes@1              | Y |
| AzureIoTEdge@2                           | Y |
| AzureKeyVault@1                          | Y |
| AzureKeyVault@2                          | Y |
| AzureMonitor@0                           | Use AzureMonitor@1 |
| AzureMonitor@1                           | Y |
| AzureMysqlDeployment@1                   | Y |
| AzureNLBManagement@1                     | N |
| AzurePolicyCheckGate@0                   | Y |
| AzurePowerShell@2                        | Y |
| AzurePowerShell@3                        | Y |
| AzurePowerShell@4                        | Y |
| AzurePowerShell@5                        | Y |
| AzureResourceGroupDeployment@2           | Y |
| AzureResourceManagerTemplateDeployment@3 | Y |
| AzureRmWebAppDeployment@3                | Y |
| AzureRmWebAppDeployment@4                | Y |
| AzureSpringCloud@0                       | Y |
| AzureVmssDeployment@0                    | Y |
| AzureWebApp@1                            | Y |
| AzureWebAppContainer@1                   | Y |
| ContainerBuild@0                         | Y |
| ContainerStructureTest@0                 | Y |
| Docker@0                                 | Y |
| Docker@1                                 | Azure service connection: Y<br/> Docker Registry service connection: N |
| Docker@2                                 | Y |
| DockerCompose@0                          | Y |
| DockerCompose@1                          | Y |
| DotNetCoreCLI@2                          | Y |
| HelmDeploy@0                             | Azure service connection: Y |
| HelmDeploy@1                             | Azure service connection: Y |
| InvokeRESTAPI@1                          | Y |
| JavaToolInstaller@0                      | Y |
| JenkinsDownloadArtifacts@1               | Y |
| Kubernetes@0                             | Use Kubernetes@1  |
| Kubernetes@1                             | Y |
| KubernetesManifest@0                     | Use KubernetesManifest@1 |
| KubernetesManifest@1                     | Y |
| Maven@4                                  | Y |
| Notation@0                               | Y |
| PackerBuild@0                            | Use PackerBuild@1 |
| PackerBuild@1                            | Y |
| PublishToAzureServiceBus@1               | Use PublishToAzureServiceBus@2 with Azure service connection |
| PublishToAzureServiceBus@2               | Y |
| ServiceFabricComposeDeploy@0             | N |
| ServiceFabricDeploy@1                    | N |
| SqlAzureDacpacDeployment@1               | Y |
| VSTest@3                                 | Y |

### Verify that workload identity federation is active

If you see error messages **AADSTS700223** or **AADSTS700238**, workload identity federation was disabled in your Microsoft Entra tenant.

Verify that there are no Microsoft Entra policies in place that block federated credentials.

### Check the issuer URL for accuracy

If you see a message that indicates **no matching federated identity record found**, either the issuer URL or the federation subject doesn't match. The correct issuer URL starts with `https://vstoken.dev.azure.com`.

You can fix the issuer URL by editing and saving the service connection to update the issuer URL. If Azure DevOps didn't create the identity, the issuer URL must be updated manually. For Azure identities, the issuer URL automatically updates.  

## Common issues

The next sections identify common issues and describe causes and resolutions.

<a name='i-dont-have-permissions-to-create-a-service-principal-in-the-microsoft-entra-tenant'></a>

### I don't have permissions to create a service principal in the Microsoft Entra tenant

You can't use the Azure DevOps service connection configuration tool if you don't have the correct permissions. Your permissions level is insufficient to use the tool if you either don't have permissions to create service principals or if you're using a different Microsoft Entra tenant than your Azure DevOps user.

You must either have permissions in Microsoft Entra ID to create app registrations or have an appropriate role (for example, Application Developer).

You have two options to resolve the issue:

- [Solution 1: Manually configure workload identity by using managed identity authentication](configure-workload-identity.md#set-a-workload-identity-service-connection-to-use-managed-identity-authentication)
- [Solution 2: Manually configure workload identity by using app registration authentication](configure-workload-identity.md#app-registration-workload-identity)

## Error messages

The following table identifies common error messages and issues that might generate them:

| Message | Possible issue |
|---------|-----------------|
| **cannot request token: Get `?audience=api://AzureADTokenExchange: unsupported protocol scheme`** | The task doesn't support workload identity federation. |
| **Identity not found** | The task doesn't support workload identity federation. |
| **Could not fetch access token for Azure** | The task doesn't support workload identity federation. |
| **AADSTS700016: Application with identifier '****' wasn't found** | The identity that is used for the service connection no longer exists, might have been removed from the service connection, or is incorrectly configured. If you configure the service connection manually with a pre-created identity, make sure the `appID`/`clientId` is correctly configured. |
| **AADSTS7000215:  Invalid client secret provided.** | You're using a service connection that has an expired secret. [Convert the service connection to workload identity federation](https://aka.ms/azdo-rm-workload-identity-conversion) and replace the expired secret with federated credentials. |
| **AADSTS700024: Client assertion is not within its valid time range** | If the error happens after approximately 1 hour, use a service connection with [Workload identity federation and a Managed Identity](configure-workload-identity.md#set-a-workload-identity-service-connection-to-use-managed-identity-authentication) instead. Managed Identity tokens have a [lifetime of around 24 hours](/entra/identity/managed-identities-azure-resources/managed-identities-faq#are-managed-identities-tokens-cached). <br/> If the error happens before 1 hour but after 10 minutes, move commands that (implicitly) request an access token to e.g. access Azure storage to the beginning of your script. The access token will be cached for subsequent commands. |
| **AADSTS70021: No matching federated identity record found for presented assertion. Assertion Issuer: `https://app.vstoken.visualstudio.com`.** | No federated credential was created or the issuer URL isn't correct. The correct issuer URL has the format `https://vstoken.dev.azure.com/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`. You can fix the issuer URL by editing and then saving a service connection. If Azure DevOps didn't create your identity, you must manually update the issuer. You can find the correct issuer in the edit dialog of the service connection or in the response (under authorization parameters) if you use the REST API. |
| **AADSTS70021: No matching federated identity record found for presented assertion. Assertion Issuer:  `https://vstoken.dev.azure.com/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`. Assertion Subject: `sc://<org>/<project>/<service-connection>.`** | Either the issuer URL or the federation subject doesn't match. The Azure DevOps organization or project was renamed or a manually created service connection was renamed without updating the federation subject on the identity. |
| **AADSTS700211: No matching federated identity record found for presented assertion issuer** | No federated credential was created or the issuer URL is not correct. |
| **AADSTS700213: No matching federated identity record found for presented assertion subject** | No federated credential was created or the subject is not correct. |
| **AADSTS700223** | Workload identity federation is constrained or disabled on the Microsoft Entra tenant. In this scenario, it may be possible to use a managed identity for the federation instead. For more information, see [Workload identity with managed identity](https://aka.ms/azdo-rm-workload-identity-manual). |
| **AADSTS70025: Client application has no configured federated identity credentials** | Make sure federated credentials are configured on the App registration or Managed Identity. |
| **Microsoft Entra rejected the token issued by Azure DevOps with error code AADSTS700238** | Workload identity federation has been constrained on the Microsoft Entra tenant. The issuer for your organization (`https://vstoken.dev.azure.com/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`) isn't allowed to use workload identity federation. Ask your Microsoft Entra tenant administrator or administration team to allow workload identity federation for your Azure DevOps organization. |
| **AADSTS900382: Confidential Client is not supported in Cross Cloud** | Some sovereign clouds block Workload identity federation. |
| **Failed to obtain the JSON Web Token (JWT) using service principal client ID** | Your federation identity credentials are misconfigured or the Microsoft Entra tenant blocks OpenID Connect (OIDC). |
| **Script failed with error: UnrecognizedArgumentError: unrecognized arguments: --federated-token** | You're using an AzureCLI task on an agent that has an earlier version of the Azure CLI installed. Workload identity federation requires Azure CLI 2.30 or later. |
| **Failed to create an app in Microsoft Entra ID. Error: Insufficient privileges to complete the operation in Microsoft Graph. Ensure that the user has permissions to create a Microsoft Entra Application.** | The ability to create app registrations was [disabled](/azure/active-directory/roles/delegate-app-roles#restrict-who-can-create-applications) in the Microsoft Entra tenant. Assign the user who is creating the service connection the [Application Developer](/azure/active-directory/roles/permissions-reference#application-developer) Microsoft Entra role. Alternatively, create the service connection manually by using a managed identity. For more information, see [Workload identity with managed identity](https://aka.ms/azdo-rm-workload-identity-manual). |

Is the AADSTS error you see not listed above? Check [Microsoft Entra authentication and authorization error codes](/entra/identity-platform/reference-error-codes#aadsts-error-codes).
