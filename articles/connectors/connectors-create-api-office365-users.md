---
title: "在 Logic Apps 中新增 Office 365 使用者連接器 | Microsoft Docs"
description: "搭配 REST API 參數來使用 Office 365 使用者連接器的概觀"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: 8f544b781bbf11214183bc0f46215bb9e16c9f55
ms.contentlocale: zh-tw
ms.lasthandoff: 05/17/2017


---
# <a name="get-started-with-the-office-365-users-connector"></a>開始使用 Office 365 使用者連接器
連接至 Office 365 使用者，以取得設定檔、搜尋使用者等等。 您可以利用 Office 365 使用者來：

* 根據您從 Office 365 使用者所取得的資料，來建置您的商務流程。 
* 使用可取得直屬員工、取得管理員的使用者設定檔等等的動作。 這些動作會收到回應，然後輸出能讓其他動作使用的資料。 例如，取得某人的直屬員工，然後利用此資訊更新 SQL Azure 資料庫。 

您可以從建立邏輯應用程式立即開始，請參閱[建立邏輯應用程式](../logic-apps/logic-apps-create-a-logic-app.md)。

## <a name="create-a-connection-to-office-365-users"></a>建立至 Office 365 使用者的連線
當您將這個連接器新增到邏輯應用程式時，您必須登入您的 Office 365 使用者帳戶，並允許邏輯應用程式連線到您的帳戶。

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

連線建立之後，您需要輸入 Office 365 使用者屬性，像是使用者識別碼。 本主題的＜REST API 參考＞  一節說明這些屬性。

## <a name="view-the-swagger"></a>檢視 Swagger

請參閱 [Swagger 詳細資料](/connectors/officeusers/)。

## <a name="more-connectors"></a>其他連接器
返回 [API 清單](apis-list.md)。
