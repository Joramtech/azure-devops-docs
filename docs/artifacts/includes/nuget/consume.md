---
ms.topic: include
ms.service: azure-devops-artifacts
ms.author: rabououn
author: ramiMSFT
ms.date: 05/15/2024
---

#### 1. Get the package source URL

[!INCLUDE [get a NuGet URL](nuget-consume-endpoint.md)]

#### 2. Set up Visual Studio

#### [Windows](#tab/windows/)

1. In Visual Studio, select **Tools** > **Options**.

1. Expand the **NuGet Package Manager** section, and then select **Package Sources**.

1. Enter the feed's **Name** value and the **Source** URL, and then select the green plus sign (+) to add a source.

1. If you enabled upstream sources in your feed, clear the **nuget.org** checkbox.

1. Select **OK** when you're done.

    :::image type="content" source="../../media/vs-addsource.png" alt-text="A screenshot that shows selections for setting up Visual Studio in Windows.":::

#### [macOS](#tab/macOS/)

1. Create a [personal access token](../../../organizations/accounts/use-personal-access-tokens-to-authenticate.md).

1. In Visual Studio, on the menu bar, select **Preferences**.

1. Select **NuGet**, and then select **Sources**.

1. Select **Add** and enter your feed's name, the source URL, a username (any string), and your personal access token.

1. Select **OK** to save your source.

1. If you enabled upstream sources in your feed, clear the **nuget.org** checkbox.

1. Select **OK** when you're done.

    :::image type="content" source="../../media/vs-mac-settings.png" alt-text="A screenshot that shows selections for setting up Visual Studio in macOS.":::

---

#### 3. Download packages

1. In Visual Studio, right-click your project, and then select **Manage NuGet Packages**.

1. Select **Browse**, and then select your feed from the **Package source** dropdown menu.

    :::image type="content" source="../../media/select-pkg-src.png" alt-text="Screenshot that shows selection of a package source in Visual Studio.":::

1. Use the search bar to search for packages from your feed.
