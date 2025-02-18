---
title: Use Azure Pipelines to build and push container images to registries
description: Use Azure Pipelines to build and push container images to Docker Hub and Azure Container Registry.
ms.topic: tutorial
ms.assetid: 3ce59600-a7f8-4a5a-854c-0ced7fdaaa82
ms.author: v-catherbund
author: cebundy
ms.date: 12/20/2024
monikerRange: 'azure-devops'
zone_pivot_groups: pipelines-docker-acr

---

# Use Azure Pipelines to build and push container images to registries

[!INCLUDE [version-eq-2022](../../../includes/version-eq-azure-devops.md)] 

This article guides you through the creation of a pipeline to build and push a Docker image to an Azure Container Registry or Docker Hub. 

## Prerequisites

:::zone pivot="acr-registry"

| **Product** | **Requirements**   |
|---|---|
| **Azure DevOps** | - An [Azure DevOps project](../../../organizations/projects/create-project.md).<br>   - **Permissions:**<br>      &nbsp;&nbsp;&nbsp;&nbsp;- To grant access to all pipelines in the project: You must be a member of the [Project Administrators group](../../../organizations/security/change-project-level-permissions.md).<br>      &nbsp;&nbsp;&nbsp;&nbsp;- To create service connections: You must have the *Administrator* or *Creator* role for [service connections](../../library/add-resource-protection.md).<br>   - If you're using a self-hosted agent, ensure Docker is installed and the Docker engine is running with elevated privileges. Microsoft-hosted agents have Docker preinstalled. |
| **GitHub** | - A [GitHub](https://github.com) account.<br>   - A GitHub repository with a Dockerfile. Use the [sample repository](https://github.com/MicrosoftDocs/pipelines-javascript-docker) if you don't have your own project.<br>   - A [GitHub service connection](../../library/service-endpoints.md#github-service-connection) to authorize Azure Pipelines.|
| **Azure** | - An [Azure subscription](https://azure.microsoft.com/free/).<br>   - An [Azure Container Registry](/azure/container-registry/container-registry-get-started-portal). |

:::zone-end

::: zone pivot="docker-registry"

| **Product** | **Requirements**   |
|---|---|
| **Azure DevOps** | - An [Azure DevOps project](../../../organizations/projects/create-project.md).<br>   - **Permissions:**<br>      &nbsp;&nbsp;&nbsp;&nbsp;- To grant access to all pipelines in the project: You must be a member of the [Project Administrators group](../../../organizations/security/change-project-level-permissions.md).<br>      &nbsp;&nbsp;&nbsp;&nbsp;- To create service connections: You must have the *Administrator* or *Creator* role for [service connections](../../library/add-resource-protection.md).<br>   - If you're using a self-hosted agent, ensure Docker is installed and the Docker engine is running with elevated privileges. Microsoft-hosted agents have Docker preinstalled. |
| **GitHub** | - A [GitHub](https://github.com) account.<br>   - A GitHub repository with a Dockerfile. Use the [sample repository](https://github.com/MicrosoftDocs/pipelines-javascript-docker) if you don't have your own project.<br>   - A [GitHub service connection](../../library/service-endpoints.md#github-service-connection) to authorize Azure Pipelines.|
| **Docker Hub**   | - A [Docker Hub](https://hub.docker.com/) account.<br>   - A [Docker Hub](https://hub.docker.com/) image repository. |

## Create a Docker registry service connection

Before pushing container images to a registry, you need to create a service connection in Azure DevOps. This service connection stores the credentials required to securely authenticate with the container registry. For more information, see [Docker Registry service connections](../../library/service-endpoints.md#docker-registry-service-connection).


1. In your Azure DevOps project, select **Project settings** > **Service connections**.

    :::image type="content" source="../media/project-settings-selection.png" alt-text="Screenshot of Project settings selection.":::

1. Select **New service connection** and **Docker Registry**.

    :::image type="content" source="../media/docker-registry-selection.png" alt-text="Screenshot of Docker Registry selection.":::

1. Select **Docker Hub** and enter the following information:

    | Field | Description |
    |---|---|
    | **Docker ID** | Enter your Docker ID. |
    | **Docker Password** | Enter your Docker password. |
    | **Service connection name** | Enter a name for the service connection. |
    | **Grant access permission to all pipelines** | Select this option to grant access to all pipelines. |

    :::image type="content" source="../media/docker-hub-service-connection-dialog.png" alt-text="Screenshot of Docker Hub service connection dialog.":::

1. Select **Verify and save**.

:::zone-end

## Create a pipeline to build and push a Docker image

::: zone pivot="docker-registry"

# [YAML](#tab/yaml)

The Docker@2 task is used to build and push the image to the container registry.
The [Docker@2 task](/azure/devops/pipelines/tasks/build/docker) is designed to streamline the process of building, pushing, and managing Docker images within your Azure Pipelines. This task supports a wide range of Docker commands, including build, push, login, logout, start, stop, and run.

Use the following steps to create a YAML pipeline that uses the Docker@2 task to build and push the image.  


1. In to your Azure DevOps project, select **Pipelines** and **New pipeline**.

1. Select **GitHub** as the location of your source code and select your repository.
    - If you're redirected to GitHub to sign in, enter your GitHub credentials.
    - If you're redirected to GitHub to install the Azure Pipelines app, select **Approve and install**.
1. Select your repository.
1. Select the **Starter pipeline** template to create a basic pipeline configuration.
1. Replace the contents of **azure-pipelines.yml** with the following code: 

    ```yaml

    trigger:
    - main
    
    pool:
      vmImage: 'ubuntu-latest' 
    
    variables:
      repositoryName: '<target repository name>' 
    
    steps:
    - task: Docker@2
      inputs:
        containerRegistry: '<docker registry service connection>'
        repository: $(repositoryName)
        command: 'buildAndPush'
        Dockerfile: '**/Dockerfile'
    
    ```

1. Edit the pipeline YAML file as follows:
    - Replace `<target repository name>` with the name of the repository in the container registry where you want to push the image.
    - Replace `<docker registry service connection>` with the name of the Docker registry service connection you created earlier.

1. When you're done, select **Save and run** > **Save and run**.

1. Select **Job** to view the logs and verify the pipeline ran successfully.

# [Classic](#tab/classic)

### Create a pipeline using the classic editor

1. From your Azure DevOps project, select **Pipelines** and **New pipeline**
1. Select **Use the classic editor** from the **Where is your code?** page.
1. On the **Select a source** page, select **GitHub**.
1. Choose your repository and select **Continue**.
1. On the **Select a template** page, select **Empty pipeline** and **Apply**.
1. Select **ubuntu latest** for the **Agent Specification**.

    :::image type="content" source="../media/classic-docker-pipeline-dialog.png" alt-text="Screenshot of classic Docker pipeline.":::

### Add the Docker@2 task to the pipeline

To build and push the image to the container registry, add the Docker@2 task to the pipeline.

1. Add a task to the **Agent job 1**.
  
    :::image type="content" source="../media/classic-pipeline-add-task.png" alt-text="Screenshot of add task icon.":::

1. Select the **Docker** task, and **Add**.
1. Select the **buildAndPush** task.
1. For **Container Registry**, select the service connection you created earlier. If you don't have one, select ***+New*** to create a new Docker Hub service connection.

    :::image type="content" source="../media/classic-pipeline-docker-2-build-and-push-task.png" alt-text="Screenshot of task to build and push image to Docker Hub.":::

### Run the pipeline

1. Select **Save and queue** > **Save and Queue**.
1. On the **Run pipeline** page, select **Save and run**.
1. Select **Job** to view the logs and verify the pipeline ran successfully.


---

:::zone-end

::: zone pivot="acr-registry"

# [YAML](#tab/yaml)

1. Go to your Azure DevOps project and select **Pipelines** from the left-hand menu.

1. Select **New pipeline**.
1. Select **GitHub** as the location of your source code and select your repository.
    - If you're redirected to GitHub to sign in, enter your GitHub credentials.
    - If you're redirected to GitHub to install the Azure Pipelines app, select **Approve and install**.
1. Select the **Docker -  Build and push an image to Azure Container Registry** template.
1. Select your Azure subscription and **Continue**.
1. Select your Container Registry, and then select **Validate and configure**.

    Example YAML pipeline:

    ```yml
    # Docker
    # Build and push an image to Azure Container Registry
    # https://docs.microsoft.com/azure/devops/pipelines/languages/docker
    
    trigger:
    - main
    
    resources:
    - repo: self
    
    variables:
      # Container registry service connection established during pipeline creation
      dockerRegistryServiceConnection: '7f9dc28e-5551-43ee-891f-33bf61a995de'
      imageRepository: 'usernamepipelinesjavascriptdocker'
      containerRegistry: 'repoistoryname.azurecr.io'
      dockerfilePath: '$(Build.SourcesDirectory)/app/Dockerfile'
      tag: '$(Build.BuildId)'
    
      # Agent VM image name
      vmImageName: 'ubuntu-latest'
    
    stages:
    - stage: Build
      displayName: Build and push stage
      jobs:
      - job: Build
        displayName: Build
        pool:
          vmImage: $(vmImageName)
        steps:
        - task: Docker@2
          displayName: Build and push an image to container registry
          inputs:
            command: buildAndPush
            repository: $(imageRepository)
            dockerfile: $(dockerfilePath)
            containerRegistry: $(dockerRegistryServiceConnection)
            tags: |
              $(tag)
    
    ```

1. Select **Save and run** and **Save and run** again.
1. Select **Job** to view the logs and verify the pipeline ran successfully.

The Docker template creates the service connection to your Azure Container Registry and uses the Docker@2 task to build and push the Docker image to the registry. 

The [Docker@2 task](/azure/devops/pipelines/tasks/reference/docker-v2) is designed to streamline the process of building, pushing, and managing Docker images within your Azure Pipelines. This task supports a wide range of Docker commands, including build, push, login, logout, start, stop, and run.

# [Classic](#tab/classic)

### Create a pipeline using the classic editor

1. From your Azure DevOps project, select **Pipelines** and **New pipeline**.
1. Select **Use the classic editor** from the **Where is your code?** page.
1. On the **Select a source** page, select **GitHub**.
1. Choose your repository and select **Continue**.
1. On the **Select a template** page, select **Empty pipeline** and **Apply**.
1. Select **ubuntu latest** for the **Agent Specification**.

    :::image type="content" source="../media/classic-docker-pipeline-dialog.png" alt-text="Screenshot of classic Docker pipeline editor.":::

### Add the Docker@2 task to the pipeline

Add a **Docker@2** task to the pipeline to build and push the image to the container registry. 

1. Add a task to **Agent job 1**.

    :::image type="content" source="../media/classic-pipeline-add-task.png" alt-text="Screenshot of add task icon.":::

1. Select the **Docker** task, and **Add**.
1. Select the **buildAndPush** task.
1. Create a service connection select **+New**.

    :::image type="content" source="../media/classic-pipeline-new-service-connection-selection.png" alt-text="Screenshot of new service connection selection."::: 

1. Fill in the following fields:

    | Field | Description |
    |---|---|
    | **Subscription** | Select your Azure subscription. |
    | **Azure container registry** | Select your Azure container registry. |
    | **Service connection name** | Enter a name for the service connection. |
    | **Grant access permission to all pipelines** | Select this option to grant access to all pipelines. |

    :::image type="content" source="../media/classic-pipeline-new-azure-container-registry-service-connection-dialog.png" alt-text="Screenshot of new Azure Container Registry service connection.":::

1. Select **Save**.

### Run the pipeline
    
1. Select **Save and queue** > **Save and Queue**.
1. On the **Run pipeline** page, select **Save and run**.
1. Select **Job** to view the logs and verify the pipeline ran successfully.

---

:::zone-end

When using self-hosted agents, be sure that Docker is installed on the agent's host, and the Docker engine/daemon is running with elevated privileges.  

## Related articles

- [Build a container image](build-image.md)
- [Docker content trust](content-trust.md)