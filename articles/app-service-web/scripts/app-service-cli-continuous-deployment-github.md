---
title: "Azure CLI 指令碼範例 - 建立可從 GitHub 連續部署的 Web 應用程式 | Microsoft Docs"
description: "Azure CLI 指令碼範例 - 建立可從 GitHub 連續部署的 Web 應用程式"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: dfb4c7651f6cfd378f9b1706abcdd732e56d7eb1
ms.contentlocale: zh-tw
ms.lasthandoff: 05/15/2017

---

# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>建立可從 GitHub 連續部署的 Web 應用程式

此範例指令碼會在 App Service 中建立 Web 應用程式及其相關資源，然後設定從 GitHub 存放庫進行的連續部署。 如需進行沒有連續部署的 GitHub 部署，請參閱[建立 Web 應用程式並從 GitHub 部署程式碼](app-service-cli-deploy-github.md)。 在此範例中，您將需要：

* 具有應用程式程式碼的 GitHub 存放庫 (您必須有此存放庫的系統管理權限)。
* 您 GitHub 帳戶的[個人存取權杖 (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use)。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="create-app-sample"></a>建立應用程式範例

[!code-azurecli-interactive[主要](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "建立可從 GitHub 連續部署的 Web 應用程式")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令。 下表中的每個命令都會連結至命令特定的文件。

| 命令 | 注意事項 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 建立用來存放所有資源的資源群組。 |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | 建立 App Service 方案。 |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#delete) | 建立 Azure Web 應用程式。 |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | 將 Azure Web 應用程式關聯到 Git 或 Mercurial 存放庫。 |
| [az appservice web browse](https://docs.microsoft.com/cli/azure/appservice/web#browse) | 在瀏覽器中開啟 Azure Web 應用程式。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](https://docs.microsoft.com/cli/azure/overview)。

您可以在 [Azure App Service 文件](../app-service-cli-samples.md)中找到其他的 App Service CLI 指令碼範例。

