---
title: "Azure Key Vault 開發人員指南 | Microsoft Docs"
description: "開發人員可以使用 Azure 金鑰保存庫來管理 Microsoft Azure 環境中的密碼編譯金鑰。"
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 05/10/2017
ms.author: bruceper
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: b046e95e2167009727f6ea8f3dd237619c61434f
ms.contentlocale: zh-tw
ms.lasthandoff: 05/10/2017


---
# <a name="azure-key-vault-developers-guide"></a>Azure 金鑰保存庫開發人員指南

Key Vault 可讓您從應用程式內安全地存取機密資訊︰

- 不需要自行撰寫程式碼即可保護金鑰和密碼，而且也能輕易地從應用程式加以使用。
- 可以讓客戶擁有及管理他們自己的金鑰，因此您可以致力於提供核心軟體功能。 透過這種方式，您的應用程式將不需要對客戶的租用戶金鑰和密碼負起責任或潛在責任。
- 您的應用程式能使用金鑰進行簽署和加密，同時也能在應用程式外部管理金鑰，讓您的解決方案適用於位於不同地點的應用程式。
- 自 2016 年 9 月起推出的 Key Vault，您的應用程式現在可以使用 Key Vault [憑證](https://docs.microsoft.com/rest/api/keyvault/certificate-operations)。 如需詳細資訊，請參閱[關於金鑰、祕密和憑證](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates)。

如需 Azure 金鑰保存庫的一般詳細資訊，請參閱 [什麼是金鑰保存庫？](key-vault-whatis.md)。

## <a name="public-preview---may-10-2017"></a>公開預覽 - 2017 年 5 月 10 日

>[!NOTE]
>Azure Key Vault 的這個預覽版本，只有「虛刪除」功能處於預覽狀態。 整體而言，Azure Key Vault 是一個完整的生產服務。

這個預覽版本包含新的虛刪除功能、可復原的 Key Vault 和 Key Vault 物件刪除，和更新的開發人員介面：[.NET/C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/)、[REST](https://docs.microsoft.com/rest/api/keyvault/) 和 [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/)。 

如需有關新的虛刪除功能的詳細資訊，請參閱 [Azure Key Vault 虛刪除概觀](key-vault-ovw-soft-delete.md)。

## <a name="videos"></a>影片

此影片示範如何建立專屬金鑰保存庫以及如何從 'Hello Key Vault' 範例應用程式使用它。

- [Key Vault 開發人員 - 快速入門指南](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

以上影片中所提及的資源︰

- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure 金鑰保存庫範例程式碼](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>建立及管理金鑰保存庫

在程式碼中使用 Azure 金鑰保存庫之前，您可以透過 REST、資源管理員範本、PowerShell 或 CLI 來建立及管理保存庫，如以下文章所述︰

- [使用 REST 建立和管理金鑰保存庫](https://docs.microsoft.com/rest/api/keyvault/)
- [使用 PowerShell 建立和管理金鑰保存庫](key-vault-get-started.md)
- [使用 CLI 建立和管理金鑰保存庫](key-vault-manage-with-cli2.md)
- [透過 Azure Resource Manager 範本建立金鑰保存庫和新增祕密](../azure-resource-manager/resource-manager-template-keyvault.md)

> [!NOTE]
> 涉及金鑰保存庫的作業乃透過 AAD 進行驗證，以及透過金鑰保存庫自己的存取原則 (每個保存庫有各自的定義) 進行授權。

## <a name="coding-with-key-vault"></a>撰寫金鑰保存庫的程式碼

程式設計人員的 Key Vault 管理系統由幾個介面組成，並以 REST 作為基礎。 透過 REST 介面，可存取所有的金鑰保存庫資源；包括金鑰、祕密和憑證。 [Key Vault REST API 參考](https://docs.microsoft.com/rest/api/keyvault/)。 

### <a name="supported-programming-languages"></a>支援的程式設計語言

#### <a name="net"></a>.NET

- [Key Vault 的 .NET API 參考](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault) 

如需 .NET SDK 2.x 版的詳細資訊，請參閱[版本資訊](key-vault-dotnet2api-release-notes.md)。

#### <a name="java"></a>Java

- [Key Vault 的 Java SDK](https://docs.microsoft.com/java/api/com.microsoft.azure.keyvault)

#### <a name="nodejs"></a>Node.js

- [Key Vault 管理的 Node.js API 參考](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)
- [Key Vault 作業的 Node.js API 參考](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/) 

### <a name="quick-start"></a>快速入門

- [建立金鑰保存庫](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [開始在 Node.js 中使用 Key Vault](https://azure.microsoft.com/en-us/resources/samples/key-vault-node-getting-started/)

### <a name="code-examples"></a>程式碼範例

如需搭配使用金鑰保存庫和應用程式的完整範例，請參閱︰

- [Azure Key Vault 程式碼範例](http://www.microsoft.com/download/details.aspx?id=45343) - .NET 範例應用程式 HelloKeyVault 和 Azure Web 服務範例。 
- [從 Web 應用程式使用 Azure Key Vault ](key-vault-use-from-web-application.md) - 此教學課程可幫助您了解如何從 Azure 中的 Web 應用程式使用 Azure Key Vault。 

## <a name="how-tos"></a>作法

下列文章和案例提供使用 Azure Key Vault 的工作特定指引：

- [訂用帳戶動作之後變更金鑰保存庫租用戶識別碼](key-vault-subscription-move-fix.md) - 當您將 Azure 訂用帳戶從租用戶 A 移動至租用戶 B 時，租用戶 B 中的實體 (使用者和應用程式) 將無法存取您現有的金鑰保存庫。請利用本指南來解決此問題。
- [存取防火牆後面的金鑰保存庫](key-vault-access-behind-firewall.md)若要存取金鑰保存庫，您的金鑰保存庫用戶端應用程式必須能夠存取多個端點，才能使用各種功能。
- [如何為 Azure 金鑰保存庫產生並傳輸受 HSM 保護的金鑰](key-vault-hsm-protected-keys.md) - 這將協助您規劃、產生並傳輸專屬受 HSM 保護的金鑰，以搭配 Azure 金鑰保存庫使用。
- [如何在部署期間傳遞安全值 (例如密碼)](../azure-resource-manager/resource-manager-keyvault-parameter.md) - 當您需要在部署期間傳遞安全值 (例如密碼) 作為參數時，可以將該值儲存為 Azure 金鑰保存庫中的密碼，並在其他資源管理員範本中參考該值。
- [如何搭配使用金鑰保存庫與 SQL Server 進行可延伸金鑰管理](https://msdn.microsoft.com/library/dn198405.aspx) - 適用於 Azure 金鑰保存庫的 SQL Server 連接器會啟用 SQL Server 和 SQL-in-a-VM，利用 Azure 金鑰保存庫服務作為可延伸金鑰管理 (EKM) 提供者來保護其針對應用程式連結的加密金鑰；透明資料加密、備份加密和資料行層級加密。
- [如何將憑證從金鑰保存庫部署至 VM](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - 在 Azure 上的 VM 中執行的雲端應用程式需要憑證。 現在應如何讓此憑證進入此 VM？
- [如何使用端對端金鑰輪替和稽核設定金鑰保存庫](key-vault-key-rotation-log-monitoring.md) - 引導您逐步完成使用 Azure Key Vault 設定金鑰輪替和稽核的步驟。
- [透過金鑰保存庫部署 Azure Web 應用程式憑證]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)提供逐步指示，以便將儲存在金鑰保存庫的憑證部署為 [App Service 憑證](https://azure.microsoft.com/blog/internals-of-app-service-certificate/)供應項目的一部分。
- [對許多應用程式授與金鑰保存庫的存取權限](key-vault-group-permissions-for-apps.md) 金鑰保存庫存取控制原則只支援 16 個項目。 不過，您可以建立 Azure Active Directory 安全性群組。 將所有相關聯的服務主體新增至這個安全性群組，然後對 Key Vault 授與此安全性群組的存取權。

如需整合及搭配使用金鑰保存庫和 Azure 的具體工作指引，請參閱 [Ryan Jones 的金鑰保存庫 Azure Resource Manager 範本範例](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)。

## <a name="integrated-with-key-vault"></a>與金鑰保存庫整合

這些文章是關於其他可讓我們使用及整合 Key Vault 的案例和服務。

- [Azure 磁碟加密](../security/azure-security-disk-encryption.md)利用 Windows 的業界標準 [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) 功能和 Linux 的 [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) 功能，為 OS 和資料磁碟提供磁碟區加密。 此解決方案與 Azure 金鑰保存庫整合，可幫助您控制和管理您的金鑰保存庫訂用帳戶中的磁碟加密金鑰和密碼，同時確保虛擬機器磁碟中的所有資料會在您的 Azure 儲存體中輕鬆加密。
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) 會為帳戶中儲存的資料提供加密選項。 金鑰管理，如 Data Lake Store 會提供兩種模式用於管理您的主要加密金鑰 (MEK)，它是解密 Data Lake Store 中儲存任何資料所需。 您可以讓 Data Lake Store 管理 MEK，或使用 Azure 金鑰保存庫帳戶，選擇保留 MEK 的擁有權。 您會在建立 Data Lake Store 帳戶時指定金鑰管理的模式。 
- [Azure 資訊保護](/information-protection/plan-design/plan-implement-tenant-key)可讓您管理自己的租用戶金鑰。 例如，您可以管理自己的租用戶金鑰，以符合適用於貴組織的特定規範，而不需 Microsoft 管理您的租用戶金鑰 (預設值)。 管理自己的租用戶金鑰也稱為「自備金鑰」或 BYOK。

## <a name="key-vault-overviews-and-concepts"></a>Key Vault 的概觀和概念

- [Key Vault 安全世界](key-vault-ovw-security-worlds.md)
- [Key Vault 虛刪除](key-vault-ovw-soft-delete.md)

## <a name="social"></a>社交

- [Key Vault Blog (金鑰保存庫部落格)](http://aka.ms/kvblog)
- [Key Vault Forum (金鑰保存庫論壇)](http://aka.ms/kvforum)


## <a name="supporting-libraries"></a>支援程式庫

- [Microsoft Azure Key Vault 核心程式庫](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core)提供 **IKey** 和 **IKeyResolver** 介面，以從識別碼找出金鑰和使用金鑰執行作業。
- [Microsoft Azure 金鑰保存庫擴充](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions)提供 Azure 金鑰保存庫的擴充功能。

## <a name="other-key-vault-resources"></a>其他金鑰保存庫資源


