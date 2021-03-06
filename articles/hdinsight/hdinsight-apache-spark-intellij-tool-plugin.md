---
title: "適用於 IntelliJ 的 Azure 工具組 - 建立適用於 HDInsight Spark 的 Scala 應用程式 | Microsoft Docs"
description: "使用適用於 IntelliJ 的 Azure 工具組來開發以 Scala 撰寫的 Spark 應用程式，並將它們提交到 HDInsight Spark 叢集。"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: e6e005e07b35bbaffebf1efa216feebafc99df8b
ms.contentlocale: zh-tw
ms.lasthandoff: 05/17/2017


---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-hdinsight-cluster"></a>使用 IntelliJ 的 Azure 工具組建立適用於 HDInsight 叢集的 Spark 應用程式

使用適用於 IntelliJ 外掛程式的 Azure 工具組來開發以 Scala 撰寫的 Spark 應用程式，並直接從 IntelliJ IDE 將它們提交到 HDInsight Spark 叢集。 您可以利用數個不同的方式來使用此外掛程式：

* 在 HDInsight Spark 叢集上開發並提交 Scala Spark 應用程式
* 存取您的 Azure HDInsight Spark 叢集資源
* 在本機開發並執行 Scala Spark 應用程式

您也可以遵循[影片](https://mix.office.com/watch/1nqkqjt5xonza)來協助您開始。

> [!IMPORTANT]
> 此外掛程式可用來僅針對 Linux 上的 HDInsight Spark 叢集建立並提交應用程式。
> 
> 

## <a name="prerequisites"></a>必要條件

* HDInsight Linux 上的 Apache Spark 叢集。 如需指示，請參閱 [在 Azure HDInsight 中建立 Apache Spark 叢集](hdinsight-apache-spark-jupyter-spark-sql.md)。

* Oracle Java Development Kit。 您可以從 [Oracle 網站](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下載。

* IntelliJ IDEA。 本文章使用 15.0.1 版。 您可以從 [JetBrains 網站](https://www.jetbrains.com/idea/download/)下載。

## <a name="install-azure-toolkit-for-intellij"></a>安裝適用於 IntelliJ 的 Azure 工具組
如需安裝指示，請參閱[安裝適用於 IntelliJ 的 Azure 工具組](../azure-toolkit-for-intellij-installation.md)。

## <a name="sign-in-to-your-azure-subscription"></a>登入您的 Azure 訂用帳戶：
1. 啟動 IntelliJ IDE，然後開啟 [Azure Explorer]。 在 [檢視] 功能表上，按一下 [工具視窗]，然後按一下 [Azure Explorer]。
   
    ![[檢視] 功能表上選取的命令](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)
2. 以滑鼠右鍵按一下 [Azure] 節點，然後按一下 [登入]。
3. 在 [Azure 登入] 對話方塊中，按一下 [登入] 並輸入您的 Azure 認證。
   
    ![[Azure 登入] 對話方塊](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)
4. 登入之後，[選取訂用帳戶] 對話方塊會列出與認證相關聯的所有 Azure 訂用帳戶。 按一下 [選取] 以關閉對話方塊。

    ![[選取訂用帳戶] 對話方塊](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)
5. 在 [Azure Explorer] 索引標籤中展開 [HDInsight]，可查看您訂用帳戶下的 HDInsight Spark 叢集。
   
    ![Azure Explorer 中的 HDInsight Spark 叢集](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)
6. 您可以進一步展開叢集名稱節點，查看與叢集相關聯的資源 (例如，儲存體帳戶)。
   
    ![展開叢集名稱以查看資源](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>在 HDInsight Spark 叢集上執行 Spark Scala 應用程式
1. 啟動 IntelliJ IDEA 並建立專案。 在 [新增專案] 對話方塊中選取下列選項，然後按 [下一步]。
   
   * 在左窗格中，選取 [HDInsight]。
   * 在右窗格中，選取 [HDInsight 上的 Spark (Scala)]。
   
    ![New Project dialog box](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)
2. 在下一個視窗中，提供下列專案詳細資料，然後按一下 [完成]。
   
   * 提供專案名稱和專案位置。
   * 針對**專案 SDK**，針對 Spark 1.6 版或 Spark 2.0 叢集使用 Java 1.8。
   * 對於 **Scala SDK**，請依序按一下 [建立]、[下載]，然後選取要使用的 Scala 版本。
     * 如果您要將作業提交至 Spark 2.0 叢集，請選擇 **JDK 1.8 和 Scala 2.11.x**。
     * 如果您要將作業提交至 Spark 1.6 叢集，請選擇 **JDK 1.8 (語言層級 7) 和 Scala 2.10.x**。

        ![選取 Scala 版本](./media/hdinsight-apache-spark-intellij-tool-plugin/show-scala2.11.x-select.png)
   * 針對 **Spark SDK**，請下載並使用 [GitHub] 的 SDK (http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409)。 請注意，spark-assembly-2.0.0-hadoop2.7.0-SNAPSHOT.jar 適用於 Spark 2.0 叢集，spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar 則是適用於 Spark 1.6 叢集。 

     您可以改用 [Spark Maven 存放庫](http://mvnrepository.com/search?q=spark)，不過請確定您已安裝正確的 Maven 存放庫，才能開發 Spark 應用程式。 例如，假設您使用 Spark 串流，請確定已安裝 Spark 串流組件。 另外，請確定您針對 Spark 1.6 叢集使用標示為 Scala 2.10 的存放庫，以及針對 Spark 2.0 叢集使用標示為 Scala 2.11 的存放庫。
     
       ![選取 Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-project-details.png)
3. Spark 專案會自動為您建立構件。 若要查看構件，請遵循下列步驟：
   
   1. 在 [檔案] 功能表上，按一下 [專案結構]。
   2. 在 [專案結構] 對話方塊中，按一下 [構件]，以查看建立的預設構件。 您也可以建立自己的構件，方法是按一下 [+] 圖示。
      
       ![對話方塊中的構件資訊](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
4. 在 [專案結構] 對話方塊中，按一下 [專案]。 如果將 [專案 SDK] 設定為 1.8，請確定將 [專案語言層級] 設為 [7 - Diamonds、ARM、Multi-Catch 等]。(它對於 Spark 2.0 叢集是選擇性的。)
   
    ![設定專案語言層級](./media/hdinsight-apache-spark-intellij-tool-plugin/set-project-language-level.png)
5. 新增應用程式的原始程式碼。
   
   1. 在 [專案總管] 中，以滑鼠右鍵按一下 [src]、指向 [新增]，然後按一下 [Scala 類別]。
      
       ![從 [專案總管] 中建立 Scala 類別的命令](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)
   2. 在 [建立新的 Scala 類別] 對話方塊中，提供一個名稱，並在 [種類] 方塊中選取 [物件]，然後按一下 [確定]。
      
       ![建立新的 Scala 類別對話方塊](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)
   3. 在 **MyClusterApp.scala** 檔案中，貼入下列程式碼。 此程式碼會從 HVAC.csv (所有 HDInsight Spark 叢集上均提供) 讀取資料、擷取在 CSV 檔案的第七個資料行中只有一個位數的資料列，並將輸出寫入叢集預設儲存體容器下的 **/HVACOut** 。

            import org.apache.spark.SparkConf
            import org.apache.spark.SparkContext

            object MyClusterApp{
              def main (arg: Array[String]): Unit = {
                val conf = new SparkConf().setAppName("MyClusterApp")
                val sc = new SparkContext(conf)

                val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

                //find the rows that have only one digit in the seventh column in the CSV file
                val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

                rdd1.saveAsTextFile("wasbs:///HVACOut")
              }

            }

6. 在 HDInsight Spark 叢集上執行應用程式。
   
   1. 在 [專案總管] 中，以滑鼠右鍵按一下專案名稱，然後選取 [將 Spark 應用程式提交給 HDInsight]。
      
       ![選取 [將 Spark 應用程式提交給 HDInsight]](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)
   2. 系統會提示您輸入 Azure 訂用帳戶認證。 在 [提交 Spark] 對話方塊中，提供下列值，然後按一下 [提交]。
      
      * 針對 [Spark clusters (Linux only) (Spark 叢集 (僅限 Linux))] ，選取您要在其上執行應用程式的 HDInsight Spark 叢集。
      * 從 IntelliJ 專案中選取構件，或從硬碟中選取一個。
      * 在 [主要類別名稱] 文字方塊中，按一下省略符號 (![ellipsis](./media/hdinsight-apache-spark-intellij-tool-plugin/ellipsis.png))，並在應用程式的原始程式碼中選取主要類別，然後按一下 [確定]。
        
          ![選取 [主要類別] 對話方塊](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)
      * 此範例中的應用程式程式碼不需要任何命令列引數或是參考 JAR 或檔案，因此您可以將其餘的方塊保留空白。
        提供所有輸入之後，對話方塊應該如下圖所示。
        
          ![[提交 Spark] 對話方塊](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)
   3. 視窗底部的 [Spark Submission (提交 Spark)]  索引標籤應會開始顯示進度。 您也可以按一下 [提交 Spark] 視窗中的紅色按鈕，即可將應用程式停止。
      
       ![[提交 Spark] 視窗](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      在本文中稍後的＜使用適用於 IntelliJ 的 Azure 工具組來存取和管理 HDInsight Spark 叢集＞一節中，您會了解如何存取作業輸出。

## <a name="choose-azure-data-lake-store-as-spark-scala-application-storage"></a>選擇 Azure Data Lake Store 作為 Spark Scala 應用程式儲存體
如果您想要將應用程式提交至 Azure Data Lake Store，則在 Azure 登入程序期間，必須選擇 [互動式] 模式。 

   ![登入時的互動式選項](./media/hdinsight-apache-spark-intellij-tool-plugin/authentication-interactive.png)

如果您選取 [自動] 模式，會發生下列錯誤：

   ![登入錯誤](./media/hdinsight-apache-spark-intellij-tool-plugin/authentication-error.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>使用適用於 IntelliJ 的 Azure 工具組來存取和管理 HDInsight Spark 叢集
您可以使用適用於 IntelliJ 的 Azure 工具組來執行各種作業。

### <a name="access-the-job-view"></a>存取作業檢視
1. 在 [Azure Explorer] 中，依序展開 [HDInsight] 和 Spark 叢集名稱，然後按一下 [作業]。
2. 在右窗格中，[Spark 作業檢視]  索引標籤會顯示已在叢集上執行的所有應用程式。 按一下您想要查看更多詳細資料的應用程式名稱。
   
    ![應用程式詳細資料](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

[錯誤]、[驅動程式記錄]、[輸出]、[Livy 記錄] 等方塊會以您所選取的應用程式作為基礎加以填入。

您也可以按一下視窗頂端的個別按鈕，開啟 [Spark 歷程記錄 UI] 和 [YARN UI] (應用程式層級)。

### <a name="access-the-spark-history-server"></a>存取 Spark 歷程記錄伺服器
1. 在 [Azure Explorer] 中，展開 [HDInsight]、以滑鼠右鍵按一下您的 Spark 叢集名稱，然後選取 [開啟 Spark 歷程記錄 UI]。 出現提示時，輸入叢集的系統管理員認證。 在佈建叢集時，您必須已指定這些項目。
2. 在 [Spark 歷程記錄伺服器] 儀表板中，您可以使用應用程式名稱，尋找您剛完成執行的應用程式。 在上述程式碼中，您可以使用 `val conf = new SparkConf().setAppName("MyClusterApp")`來設定應用程式名稱。 因此，Spark 應用程式名稱為 **MyClusterApp**。

### <a name="start-the-ambari-portal"></a>啟動 Ambari 入口網站
1. 在 [Azure Explorer] 中，展開 [HDInsight]、以滑鼠右鍵按一下您的 Spark 叢集名稱，然後選取 [開啟叢集管理入口網站 (Ambari)]。 
2. 出現提示時，輸入叢集的系統管理員認證。 您在叢集佈建程序期間已指定這些認證。

### <a name="manage-azure-subscriptions"></a>管理 Azure 訂用帳戶
根據預設，適用於 IntelliJ 的 Azure 工具組會列出您所有 Azure 訂用帳戶中的 Spark 叢集。 如有必要，您可以指定要存取其叢集的訂用帳戶。 

1. 在 [Azure Explorer] 中，以滑鼠右鍵按一下 [Azure] 根節點，然後按一下 [管理訂用帳戶]。 
2. 在對話方塊中，清除您不想要存取的訂用帳戶核取方塊，然後按一下 [關閉]。 如果您想要登出 Azure 訂用帳戶，也可以按一下 [登出]。

## <a name="run-a-spark-scala-application-locally"></a>在本機執行 Spark Scala 應用程式
您可以使用適用於 IntelliJ 的 Azure 工具組，在工作站上本機執行 Spark Scala 應用程式。 一般而言，這些應用程式不需要存取叢集資源 (例如儲存體容器)，而且您可在本機上執行並測試。

### <a name="prerequisite"></a>必要條件
在 Windows 電腦上執行本機 Spark Scala 應用程式時，可能會發生如 [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) 中所述的例外狀況。 發生這個例外狀況是因為 Windows 上遺失 WinUtils.exe。 

若要解決這個錯誤，您必須[下載可執行檔](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe)，並將其放在 **C:\WinUtils\bin** 之類的位置。 然後，新增環境變數 **HADOOP_HOME**，並將變數的值設為 **C\WinUtils**。

### <a name="run-a-local-spark-scala-application"></a>執行本機 Spark Scala 應用程式
1. 啟動 IntelliJ IDEA 並建立專案。 在 [新增專案] 對話方塊中選取下列選項，然後按 [下一步]。
   
   * 在左窗格中，選取 [HDInsight]。
   * 在右窗格中，選取 [HDInsight 上的 Spark 本機執行範例 (Scala)]。
   
    ![[新增專案] 對話方塊中的選取項目](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)
2. 在下一個視窗中，提供下列專案詳細資料，然後按一下 [完成]。
   
   * 提供專案名稱和專案位置。
   * 對於 [專案 SDK]，請確定您提供的是 7 以上的 Java 版本。
   * 對於 [Scala SDK]，請依序按一下 [建立]、[下載]，然後選取要使用的 Scala 版本：Scala 2.11.x 適用於 Spark 2.0，Scala 2.10.x 適用於 Spark 1.6。
     
       ![選取 Scala 版本](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-scala-version.png)
   * 對於 **Spark SDK**，請從 [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) 下載並使用 SDK。 您可以改用 [Spark Maven 存放庫](http://mvnrepository.com/search?q=spark)，不過請確定您已安裝正確的 Maven 存放庫，才能開發 Spark 應用程式。 例如，假設您使用 Spark 串流，請確定已安裝 Spark 串流組件。 另外，請確定您針對 Spark 1.6 叢集使用標示為 Scala 2.10 的存放庫，以及針對 Spark 2.0 叢集使用標示為 Scala 2.11 的存放庫。
     
       ![選取 Spark SDK](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-create-project.png)
3. 範本會在 **src** 資料夾下方新增可在電腦本機執行的程式碼範例 (**LogQuery**)。
   
    ![LogQuery 的位置](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)
4. 以滑鼠右鍵按一下 [LogQuery] 應用程式，然後按一下 [執行 'LogQuery']。 您會在底部的 [執行] 索引標籤上看到如下的輸出。
   
   ![Spark 應用程式本機執行結果](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>轉換現有的 IntelliJ IDEA 應用程式，來使用適用於 IntelliJ 的 Azure 工具組
您可以將 IntelliJ IDEA 中建立的現有 Spark Scala 應用程式轉換成與適用於 IntelliJ 的 Azure 工具組相容。 然後您可以使用外掛程式，將應用程式提交給 HDInsight Spark 叢集。

1. 對於透過 IntelliJ IDEA 建立的現有 Spark Scala 應用程式，請開啟相關聯的 .iml 檔案。
2. 在根層級中，您會看到如下的 **module** 元素：
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">
   編輯該元素以加入 `UniqueKey="HDInsightTool"` ，使 **module** 元素看起來如下所示：
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">
3. 儲存變更。 您的應用程式現在應該可與適用於 IntelliJ 的 Azure 工具組相容。 您可以滑鼠右鍵按一下 [專案總管] 中的專案名稱來進行測試。 現在，快顯功能表提供 [將 Spark 應用程式提交給 HDInsight] 的選項。

## <a name="troubleshooting"></a>疑難排解
### <a name="please-use-a-larger-heap-size-error-in-local-run"></a>本機執行發生「請使用較大的堆積大小」錯誤
在 Spark 1.6 中，如果您在本機執行期間是使用 32 位元的 Java SDK，可能會遇到下列錯誤︰

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

會發生這些錯誤是因為堆積大小不足，使 Spark 無法執行。 (Spark 需要至少 471 MB。 您可以從 [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081) 取得更多詳細資料)。 其中一個簡單的解決方案就是使用 64 位元的 Java SDK。 您也可以新增下列選項來變更 IntelliJ 中的 JVM 設定：

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![IntelliJ 中的將選項新增至 [VM 選項] 方塊](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="feedback-and-known-issues"></a>意見反應和已知問題
目前，不支援直接檢視 Spark 輸出。

如果您有任何建議或意見反應，或使用此外掛程式時遇到任何問題，請將電子郵件傳送到 hdivstool@microsoft.com。

## <a name="seealso"></a>另請參閱
* [概觀：Azure HDInsight 上的 Apache Spark](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>案例
* [Spark 和 BI：在 HDInsight 中搭配使用 Spark 和 BI 工具執行互動式資料分析](hdinsight-apache-spark-use-bi-tools.md)
* [Spark 和機器學習服務：使用 HDInsight 中的 Spark，利用 HVAC 資料來分析建築物溫度](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark 和機器學習服務：使用 HDInsight 中的 Spark 來預測食品檢查結果](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark 串流：使用 HDInsight 中的 Spark 來建置即時串流應用程式](hdinsight-apache-spark-eventhub-streaming.md)
* [使用 HDInsight 中的 Spark 進行網站記錄分析](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>建立和執行應用程式
* [使用 Scala 建立獨立應用程式](hdinsight-apache-spark-create-standalone-application.md)
* [利用 Livy 在 Spark 叢集上遠端執行作業](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>工具和擴充功能
* [使用適用於 IntelliJ 的 Azure 工具組來遠端偵錯 Spark 應用程式](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [使用適用於 Eclipse 的 Azure 工具組中的 HDInsight 工具建立 Spark 應用程式](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [利用 HDInsight 上的 Spark 叢集來使用 Zeppelin Notebook](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [HDInsight 的 Spark 叢集中 Jupyter Notebook 可用的核心](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [搭配 Jupyter Notebook 使用外部套件](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [在電腦上安裝 Jupyter 並連接到 HDInsight Spark 叢集](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>管理資源
* [在 Azure HDInsight 中管理 Apache Spark 叢集的資源](hdinsight-apache-spark-resource-manager.md)
* [追蹤和偵錯在 HDInsight 中的 Apache Spark 叢集上執行的作業](hdinsight-apache-spark-job-debugging.md)


