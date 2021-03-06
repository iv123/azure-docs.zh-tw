您現在可以使用 [資料總管] 來建立圖形容器，並將資料新增至您的資料庫。 

1. 在 Azure 入口網站的導覽功能表中，按一下 [資料總管]。 
2. 在 [資料總管] 刀鋒視窗中，按一下 [新增資料表]，然後使用下列資訊填入頁面。

    ![Azure 入口網站中的資料總管](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

    設定|建議的值|說明
    ---|---|---
    資料庫識別碼|sample-database|新資料庫的識別碼。 資料庫名稱的長度必須介於 1 到 255 個字元，且不能包含 `/ \ # ?` 或尾端空格。
    資料表識別碼|sample-table|新資料表的識別碼。 資料表名稱與資料庫識別碼具有相同的字元需求。
    儲存體容量| 10 GB|保留預設值。 這是資料庫的儲存體容量。
    輸送量|400 RU|保留預設值。 如果您想要降低延遲，稍後可以相應增加輸送量。

3. 填妥表單後，按一下 [確定]。