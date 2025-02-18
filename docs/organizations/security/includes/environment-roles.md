
::: moniker range="azure-devops"
> [!div class="mx-tdCol2BreakAll"]  
> | Role | Description |
> |------------------------------------|---------|
> | **Creator** | Global role, available from environments hub security option. Members of this role can create the environment in the project. Contributors are added as members by default. Required to trigger a YAML pipeline when the environment does not already exist.|
> | **Reader** | Members of this role can view the environment. |
> | **User** | Members of this role can use the environment when creating or editing YAML pipelines. |
> | **Administrator** | Members of this role can administer permissions, create, manage, view and use environments. For a particular environment, its creator is added as Administrator by default. Administrators can also open access to an environment to all pipelines. |
::: moniker-end

::: moniker range="< azure-devops"
> [!IMPORTANT]
> When you create an environment, only the creator has the administrator role. 

> [!div class="mx-tdCol2BreakAll"]  
> | Role | Description |
> |------------------------------------|---------|
> | **Creator** | Global role, available from environments hub security option. Members of this role can create the environment in the project. Contributors are added as members by default. Required to trigger a YAML pipeline when the environment does not already exist.|
> | **Reader** | Members of this role can view the environment. |
> | **User** | Members of this role can use the environment when creating or editing YAML pipelines. |
> | **Administrator** | In addition to using the environment, members of this role can manage membership of all other roles for the environment. Creators are added as members by default. |
::: moniker-end
