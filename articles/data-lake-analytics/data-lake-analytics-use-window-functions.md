---
title: "針對 Azure Data Lake Analytics 作業使用 U-SQL 視窗函式 | Microsoft Docs"
description: "了解如何使用 U-SQL 視窗函式。 "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: a5e14b32-d5eb-4f4b-9258-e257359f9988
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data`
ms.date: 12/05/2016
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 55d19a00198f1943a8196d31399c617397b4e5d2
ms.contentlocale: zh-tw
ms.lasthandoff: 05/05/2017


---
# <a name="using-u-sql-window-functions-for-azure-data-lake-analytics-jobs"></a>針對 Azure Data Lake Analytics 工作使用 U-SQL 視窗函式
視窗函式是在 2003 年的 ISO/ANSI SQL Standard 中導入的。 U-SQL 採用 ANSI SQL Standard 所定義之視窗函式的子集。

視窗函式可用來進行名為 *視窗*之資料列集內的計算。 視窗由 OVER 子句所定義。 視窗函式可高效率地解決一些重要的案例。

視窗函式可分類為： 

* [報告彙總函式](#reporting-aggregation-functions)，如 SUM 或 AVG
* [排名函式](#ranking-functions)，例如 DENSE_RANK、ROW_NUMBER、NTILE 和 RANK
* [分析函式](#analytic-functions)，例如累加分配或百分位數，從相同結果集中的前一個資料列存取資料，而不使用自我聯結

## <a name="sample-datasets"></a>範例資料集
本教學課程使用兩個資料集：

### <a name="the-querylog-sample-dataset"></a>QueryLog 範例資料集
  
QueryLog 代表人員在搜尋引擎中搜尋的項目清單。 每個查詢記錄檔包含：
  
* Query - 使用者所搜尋的資料
* Latency - 查詢返回至使用者的時間，以毫秒為單位
* Vertical - 使用者有興趣的內容種類 (Web 連結、影像、影片)。  
 
```
@querylog = 
    SELECT * FROM ( VALUES
        ("Banana"  , 300, "Image" ),
        ("Cherry"  , 300, "Image" ),
        ("Durian"  , 500, "Image" ),
        ("Apple"   , 100, "Web"   ),
        ("Fig"     , 200, "Web"   ),
        ("Papaya"  , 200, "Web"   ),
        ("Avocado" , 300, "Web"   ),
        ("Cherry"  , 400, "Web"   ),
        ("Durian"  , 500, "Web"   ) )
    AS T(Query,Latency,Vertical);
```

## <a name="the-employees-sample-dataset"></a>員工範例資料集
  
員工資料集包含下列欄位：
  
* EmpID - 員工識別碼
* EmpName - 員工名稱
* DeptName - 部門名稱 
* DeptID - 部門識別碼
* Salary - 員工薪資

```
@employees = 
    SELECT * FROM ( VALUES
        (1, "Noah",   "Engineering", 100, 10000),
        (2, "Sophia", "Engineering", 100, 20000),
        (3, "Liam",   "Engineering", 100, 30000),
        (4, "Emma",   "HR",          200, 10000),
        (5, "Jacob",  "HR",          200, 10000),
        (6, "Olivia", "HR",          200, 10000),
        (7, "Mason",  "Executive",   300, 50000),
        (8, "Ava",    "Marketing",   400, 15000),
        (9, "Ethan",  "Marketing",   400, 10000) )
    AS T(EmpID, EmpName, DeptName, DeptID, Salary);
