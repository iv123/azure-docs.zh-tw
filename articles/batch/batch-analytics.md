---
title: "Azure Batch 分析 | Microsoft Docs"
description: "Azure Batch 分析的參考。"
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.lasthandoff: 04/22/2017

---

# <a name="batch-analytics"></a>批次分析
批次分析中的主題包含批次服務資源所適用事件和警示的參考資訊。

請參閱[記錄事件以便對 Batch 解決方案進行診斷評估和監視](https://azure.microsoft.com/documentation/articles/batch-diagnostics/)，以取得啟用與取用批次診斷記錄檔的詳細資訊。

## <a name="diagnostic-logs"></a>診斷記錄檔

Azure 批次服務會在某些批次資源的存留期間發出下列診斷記錄事件。

**服務記錄檔事件**
* [建立集區](batch-pool-create-event.md)
* [開始刪除集區](batch-pool-delete-start-event.md)
* [完成集區刪除](batch-pool-delete-complete-event.md)
* [開始調整集區大小](batch-pool-resize-start-event.md)
* [完成集區大小調整](batch-pool-resize-complete-event.md)
* [開始工作](batch-task-start-event.md)
* [完成工作](batch-task-complete-event.md)
* [工作失敗](batch-task-fail-event.md)
