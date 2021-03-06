---
title: "將記錄從 Azure 資源整合到 SIEM 系統 | Microsoft Docs"
description: "了解 Azure 記錄整合、其主要功能及運作方式。"
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/20/2017
ms.author: TomSh
ms.translationtype: Human Translation
ms.sourcegitcommit: d9ae8e8948d82b9695d7d144d458fe8180294084
ms.openlocfilehash: 616a29b3f7db817e63a3a3673b845cc86752af77
ms.contentlocale: zh-tw
ms.lasthandoff: 05/23/2017


---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Microsoft Azure 記錄整合簡介
了解 Azure 記錄整合、其主要功能及運作方式。

## <a name="overview"></a>概觀

Azure 記錄整合是免費的解決方案，可讓您將來自 Azure 資源的未經處理記錄，整合到內部部署安全性資訊及事件管理 (SIEM) 系統內。

Azure 記錄整合會從 Windows 事件檢視器頻道、[Azure 活動記錄檔](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)、[Azure 資訊安全中心警示](../security-center/security-center-intro.md)和 Azure 資源的 [Azure 診斷記錄](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)收集 Windows 事件。 這項整合協助您的 SIEM 方案提供內部部署或在雲端中所有資產統一的儀表板，以便您彙總、相互關聯、分析和警示安全性事件。

>[!NOTE]
目前唯一支援的雲端是 Azure 的商業和 Azure Government。 此時不支援其他雲端。

![Azure 記錄整合][1]

## <a name="what-logs-can-i-integrate"></a>可以整合哪些記錄檔？
Azure 會為每項 Azure 服務產生大量記錄。 這些記錄檔表示三種類型的記錄檔︰

* **控制/管理記錄檔**：可讓您看到 [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) 的 CREATE、UPDATE 和 DELETE 作業。 Azure 活動記錄是本類型的一個範例。
* **資料平面記錄檔**：可讓您看到使用 Azure 資源時所引發的事件。 本類型的記錄檔範例是 Windows 事件檢視器**系統**、**安全性**和 Windows 虛擬機器中的**應用程式**頻道。 另一個範例是透過 Azure 監視設定的診斷記錄
* **處理事件**提供分析的事件與以您的名義處理的警示資訊。 本事件類型的範例是 Azure 安全性中心警示，其中 Azure 資訊安全中心已處理並分析您的訂用帳戶，以提供與您目前安全性狀態相關的警示。

Azure 記錄整合目前支援整合 Azure 活動記錄、您的 Azure 訂用帳戶中 Windows 虛擬機器的 Windows 事件記錄檔、Azure 資訊安全中心警示、Azure 診斷記錄及 Azure Active Directory 稽核記錄。

下表說明記錄檔分類和 SIEM 整合詳細資訊

| 記錄類型  |支援 JSON 的記錄分析 (Splunk、ELK)| ArcSight  | QRadar  |   
|---|---|---|---|
|  AAD 稽核記錄 |  是 | 需要建立 FlexConnector JSON 剖析器檔案。 如需詳細資訊，請參閱 ArcSight 文件  |  您必須建立記錄來源擴充。 如需詳細資訊，請參閱 QRadar文件。 |  
| 活動記錄檔  | 是  |  下載中心提供 FlexConnector JSON 剖析器檔案以及 Azure 記錄整合下載 |  [QRadar DSM](https://www.ibm.com/support/knowledgecenter/SSKMKU/com.ibm.dsm.doc/c_dsm_guide_microsoft_azure_overview.html) (透過 Syslog 傳送) |  
| ASC 警示  | 是  |  需要建立 FlexConnector JSON 剖析器檔案。 如需詳細資訊，請參閱 ArcSight 文件。 | [QRadar DSM](https://www.ibm.com/support/knowledgecenter/SSKMKU/com.ibm.dsm.doc/c_dsm_guide_microsoft_azure_overview.html) (透過 Syslog 傳送)   |   
| 診斷記錄 (資源記錄) | 是 | 需要使用者建立 FlexConnector JSON 剖析器檔案。 如需如何進行的指示，請參閱 ArcSight 文件。 | 您必須建立記錄來源擴充。 如需詳細資訊，請參閱 QRadar 文件 |
| VM 記錄 | 是，透過轉寄的事件，而非透過 JSON | 是，透過轉寄的事件 | 是，透過轉寄的事件 |

>[!NOTE]
雖然 Azure 記錄檔整合是免費的解決方案，記錄檔資訊的儲存體仍會產生 Azure 儲存體成本。

可透過 [Azure 記錄檔整合 MSDN 論壇](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration)取得社群協助。 論壇讓 AzLog 社群能針對如何充分利用 Azure 記錄檔整合，透過分享問題、 解答、 秘訣和技巧支援彼此。 此外，Azure 記錄整合小組會監視這個論壇，並且會盡全力提供協助。

您也可以建立[支援要求](../azure-supportability/how-to-create-azure-support-request.md)。 若要這樣做，請選取 [記錄整合] 作為您要求支援的服務。

## <a name="next-steps"></a>後續步驟
在本文中為您介紹了 Azure 記錄整合。 若要深入了解 Azure 記錄整合和支援的記錄檔的類型，請參閱下列各項︰

* [Microsoft Azure 記錄整合](https://www.microsoft.com/download/details.aspx?id=53324) – Azure 記錄整合詳細資料、系統需求和安裝指示的下載中心。
* [開始使用 Azure 記錄整合](security-azure-log-integration-get-started.md) - 本教學課程將逐步引導您安裝 Azure 記錄整合，和來自 Azure WAD 儲存體、Azure 活動記錄、Azure 資訊安全中心警示以及 Azure Active Directory 稽核記錄檔。
* [合作夥伴設定步驟](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – 此部落格文章說明如何設定 Azure 記錄整合，以搭配使用合作夥伴解決方案 Splunk、HP ArcSight 和 IBM QRadar。
* [透過 syslog 傳送到 QRadar 的活動和 ASC 警示 (英文)](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) – 此部落格文章提供透過 syslog 將活動和 Azure 資訊安全中心警示傳送至 QRadar 的步驟
* [Azure 記錄整合常見問題集 (FAQ)](security-azure-log-integration-faq.md) - 此常見問題集會回答有關 Azure 記錄整合的問題。
* [Azure 記錄整合的整合資訊安全中心警示](../security-center/security-center-integrating-alerts-with-log-integration.md) – 本文件說明使用 Azure 記錄整合同步 Azure 資訊安全中心警示。

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png