```  

## <a name="compare-window-functions-to-grouping"></a>比較視窗函式與分組
「視窗化」與「分組」在概念上相關。 了解此關聯性將有所幫助。

### <a name="use-aggregation-and-grouping"></a>使用彙總與分組
下列查詢會使用彙總來計算所有員工的薪資總和：

    @result = 
        SELECT 
            SUM(Salary) AS TotalSalary
        FROM @employees;

結果是具有單一資料行的單一資料列。 $165000 是整份資料表中 [薪資] 值的總和。 

| TotalSalary |
| --- |
| 165000 |


下列陳述式使用 GROUP BY 子句來計算每個部門的薪資總計：

    @result=
        SELECT DeptName, SUM(Salary) AS SalaryByDept
        FROM @employees
        GROUP BY DeptName;

結果為：

| DeptName | SalaryByDept |
| --- | --- |
| Engineering |60000 |
| HR |30000 |
| [行政人員] |50000 |
| 行銷 |25000 |

SalaryByDept 資料行的總和為 $165000，符合最後一個指令碼中的金額。

在這兩個案例中，輸出資料列的數字都少於輸入資料列：

* 未使用 GROUP BY 時，彙總會將所有資料列摺疊成單一資料列。 
* 使用 GROUP BY 時會有 N 個輸出資料列，其中 N 是出現在資料中的相異值。  在此情況下，輸出中有 4 個資料列。

### <a name="use-a-window-function"></a>使用視窗函式
下列範例中的 OVER 子句是空的，所以視窗包含所有資料列。 在此範例中的 SUM 會套用到它前面的 OVER 子句。

您可以將此查詢解讀為：「所有資料列視窗內的薪資總和」。

    @result=
        SELECT
            EmpName,
            SUM(Salary) OVER( ) AS SalaryAllDepts
        FROM @employees;

不同於 GROUP BY，輸出資料列的數量會與輸入資料列一樣多： 

| EmpName | TotalAllDepts |
| --- | --- |
| Noah |165000 |
| Sophia |165000 |
| Liam |165000 |
| Emma |165000 |
| Jacob |165000 |
| Olivia |165000 |
| Mason |165000 |
| Ava |165000 |
| Ethan |165000 |

值 165000 (所有薪資的總計) 會放在每個輸出資料列中。 該總計來自於所有資料列的「視窗」，因此其中包含所有的薪資。 

下一個範例示範如何讓「視窗」更完善，以列出所有員工、部門，以及部門的薪資總計。 PARTITION BY 會新增到 OVER 子句中。

    @result=
    SELECT
        EmpName, DeptName,
        SUM(Salary) OVER( PARTITION BY DeptName ) AS SalaryByDept
    FROM @employees;

結果為：

| EmpName | DeptName | SalaryByDep |
| --- | --- | --- |
| Noah |Engineering |60000 |
| Sophia |Engineering |60000 |
| Liam |Engineering |60000 |
| Mason |[行政人員] |50000 |
| Emma |HR |30000 |
| Jacob |HR |30000 |
| Olivia |HR |30000 |
| Ava |行銷 |25000 |
| Ethan |行銷 |25000 |

同樣地，輸入資料列與輸出資料列的數量是相同的。 不過，每個資料列都有對應部門的薪資總計。

## <a name="reporting-aggregation-functions"></a>報告彙總函式
視窗函式也支援下列彙總：

* COUNT
* SUM
* 最小值
* 最大值
* 平均
* STDEV
* VAR

語法：

    <AggregateFunction>( [DISTINCT] <expression>) [<OVER_clause>]

注意： 

* 根據預設，彙總函式 (COUNT 除外) 會忽略 null 值。
* 當彙總函式與 OVER 子句一起指定時，將不允許在 OVER 子句中使用 ORDER BY 子句。

### <a name="use-sum"></a>使用 SUM
下列範例讓每個輸入資料列新增各部門的薪資總計：

    @result=
        SELECT 
            *,
            SUM(Salary) OVER( PARTITION BY DeptName ) AS TotalByDept
        FROM @employees;

輸出如下：

| EmpID | EmpName | DeptName | DeptID | Salary | TotalByDept |
| --- | --- | --- | --- | --- | --- |
| 1 |Noah |Engineering |100 |10000 |60000 |
| 2 |Sophia |Engineering |100 |20000 |60000 |
| 3 |Liam |Engineering |100 |30000 |60000 |
| 7 |Mason |[行政人員] |300 |50000 |50000 |
| 4 |Emma |HR |200 |10000 |30000 |
| 5 |Jacob |HR |200 |10000 |30000 |
| 6 |Olivia |HR |200 |10000 |30000 |
| 8 |Ava |行銷 |400 |15000 |25000 |
| 9 |Ethan |行銷 |400 |10000 |25000 |

### <a name="use-count"></a>使用 COUNT
下列範例會將額外的欄位新增至每個資料列，以顯示每個部門的員工總數。

    @result =
        SELECT *, 
            COUNT(*) OVER(PARTITION BY DeptName) AS CountByDept 
        FROM @employees;

結果：

| EmpID | EmpName | DeptName | DeptID | Salary | CountByDept |
| --- | --- | --- | --- | --- | --- |
| 1 |Noah |Engineering |100 |10000 |3 |
| 2 |Sophia |Engineering |100 |20000 |3 |
| 3 |Liam |Engineering |100 |30000 |3 |
| 7 |Mason |[行政人員] |300 |50000 |1 |
| 4 |Emma |HR |200 |10000 |3 |
| 5 |Jacob |HR |200 |10000 |3 |
| 6 |Olivia |HR |200 |10000 |3 |
| 8 |Ava |行銷 |400 |15000 |2 |
| 9 |Ethan |行銷 |400 |10000 |2 |

### <a name="use-min-and-max"></a>使用 MIN 和 MAX
下列範例會將額外的欄位新增至每個資料列，以顯示每個部門的最低薪資：

    @result =
        SELECT 
            *,
            MIN(Salary) OVER( PARTITION BY DeptName ) AS MinSalary
        FROM @employees;

結果：

| EmpID | EmpName | DeptName | DeptID | Salary | MinSalary |
| --- | --- | --- | --- | --- | --- |
| 1 |Noah |Engineering |100 |10000 |10000 |
| 2 |Sophia |Engineering |100 |20000 |10000 |
| 3 |Liam |Engineering |100 |30000 |10000 |
| 7 |Mason |[行政人員] |300 |50000 |50000 |
| 4 |Emma |HR |200 |10000 |10000 |
| 5 |Jacob |HR |200 |10000 |10000 |
| 6 |Olivia |HR |200 |10000 |10000 |
| 8 |Ava |行銷 |400 |15000 |10000 |
| 9 |Ethan |行銷 |400 |10000 |10000 |

## <a name="ranking-functions"></a>排名函式
排名函式會依照 PARTITION BY 和 OVER 子句的定義，針對每個分割中的每個資料列傳回排名值 (LONG)。 排名的順序由 OVER 子句中的 ORDER BY 所控制。

支援的排名函式如下：

* RANK
* DENSE_RANK 
* NTILE
* ROW_NUMBER

**語法：**

    [ RANK() | DENSE_RANK() | ROW_NUMBER() | NTILE(<numgroups>) ]
        OVER (
            [PARTITION BY <identifier, > …[n]]
            [ORDER BY <identifier, > …[n] [ASC|DESC]] 
    ) AS <alias>

* ORDER BY 子句對於排名函式是選擇性的。 如果未指定 ORDER BY，則 U-SQL 會根據它讀取記錄的順序來指派值，以致產生 ROW_NUMBER、RANK 或 DENSE_RANK 不具決定性的值。
* NTILE 需要評估為正整數的運算式。 此數字會指定每個分割必須劃分成的群組數目。 此識別碼只會與 NTILE 排名函式一起使用。 

如需 OVER 子句的詳細資訊，請參閱 [U-SQL 參考](http://go.microsoft.com/fwlink/p/?LinkId=691348)。

ROW_NUMBER、RANK 和 DENSE_RANK 都會指派數字給視窗中的資料列。 與其個別加以檢視，查看它們如何回應相同的輸入，會更加一目瞭然。

    @result =
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank, 
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank 
    FROM @querylog;

請注意，OVER 子句都相同。 結果：

| 查詢 | 延遲：INT | Vertical | RowNumber | RANK | DenseRank |
| --- | --- | --- | --- | --- | --- |
| Banana |300 |映像 |1 |1 |1 |
| Cherry |300 |映像 |2 |1 |1 |
| Durian |500 |映像 |3 |3 |2 |
| Apple |100 |Web |1 |1 |1 |
| Fig |200 |Web |2 |2 |2 |
| Papaya |200 |Web |3 |2 |2 |
| Fig |300 |Web |4 |4 |3 |
| Cherry |400 |Web |5 |5 |4 |
| Durian |500 |Web |6 |6 |5 |

### <a name="rownumber"></a>ROW_NUMBER
在每個視窗內 (Vertical，無論是 Image 或 Web)，依 Latency 排序的資料列號碼會漸次加 1。  

![U-SQL 視窗函式 ROW_NUMBER](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-row-number-result.png)

### <a name="rank"></a>RANK
不同於 ROW_NUMBER()，RANK() 會使用在視窗的 ORDER BY 子句中所指定的 Latency 值。

RANK 從 (1、1、3) 開始，因為 Latency 的頭兩個值是相同的。 下一個值是 3，因為 Latency 值已到達 500。 關鍵點在於，即使重複的值指定了相同的排名，RANK 數值仍會跳至下一個 ROW_NUMBER 值。 您可以在 Web 垂直中看到這種模式以 (2、2、4) 的序列重複。

![U-SQL 視窗函式 RANK](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-rank-result.png)

### <a name="denserank"></a>DENSE_RANK
DENSE_RANK 類似於 RANK，唯一差別在於它不會跳至下一個 ROW_NUMBER。 DENSE_RANK 會移至序列中的下一個數字。 請注意範例中的序列 (1、1、2) 和 (2、2、3)。

![U-SQL 視窗函式 DENSE_RANK](./media/data-lake-analytics-use-windowing-functions/u-sql-windowing-function-dense-rank-result.png)

### <a name="remarks"></a>備註
* 如果未指定 ORDER BY，則會將排名函式套用到資料列集 (不含任何排序)，以致產生不具決定性的行為。
* 下列條件必須成立，才能保證查詢使用 ROW_NUMBER 所傳回的資料列在每次執行時會有相同的排序。
  
  * 分割資料行的值是唯一的。
  * ORDER BY 資料行的值是唯一的。
  * 分割資料行和 ORDER BY 資料行的值組合是唯一的。

### <a name="ntile"></a>NTILE
NTILE 會將已排序分割中的資料列分配到指定數目的群組中。 群組會編號，從 1 開始。 

下列範例會將每個分割中的資料列集 (垂直) 分割為四個群組 (依延遲排序)，並傳回每個資料列的群組號碼。 

Image 垂直有三個資料列，所以有三個群組。 

Web 垂直有六個資料列。  兩個額外的資料列會分配到前兩個群組。 這就是為什麼群組 1 和群組 2 中會有兩個資料列，而群組 3 和群組 4 中只有一個資料列。  

    @result =
        SELECT 
            *,
            NTILE(4) OVER(PARTITION BY Vertical ORDER BY Latency) AS Quartile   
        FROM @querylog;

結果：

| 查詢 | 延遲 | Vertical | Quartile |
| --- | --- | --- | --- |
| Banana |300 |映像 |1 |
| Cherry |300 |映像 |2 |
| Durian |500 |映像 |3 |
| Apple |100 |Web |1 |
| Fig |200 |Web |1 |
| Papaya |200 |Web |2 |
| Fig |300 |Web |2 |
| Cherry |400 |Web |3 |
| Durian |500 |Web |4 |

NTILE 會使用參數 ("numgroups")。 Numgroups 是一個正整數或長常數運算式，會指定每個分割必須劃分成的群組數目。 

* 如果分割中的資料列數目可由 numgroups 整除，則群組會有相同的大小。 
* 如果分割區中的資料列數目無法被 numgroups 整除，則群組的大小會稍微不同。 在 OVER 子句所指定的順序中，較大的群組會排在較小的群組前面。 

例如：

    100 rows divided into 4 groups: 
    [ 25, 25, 25, 25 ]

    102 rows divided into 4 groups: 
    [ 26, 26, 25, 25 ]

### <a name="top-n-records-per-partition-via-rank-denserank-or-rownumber"></a>每個分割的前 N 筆記錄 (使用 RANK、DENSE_RANK 或 ROW_NUMBER)
許多使用者只想要選取每個群組的前 n 個資料列，這無法使用傳統 GROUP BY 達成。 

您在「排名函式」一節的開頭已看過下列範例。 其中並未顯示每個分割的前 n 筆記錄：

    @result =
    SELECT 
        *,
        ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber,
        RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank,
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;

結果：

| 查詢 | 延遲 | Vertical | RANK | DenseRank | RowNumber |
| --- | --- | --- | --- | --- | --- |
| Banana |300 |映像 |1 |1 |1 |
| Cherry |300 |映像 |1 |1 |2 |
| Durian |500 |映像 |3 |2 |3 |
| Apple |100 |Web |1 |1 |1 |
| Fig |200 |Web |2 |2 |2 |
| Papaya |200 |Web |2 |2 |3 |
| Fig |300 |Web |4 |3 |4 |
| Cherry |400 |Web |5 |4 |5 |
| Durian |500 |Web |6 |5 |6 |

### <a name="top-n-with-dense-rank"></a>前 N 個 (使用 DENSE RANK)
下列範例會從每個群組中傳回前三筆記錄，且每個分割中的資料列沒有循序排名編號的間距。

    @result =
    SELECT 
        *,
        DENSE_RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS DenseRank
    FROM @querylog;

    @result = 
        SELECT *
        FROM @result
        WHERE DenseRank <= 3;

結果：

| 查詢 | 延遲 | Vertical | DenseRank |
| --- | --- | --- | --- |
| Banana |300 |映像 |1 |
| Cherry |300 |映像 |1 |
| Durian |500 |映像 |2 |
| Apple |100 |Web |1 |
| Fig |200 |Web |2 |
| Papaya |200 |Web |2 |
| Fig |300 |Web |3 |

### <a name="top-n-with-rank"></a>前 N 個 (使用 RANK)
    @result =
        SELECT 
            *,
            RANK() OVER (PARTITION BY Vertical ORDER BY Latency) AS Rank
        FROM @querylog;

    @result = 
        SELECT *
        FROM @result
        WHERE Rank <= 3;

結果：    

| 查詢 | 延遲 | Vertical | RANK |
| --- | --- | --- | --- |
| Banana |300 |映像 |1 |
| Cherry |300 |映像 |1 |
| Durian |500 |映像 |3 |
| Apple |100 |Web |1 |
| Fig |200 |Web |2 |
| Papaya |200 |Web |2 |

### <a name="top-n-with-rownumber"></a>前 N 個 (使用 ROW_NUMBER)
    @result =
        SELECT 
            *,
            ROW_NUMBER() OVER (PARTITION BY Vertical ORDER BY Latency) AS RowNumber
        FROM @querylog;

    @result = 
        SELECT *
        FROM @result
        WHERE RowNumber <= 3;

結果：   

| 查詢 | 延遲 | Vertical | RowNumber |
| --- | --- | --- | --- |
| Banana |300 |映像 |1 |
| Cherry |300 |映像 |2 |
| Durian |500 |映像 |3 |
| Apple |100 |Web |1 |
| Fig |200 |Web |2 |
| Papaya |200 |Web |3 |

### <a name="assign-globally-unique-row-number"></a>指派全域唯一資料列號碼
為每個資料列指派全域唯一號碼，通常會很有用。 排名函式比較簡單，而且比使用歸納器更有效率。

    @result =
        SELECT 
            *,
            ROW_NUMBER() OVER () AS RowNumber
        FROM @querylog;

<!-- ################################################### -->
## <a name="analytic-functions"></a>分析函式
分析函式可用來了解值在視窗中的分佈情形。 分析函式最常見的使用案例，就是計算百分位數。

**支援的分析視窗函式**

* CUME_DIST 
* PERCENT_RANK
* PERCENTILE_CONT
* PERCENTILE_DISC

### <a name="cumedist"></a>CUME_DIST

CUME_DIST 會計算指定的值在值群組中的相對位置。 它會計算延遲小於或等於目前的查詢延遲 (具有相同垂直) 之查詢的百分比。 

資料列 R (採用遞增排序) 的 CUME_DIST 是值小於或等於 R 之值的資料列數目，除以分割中評估的資料列數目所得之數。 

CUME_DIST 會傳回 0 < x <= 1 範圍內的數值。

**語法：**

    CUME_DIST() 
        OVER (
            [PARTITION BY <identifier, > …[n]]
            ORDER BY <identifier, > …[n] [ASC|DESC] 
    ) AS <alias>

下列範例使用 CUME_DIST 函式來計算垂直中每個查詢的延遲百分位數。 

    @result=
        SELECT 
            *,
            CUME_DIST() OVER(PARTITION BY Vertical ORDER BY Latency) AS CumeDist
        FROM @querylog;

結果：

| 查詢 | 延遲 | Vertical | CumeDist |
| --- | --- | --- | --- |
| Durian |500 |映像 |1 |
| Banana |300 |映像 |0.666666666666667 |
| Cherry |300 |映像 |0.666666666666667 |
| Durian |500 |Web |1 |
| Cherry |400 |Web |0.833333333333333 |
| Fig |300 |Web |0.666666666666667 |
| Fig |200 |Web |0.5 |
| Papaya |200 |Web |0.5 |
| Apple |100 |Web |0.166666666666667 |

分割區索引鍵為 "Web" 的分割區中有六個資料列。

* 有六個資料列的值等於或小於 500，因此 CUME_DIST 等於 6/6=1
* 有五個資料列的值等於或小於 400，因此 CUME_DIST 等於 5/6=0.83
* 有四個資料列的值等於或小於 300，因此 CUME_DIST 等於 4/6=0.66
* 有三個資料列的值等於或小於 200，因此 CUME_DIST 等於 3/6=0.5。 有兩個資料列具有相同的延遲值。
* 有一個資料列的值等於或小於 100，因此 CUME_DIST 等於 1/6=0.16。 

**使用注意事項：**

* 相等值一律會評估為相同的累加分配值。
* NULL 值會被視為最低的可能值。
* 需要 ORDER BY 子句才能計算 CUME_DIST。
* CUME_DIST 類似於 PERCENT_RANK 函式

附註：如果 SELECT 陳述式後面沒接著 OUTPUT，則不允許 ORDER BY 子句。

### <a name="percentrank"></a>PERCENT_RANK
PERCENT_RANK 會計算資料列群組內某資料列的相對排名。 PERCENT_RANK 可用來評估某值在資料列集或分割內的相對位置。 PERCENT_RANK 傳回的值範圍會大於 0 且小於或等於 1。 不同於 CUME_DIST，PERCENT_RANK 的第一個資料列一律為 0。

** 語法：**

    PERCENT_RANK() 
        OVER (
            [PARTITION BY <identifier, > …[n]]
            ORDER BY <identifier, > …[n] [ASC|DESC] 
        ) AS <alias>

**注意事項**

* 任何集合中第一個資料列的 PERCENT_RANK 皆為 0。
* NULL 值會被視為最低的可能值。
* PERCENT_RANK 需要 ORDER BY 子句。
* CUME_DIST 類似於 PERCENT_RANK 函式 

下列範例使用 PERCENT_RANK 函式來計算垂直中每個查詢的延遲百分位數。 

指定的 PARTITION BY 子句會依垂直分割結果集中的資料列。 OVER 子句中的 ORDER BY 子句會排序每個分割中的資料列。 

PERCENT_RANK 函式所傳回的值代表查詢在垂直內的延遲排名 (以百分比表示)。 

    @result=
        SELECT 
            *,
            PERCENT_RANK() OVER(PARTITION BY Vertical ORDER BY Latency) AS PercentRank
        FROM @querylog;

結果：

| 查詢 | 延遲：INT | Vertical | PercentRank |
| --- | --- | --- | --- |
| Banana |300 |映像 |0 |
| Cherry |300 |映像 |0 |
| Durian |500 |映像 |1 |
| Apple |100 |Web |0 |
| Fig |200 |Web |0.2 |
| Papaya |200 |Web |0.2 |
| Fig |300 |Web |0.6 |
| Cherry |400 |Web |0.8 |
| Durian |500 |Web |1 |

### <a name="percentilecont--percentiledisc"></a>PERCENTILE_CONT 和 PERCENTILE_DISC
這兩個函式會根據資料行值的連續或離散分佈來計算百分位數。

**語法：**

    [PERCENTILE_CONT | PERCENTILE_DISC] ( numeric_literal ) 
        WITHIN GROUP ( ORDER BY <identifier> [ ASC | DESC ] )
        OVER ( [ PARTITION BY <identifier,>…[n] ] ) AS <alias>

**numeric_literal** - 要計算的百分位數。 此值必須介於 0.0 到 1.0 的範圍內。

    WITHIN GROUP (ORDER BY <identifier> [ ASC | DESC ])

指定用來排序和計算百分位數的數值清單。 僅允許單一資料行識別碼。 運算式必須評估為數值類型。 不允許其他資料類型。 預設排序順序為遞增。

    OVER ([ PARTITION BY <identifier,>…[n] ] )

根據套用百分位數函式的分割區索引鍵，將輸入資料列集分成多個分割區。 如需詳細資訊，請參閱本文的「排名」一節。
附註：資料集中的任何 null 值都會被忽略。

**PERCENTILE_CONT** 會根據資料行值的連續分佈來計算百分位數。 其結果會以內插值取代，且可能不會等於資料行中的任何特定值。 

**PERCENTILE_DISC** 會根據資料行值的離散分佈來計算百分位數。 其結果會等於資料行中的特定值。 換句話說，相較於 PERCENTILE_CONT，PERCENTILE_DISC 一律會傳回實際 (原始輸入) 值。

您可以檢視兩者在下列範例中的運作情形；它們會嘗試尋找每個垂直內之延遲的中間值 (百分位數=0.50)

    @result = 
        SELECT 
            Vertical, 
            Query,
            PERCENTILE_CONT(0.5) 
                WITHIN GROUP (ORDER BY Latency)
                OVER ( PARTITION BY Vertical ) AS PercentileCont50,
            PERCENTILE_DISC(0.5) 
                WITHIN GROUP (ORDER BY Latency) 
                OVER ( PARTITION BY Vertical ) AS PercentileDisc50 

        FROM @querylog;

結果：

| 查詢 | 延遲：INT | Vertical | PercentileCont50 | PercentilDisc50 |
| --- | --- | --- | --- | --- |
| Banana |300 |映像 |300 |300 |
| Cherry |300 |映像 |300 |300 |
| Durian |500 |映像 |300 |300 |
| Apple |100 |Web |250 |200 |
| Fig |200 |Web |250 |200 |
| Papaya |200 |Web |250 |200 |
| Fig |300 |Web |250 |200 |
| Cherry |400 |Web |250 |200 |
| Durian |500 |Web |250 |200 |

就 PERCENTILE_CONT 而言，由於可以插補值，因此 Web 的中間值是 250，即使 Web 垂直中沒有查詢的延遲是 250 亦然。 

PERCENTILE_DISC 不會插補值，因此 Web 的中間值是 200 - 也就是在輸入資料列中找到的實際值。

## <a name="see-also"></a>另請參閱
* [使用適用於 Visual Studio 的 Data Lake 工具開發 U-SQL 指令碼](data-lake-analytics-data-lake-tools-get-started.md)
* [使用 Azure Data Lake Analytics 互動式教學課程](data-lake-analytics-use-interactive-tutorials.md)
* [開始使用 Azure Data Lake Analytics U-SQL 語言](data-lake-analytics-u-sql-get-started.md)



