---
title: "使用 Azure 入口網站開始使用 Azure Data Lake Analytics | Microsoft Docs"
description: "了解如何使用 Azure 入口網站建立 Data Lake Analytics 帳戶、使用 U-SQL 建立 Data Lake Analytics 作業，以及提交作業。 "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: eb85d8ef6b29605d7e26b0d2139a4a95c35141fb
ms.contentlocale: zh-tw
ms.lasthandoff: 06/01/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-portal"></a>教學課程：使用 Azure 入口網站開始使用 Azure Data Lake Analytics
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

了解如何使用 Azure 入口網站建立 Azure Data Lake Analytics 帳戶、在 [U-SQL](data-lake-analytics-u-sql-get-started.md)中定義作業，以及將作業提交至 Data Lake Analytics 服務。 如需有關 Data Lake Analytics 的詳細資訊，請參閱 [Azure Data Lake Analytics 概觀](data-lake-analytics-overview.md)。

## <a name="prerequisites"></a>必要條件

開始進行本教學課程之前，您必須擁有 **Azure 訂用帳戶**。 請參閱 [取得 Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="create-data-lake-analytics-account"></a>建立 Data Lake Analytics 帳戶

現在，您會同時建立 Data Lake Analytics 和 Data Lake Store 帳戶。  這個步驟很簡單，只需要大約 60 即可完成。

1. 登入 [Azure 入口網站](https://portal.azure.com)。
2. 按一下 [新增] >  [智慧 + 分析] > [Data Lake Analytics]。
3. 選取下列項目的值︰
   * **名稱**：替 Data Lake Analytics 帳戶命名 (只允許小寫字母和數字)。
   * **訂用帳戶**：選擇用於分析帳戶的 Azure 訂用帳戶。
   * **資源群組**。 選取現有的 Azure 資源群組，或建立一個新的群組。
   * **位置**。 為 Data Lake Analytics 帳戶選取 Azure 資料中心。
   * **Data Lake Store**：請依照指示來建立新的 Data Lake Store 帳戶，或選取現有的帳戶。 
4. (選擇性) 選取 Data Lake Analytics 帳戶的定價層。
5. 按一下 [建立] 。 

## <a name="create-and-submit-data-lake-analytics-jobs"></a>建立並提交 Data Lake Analytics 作業
在您備妥來源資料後，您可以開始開發 U SQL 指令碼。  

**提交作業**

1. 從 Data Lake Analytics 帳戶中，按一下 [新增作業]。
2. 輸入 [作業名稱] 與下列 U-SQL 指令碼：

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog   
    TO "/Output/SearchLog-from-Data-Lake.csv"
    USING Outputters.Csv();
```


此 U-SQL 指令碼會使用 **Extractors.Tsv()** 讀取來源資料檔案，然後使用 **Outputters.Csv()** 建立 csv 檔案。

1. 按一下 [ **提交作業**]。   
2. 請等待直到工作狀態變為 [成功] 。
3. 如果作業失敗，請參閱[監視和移難排解 Data Lake Analytics 作業](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)。
4. 按一下 [輸出] 索引標籤，然後按一下 `SearchLog-from-Data-Lake.csv`。 

## <a name="see-also"></a>另請參閱

* 若要開始開發 U-SQL 應用程式，請參閱 [使用適用於 Visual Studio 的 Data Lake 工具開發 U-SQL 指令碼](data-lake-analytics-data-lake-tools-get-started.md)。
* 若要了解 U-SQL，請參閱 [開始使用 Azure Data Lake Analytics U-SQL 語言](data-lake-analytics-u-sql-get-started.md)。
* 針對管理工作，請參閱 [使用 Azure 入口網站管理 Azure Data Lake Analytics](data-lake-analytics-manage-use-portal.md)。

