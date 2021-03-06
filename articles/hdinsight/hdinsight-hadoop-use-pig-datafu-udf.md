---
title: "在 HDInsight 上搭配使用 DataFu 與 Pig"
description: "DataFu 是搭配 Hadoop 使用的程式庫集合。 了解如何在 HDInsight 叢集上搭配使用 DataFu 與 Pig。"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 0016721a-82be-4773-88ad-91e6b2c21cbb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: b9dcb069003c647073c978feb588debb689d560e
ms.contentlocale: zh-tw
ms.lasthandoff: 05/18/2017


---
# <a name="use-datafu-with-pig-on-hdinsight"></a>在 HDInsight 上搭配使用 DataFu 與 Pig

了解如何搭配 HDInsight 使用 DataFu。 DataFu 是搭配 Hadoop 上的 Pig 使用的開放原始碼程式庫集合。

## <a name="prerequisites"></a>必要條件

* Azure 訂用帳戶。

* Azure HDInsight 叢集 (以 Linux 或 Windows 為基礎)

  > [!IMPORTANT]
  > Linux 是唯一使用於 HDInsight 3.4 版或更新版本的作業系統。 如需詳細資訊，請參閱 [Windows 上的 HDInsight 淘汰](hdinsight-component-versioning.md#hdi-version-33-nearing-retirement-date)。

* [在 HDInsight 使用 Pig](hdinsight-use-pig.md)

## <a name="install-datafu-on-linux-based-hdinsight"></a>在以 Linux 為基礎的 HDInsight 上安裝 DataFu

> [!NOTE]
> DataFu 是安裝在 Linux 架構的叢集 3.3 版和更高版本上，以及在 Windows 架構的叢集上。 不會安裝在比 3.3 版更早的 Linux 架構叢集上。
>
> 如果您使用 Linux 架構的叢集 3.3 版或更高版本，或是 Windows 架構的叢集，可以略過本節。

可以從 Maven 儲存機制下載並安裝 DataFu。 使用下列步驟將 DataFu 新增至您的 HDInsight 叢集：

1. 使用 SSH 連線至以 Linux 為基礎的 HDInsight 叢集： 如需詳細資訊，請參閱[搭配 HDInsight 使用 SSH](hdinsight-hadoop-linux-use-ssh-unix.md)。

2. 使用下列命令以 wget 公用程式下載 DataFu jar 檔案，或複製連結並在瀏覽器中貼上來開始下載。

    ```
    wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar
    ```

3. 接下來，將檔案上傳至 HDInsight 叢集的預設儲存體。 如果將檔案放在預設儲存體中，則叢集的所有節點都可使用此檔案。

    ```
    hdfs dfs -put datafu-1.2.0.jar /example/jars
    ```

    > [!NOTE]
    > 前一個命令會將 jar 儲存在 `/example/jars`，因為叢集儲存體上已經有此目錄。 您可以使用 HDInsight 叢集上任何想要的位置。

## <a name="use-datafu-with-pig"></a>搭配使用 DataFu 與 Pig

本節中的步驟假設您已熟悉如何在 HDInsight 上使用 Pig。 如需搭配使用 Pig 與 HDInsight 的詳細資訊，請參閱 [搭配使用 Pig 與 HDInsight](hdinsight-use-pig.md)。

> [!IMPORTANT]
> 在以 Linux 為基礎的 HDInsight 叢集上透過 Pig 使用 DataFu 時，您必須先註冊 jar 檔案。
>
> 如果叢集使用 Azure 儲存體，請使用 `wasb://` 路徑。 例如， `register wasb:///example/jars/datafu-1.2.0.jar`。
>
> 如果叢集使用 Azure Data Lake Store，請使用 `adl://` 路徑。 例如， `register adl://home/example/jars/datafu-1.2.0.jar`。
>
> 以 Windows 為基礎的 HDInsight 叢集已預設註冊 DataFu。

您通常會定義 DataFu 函式的別名。 例如：

    DEFINE SHA datafu.pig.hash.SHA();

此陳述式會為 SHA 雜湊函式定義名為 `SHA` 的別名。 您之後可以在 Pig Latin 指令碼中使用此別名來產生輸入資料的雜湊值。 例如，下列程式碼會使用雜湊值取代輸入資料中的名稱：

```piglatin
raw = LOAD '/data/raw/' USING PigStorage(',') AS  
    (name:chararray,
    int1:int,
    int2:int,
    int3:int);
mask = FOREACH raw GENERATE SHA(name), int1, int2, int3;
DUMP mask;
```

如果此程式碼搭配下列輸入資料一起使用：

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5

它會產生下列輸出：

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

## <a name="next-steps"></a>後續步驟

如需 DataFu 或 Pig 的詳細資訊，請參閱下列文件：

* [Apache DataFu Pig 指南](http://datafu.incubator.apache.org/docs/datafu/guide.html)(英文)。
* [搭配 HDInsight 使用 Pig](hdinsight-use-pig.md)

