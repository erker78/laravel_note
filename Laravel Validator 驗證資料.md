# Laravel Validator 驗證資料

方式1:

```php
use Validator;

$validator = Validator::make(request()->all(), [
    'name' => 'required|max:10',
    'cell' => 'required|numeric|regex:/(09)[0-9]/|digits:10',  // 09開頭, 0-9數字, 一定要剛好10個數字
    'email' => 'nullable|email|max:30',
    'b' => 'required|numeric|digits_between:0,9|max:9',   // 0-9 數字
    'password' => [
        'required',
        'string',
        'min:8',              // must be at least 8 characters in length
        'regex:/[a-z]/',      // must contain at least one lowercase letter
        'regex:/[A-Z]/',      // must contain at least one uppercase letter
        'regex:/[0-9]/',      // must contain at least one digit
        'regex:/[@$!%*#?&]/', // must contain a special character
        'regex:/^\S*$/u'  // can not contain space
    ],
    'password_confirm' => 'required|same:password',
    "question_name" => "required|max:30",
    "question_options_name"    => "required|array|min:1",  //array最少須包含一個element
    "question_options_name.*"  => "required|string|distinct|min:1|max:30",   //array中的字串必須是distinct, 且最少一個字, 最多30個字
]);

if ($validator->fails()) {
    return response()->json([
        'status' => 'fail',
        'errors' => $validator->errors()
    ], 400);
}
```

方式2:

```php
public function validatedData($request)
    {
        return $request->validate([
            'ID' => 'nullable|integer',		//選填，須為整數
            'pairing_by_type' => 'nullable|in:0,1',	//選填，只允許0與1
            'start_date' => 'nullable|date|required_with:end_date', //選填，日期，如果end_date有數值，這個數值為必填
            'end_date' => 'nullable|date|required_with:start_date|after_or_equal:start_date', 
            //選填，日期，如果start_date有數值，這個數值為必填，start_date是否在指定日期之後或同一天
            'hide_no_card' => 'nullable|boolean', //選填，布林數
            'tpi_txn_mode' => 'nullable|array', //選填，陣列
            'tpi_txn_mode.*' => 'integer|min:101', //整數，最小值101
        ], [], trans('validation.model-attributes.FI100')); //錯誤訊息 validation
    }
```







| code                       | 解釋                                                         | Coding Style |
| -------------------------- | ------------------------------------------------------------ | ------------ |
| required                   | 必要，不能為空                                               | 前           |
| present                    | 必要，可以為空                                               | 前           |
| nullable                   | 選填，可以為空                                               | 前           |
| filled                     | 選填，不能為空                                               | 前           |
| string                     | 字串                                                         | 中           |
| numeric                    | 數值，包含小數                                               | 中           |
| integer                    | 整數                                                         | 中           |
| array                      | 陣列                                                         | 中           |
| date                       | 日期，會根據 PHP 的 `strtotime` 函示做驗證                   | 中           |
| unique:table,column,except | 判斷是否為資料表內唯一值<br />table: 資料表<br />column: 欄位 | 後           |
| exists:table,column        | 判斷是否能在資料表內搜尋到<br />table: 資料表<br />column: 欄位 | 後           |
| regex                      | 正規表示式                                                   | 後           |
| digits:value               | 數字且長度須為value                                          | 後           |
| in:foo,bar                 | 驗證欄位值有在給定的清單裡                                   | 後           |
| required_with              | 如果指定的欄位之中， *任一* 個有值，則此欄位為必填           | 後           |
| after_or_equal:date        | 驗證欄位是否在指定日期之後或同一天                           | 後           |
| date_format:format         | 驗證欄位值符合定義的日期格式（ *format* ）                   | 後           |

---

參考網址:

[1] https://laravel.tw/docs/4.2/validation#rule-numeric

[2] https://vocus.cc/article/5fc4cb5efd897800017818c7