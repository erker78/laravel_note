API通則
===
###### tags: `規範`
## API 結構
### 結果為單筆
StatusCode: 200
```
{
    "id": "<RequestID>",
    "status": "SUCCESS", 
    "data": {
        "<KEY>": "<VALUE>"
    }, 
    "message" => ["我是訊息1", "我是訊息2", "我是訊息3"]
}
```
### 結果為多筆
StatusCode: 200
```
{
    "id": "<RequestID>",
    "status": "SUCCESS", 
    "data": {
        "detail": [
            {
                "<KEY>": "<VALUE>"
            }
        ],
        "status_count": {
            "<狀態代碼>": (integer | string[Deprecating])
        },
        "total": (integer | string[Deprecating])<總筆數>,
        "total_page": (integer | string[Deprecating])<總頁數>,
        "current_page": (integer | string[Deprecating])<目前頁數>,
        "limit": (integer | string[Deprecating])<一頁幾筆>
    }
}
```
### 錯誤結果
StatusCode:
* 400 => 輸入非Json => 不導頁
* 401 => 帳號錯誤 / Token錯誤 / Token過期
* 403 => 權限不足
* 404 => 找不到路徑 / 找不到資料 => 不導頁
* 405 => Method錯誤 => 不導頁
* 400(Deprecating), 422 => 資料驗證錯誤 => 不導頁
* 500 => 系統錯誤 => 不導頁
* 503 => 維護中 => 導頁回首頁並顯示維護中的訊息
```
{
    "id": "<RequestID>",
    "status": "FAILURE",
    "error": {
        "code": (string)<錯誤代碼>,
        "message": (string)<錯誤訊息>,
        "reason": (string)<Exception>, => Deprecate
        "show_reason": (boolean)<是否顯示原因> => Deprecate
    }, 
    "message" => ["我是訊息1", "我是訊息2", "我是訊息3"]
}
```
## API輸入
### Authorization: 
#### 登入後
```
Authorization: Bearer (string)<Token from Auth service>
```
### 參數規範
```
若參數為數字, 文字陣列, 請以s當作key結尾
EX: product_ids=1,2,3 or product_ids: [1, 2, 3]
若參數為物件 / 物件陣列, 則以info當結尾
EX: prodcut_info: { a: b, c:d }
```
### GET: Query String
```
<APL_URL>?<PARAM_KEY>=<PARAM_VALUE>&<PARAM_KEY>=<PARAM_VALUE>...
```
### POST / PUT / PATCH: Request Body(Content-Type: application/json)
```
{
    "<KEY>": "<VALUE>", 
    "<KEY>": [{
        "<KEY>": "<VALUE>"
    }], 
    "<KEY>": {
        "<KEY>": "<VALUE>"
    }
}
```
---
##### Q&A List: 
> [name=Q1]partner_id到底還要不要帶
> [name=A1]新的不需要, 舊的慢慢刪
---
> [name=Q2]資料輸入格式檢查?
> [name=A2]只要該Key值有傳入, 就會檢查格式; 因此, 如果是非必填的欄位但是有檢查格式, 未輸入時請將該欄位型態決定, String / Integer則為null, Array則為空陣列
> EX1: company_no 欄位為非必填, 但必須為八碼數字
```
Scenario 1: company_no: "12345678"
Result: Pass
Scenario 2: company_no: ""
Result: Pass, (因為會自動trim + tranform to null)
Scenario 3: 不傳入company_no
Result: Pass
Scenario 4: company_no: null
Result: Pass
```
> EX2: company_info 的 name 欄位為非必填, 但必須為Object
```
Scenario 1: company_info: { name: "ABC"}
Result: Pass
Scenario 2: company_info: {}
Result: Pass
Scenario 3: 不傳入company_info
Result: Pass
Scenario 4: company_info: null
Result: Fail, 型態必須為物件
```
> EX3: companies 欄位為非必填, 但必須為Array
```
Scenario 1: companies: [1, 2, 3]
Result: Pass
Scenario 2: companies: []
Result: Pass
Scenario 3: 不傳入companies
Result: Pass
Scenario 4: companies: null
Result: Fail, 型態必須為陣列
```
# For 後端
## Naming
1. Object欄位延續_info後綴
2. Array欄位改為複數欄位
## Request
1. PUT / PATCH分開, 
    * 部分更新採用PATCH
    * 全部更新採用PUT
> Ex: 
> * PUT /channel_store, name為非必填, 若沒傳入則set db name = null
> * PATCH / channel_store, name為非必填, 沒傳入, 則不改變db name的值
> * 此通則一併套在Object上, Object中沒有傳入的key值依照上面的method決定
2. Validation規則更動如下
    1. 必填, 請使用required
    2. 非必填, **不**需加上sometimes
    3. 允許為空值, 用nullable
        1. 空字串, 因為有TrimStrings and ConvertEmptyStringsToNull, 所以會自動變成null
    4. Object / Array, 一率加上array
    5. https://laravel.com/docs/5.8/validation, 煩請熟讀!!!!可以用validation做掉的就做掉
    6. 現在錯誤訊息都會顯示給前端, 因此Request中的$attributes請自行加上, 讓驗證訊息更完善
## Response 
1. GET的值不應該受到PUT / PATCH影響, API Doc上有的欄位就必須全部都有, 不管是不是存在於Object當中
> Ex:
> GET /channel_store, contact_info.name, 永遠都要有此欄位
2. 若該欄位沒值, 一律回傳null, 除了Array要回傳[]
3. Object沒有上述問題, 因為Object裡面的Attribute都要存在