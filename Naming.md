# Naming Rule 
###### tags: `規範`
General
---
1. 變數以「小駝峰」命名。
ex: `$partnerId`、 `$partnerNo`
2. 陣列/物件內key值使用「蛇形」命名。
ex: `$filter['spec_type']`、`$result['stock_status']`
3. Function名稱以「小駝峰」命名。
ex: `prepareOtherInfo`、`getAutoComplete`
3. Class名稱「大駝峰」命名。
ex: `ReceiptController`、`ProductService`

Controller
---
1. 以「單數名詞」為class命名準則。
ex: `EmployeeController`、`ReceiptController`
2. function名稱使用「Http Method」為前綴。
ex: `getEmployee`、`getChannelList`、`putReceipt`、`postGoods`、`deleteAuthGroup`
3. List的API, 請一律加上 excluded_ids filter欄位

Service
---
1. 以「單一事件」作為function命名準則
ex: `getList`、`export`、`checkProductExist`
2. 新增單一資料可直接使用「add」作為命名前綴
3. 更新單一資料可直接使用「update」作為命名前綴
4. 若新增與更新在同一個function作動, 命名前綴改用「save」

Repository
---
> 以下原則針對function name進行規範
1. 若以指定條件進行處理，使用「By」為中間字，其後加上條件。
ex: `getById`、`getListByFilter`
2. 若取回的資料有包含關聯，使用「With」當後綴。
ex: `getWithDetail`、`getByIdWithDetail`
3. 取得單筆資料以「get」為前綴。
ex: `getByNo`、`getById`
4. 取得列表資料以「getList」為前綴。
ex: `getListByFilter`
5. 新增資料以「add」為前綴。
ex: `add`
6. 更新資料以「update」為前綴。
ex: `updateStatus`
7. 刪除資料以「delete」為前綴(不論軟刪還是硬刪)。
ex: `delete`

Enum
---
1. 若與DB存放資料相關聯，Enum名稱請以「Table名稱+欄位名稱」為前綴。
ex: `ChannelTypeEnum`、`ReceiptStatusEnum`
2. 與搜尋排序相關Enum名稱請以「Table名稱+List+功能」為前綴。
ex: `PayableListOrderByEnum`

Database
---
1. Table name、Column name皆以「小寫」、「蛇形」、「單數名詞」命名。
ex: Table name - `receipt_warehouse`、`channel_store`
ex: Column name - `channel_store_no`、`other_info`
2. true/false column name前綴加「is_」 或「has_」。
ex: `is_edited`、`is_enable`、`has_children`

Column
---
| 類型 | 命名 |
| -------- | -------- |
| 排序      | sort|
| 金額     | amount     |
| 數量     | quantity     |
| 計數     | count     |
| 搜尋時間區間     | xxx_at_from / xxx_at_to     |
| 搜尋日期區間     | xxx_date_from / xxx_date_to     |
| 比例 / 比率 | ratio |


TBD
---
1. mapping table是否要加primary key Id or 組合鍵？