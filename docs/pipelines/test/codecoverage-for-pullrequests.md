---
title: Code coverage for pull requests
description: Continuous testing. Measuring code coverage for changes in pull requests.
ms.assetid: 1B7C890E-FB67-4BEF-A48E-20C9453BD54A
ms.topic: conceptual
ms.custom: continuous-test, cross-service
ms.author: jeom
author: raviLiftr
ms.date: 09/14/2023
monikerRange: 'azure-devops'
---

# Code coverage for pull requests

[!INCLUDE [version-eq-azure-devops](../../includes/version-eq-azure-devops.md)]

Code coverage is an important quality metric and helps you measure the percentage of your project's code that is being tested. To ensure that quality for your project improves over time (or at the least, doesn't regress), new code being brought into the system must be well tested. When developers raise pull requests, knowing whether their changes are covered by tests helps plug any testing holes before the changes are merged into the target branch. Repo owners might also want to set policies to prevent merging large untested changes.

### Full coverage, diff coverage

**Full coverage** is when coverage gets measured for the entire codebase of a project. But, in the context of pull requests, developers focus on the changes they're making and want to know whether the specific lines of code they added or changed are covered. This type of coverage is **diff coverage**.

## Prerequisites

In order to get coverage metrics for a pull request, first configure a pipeline that validates pull requests. In this pipeline, configure the test tool you're using to collect code coverage metrics. Coverage results must then be published to the server for reporting. 

To learn more about collecting and publishing code coverage results for the language of your choice, see the [Ecosystems](../ecosystems/ecosystems.md) section. For example, collect and publish code coverage for [.NET core apps](../ecosystems/dotnet-core.md#collect-code-coverage).

> [!NOTE] 
> While you can collect and publish code coverage results for many different languages using Azure Pipelines, the **code coverage for pull requests** feature discussed in this document is currently available only for .NET and .NET core projects using the Visual Studio code coverage results format (file extension .coverage). Support for other languages and coverage formats will be added in future milestones.

## Coverage status, details, and indicators

Once you configure a pipeline that collects and publishes code coverage, it posts a code coverage status when you create a pull request. By default, the server checks for at least 70% of changed lines being covered by tests. The diff coverage threshold target can be changed to a value of your choice. For more information, see the [settings configuration section](#configuring-coverage-settings) further in this article. 

> [!div class="mx-imgBorder"]
> ![Screenshot showing coverage status check.](media/codecoverage-for-pullrequests/coveragestatuscheck.png)

The status check evaluates the diff coverage value for all the code files in the pull request. If you would like to view the % diff coverage value for each of the files, you can turn on details as mentioned in the configuration section. Turning on details posts details as a comment in the pull request.

> [!div class="mx-imgBorder"]
> ![Screenshot showing coverage detail comments.](media/codecoverage-for-pullrequests/coverageDetailsComments.png)

In the changed files view of a pull request, lines that are changed are also annotated with coverage indicators to show whether those lines are covered.

> [!div class="mx-imgBorder"]
> ![Screenshot showing coverage indicators.](media/codecoverage-for-pullrequests/coverageIndicatorsPR.png)

> [!NOTE] 
> While you can build code from a wide variety of version control systems that Azure Pipelines supports, the **code coverage for pull requests** feature discussed in this document is currently available only for Azure Repos.

## Configuring coverage settings

If you would like to change the default settings of the code coverage experience for pull requests, you must include a configuration YAML file named azurepipelines-coverage.yml at the root of your repo. Set the desired values in this file and it will be used automatically the next time the pipeline runs.

The settings that can be changed are: 

| Setting | Description | Default | Permissible values |
|:---:|:---:|:---:|:---:|
| status | Indicates whether code coverage status check should be posted on pull requests. <br/> Turning off doesn't post any coverage checks and coverage annotations don't appear in the changed files view. | on | on, off |
| target | Target threshold value for diff coverage must be met for a successful coverage status to be posted. | 70% | Desired % number | 
| comments | Indicates whether a comment containing coverage details for each code file should be posted in the pull request | off | on, off |

Example configuration:

```yaml
coverage:
  status:           # Code coverage status will be posted to pull requests based on targets defined below.
    comments: on    # Off by default. When on, details about coverage for each file changed will be posted as a pull request comment. 
    diff:           # Diff coverage is code coverage only for the lines changed in a pull request.
      target: 60%   # Set this to a desired percentage. Default is 70 percent
```

More examples with details can be found in the [code coverage YAML samples repo](https://github.com/microsoftdocs/codecoverage-yaml-samples).

> [!NOTE] 
> Coverage indicators light up in the changed files view regardless of whether the pull request comment details are turned on.

> [!TIP] 
> The coverage settings YAML is different from a YAML pipeline. This is because the coverage settings apply to your repo and will be used regardless of which pipeline builds your code. This separation also means that if you are using the classic designer-based build pipelines, you will get the code coverage status check for pull requests.

## Protect a branch using a code coverage policy

Code coverage status check for pull requests is only a suggestion for developers and it doesn't prevent pull requests with low code coverage from being merged into the target branch. To prevent developers from merging changes that don't meet a coverage threshold, you must configure a [branch policy using the coverage status check](../../repos/git/pr-status-policy.md).

> [!TIP] 
> Code coverage status posted from a pipeline follows the naming convention `{name-of-your-pipeline/codecoverage}`. 

> [!NOTE] 
> Branch policies in Azure Repos (even optional policies) prevent pull requests from completing automatically if they fail. This behavior is not specific to code coverage policy.
>
> [Starting September 2023, the code coverage policy won't be overridden to **Failed** if the build fails. This feature will be enabled for all customers](/azure/devops/release-notes/2023/pipelines/sprint-227-update#disabled-override-of-code-coverage-policy-status-to-failed-when-build-is-failing).

## FAQ

### Which coverage tools and result formats can be used for validating code coverage in pull requests?

Code coverage for pull requests capability is currently only available for Visual Studio Code coverage, `.coverage`, formats. Use it if you publish code coverage using the Visual Studio Test task, the test verb of dotnet core task and the TRX option of the publish test results task.
Support for other coverage tools and result formats will be added in future milestones.

### If multiple pipelines are triggered when a pull request is raised, will coverage get merged across the pipelines?

If multiple pipelines get triggered when a pull request gets raised, code coverage doesn't merge. The capability is currently designed for a single pipeline that collects and publishes code coverage for pull requests. 
If you need to merge coverage data across pipelines, submit a feature request on [Developer Community](https://developercommunity.visualstudio.com/spaces/21/index.html). 

[!INCLUDE [help-and-support-footer](includes/help-and-support-footer.md)]
