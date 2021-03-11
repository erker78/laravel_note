# laravel 三不五時就會忘記
被遺忘的Laravel事件簿，常常忘記一堆語法，找個地方存放。

[TOC]

## Observer 觀察者

建立觀察者class : app/Observers/名稱Observer.php

時機點:
```creating($model)	: 創建前
created($model)		: 創建後
updating($model)	: 更新前
updated($model)		: 更新後
saving($model)		: 執行前
saved($model)		: 執行後
deleting($model)	: 刪除前
deleted($model)		: 刪除後
restoring($model)	: 軟刪除前
restored($model)	: 軟刪除後

啟用方式: Model 的 boot 添加此class即可.
protected static function boot()
{
	parent::boot();

	static::observe(名稱Observer::class);
}
```



## model

fillable : 要改變的數值
dates : 時間參數
casts: 變數型態



## laravel 軟刪除-恢復

加入 withTrashed() 即可搜尋軟刪除資料
$this->model->withTrashed()



## laravel 軟刪除-特殊化

Model 添加 use SoftDeletes

如果欄位名稱剛好是 is_deleted
const IS_DELETED = 'is_deleted' 就不用寫

如果被軟刪的值剛好是 1
const DELETION_VALUE = 1 就不用寫

如果被還原的值剛好是 0 
const RESTORATION_VALUE = 0 就不用寫



## DB 關聯

參考: https://laravel.com/docs/5.4/eloquent-relationships

**一對一**
$this->hasOne(位置, key)

**一對多**
$this->hasMany(位置, key, 相對應key)

$this->hasMany(VB100::class, 'BC100_ID', 'BC100_ID');

$this->belongsTo(PG100::class, 'PG100_ID', 'PG100_ID')->withDefault(); //withDefault()當抓不到資料可以預設



## optional 輔助函數

```
optional() : 當不確定是否有含某個 object 的時候使用。

原本: $user->profile->address; //希望取得個人的地址，不過不確定user是否有這個 profile。
修改: optional($user->profile)->address; // 如果 profile 是沒有的，就會變成null，避免出現錯誤訊息。
```



## Eloquent ORM 基本操作

```
https://ithelp.ithome.com.tw/articles/10209653

firstOrCreate : 如果在資料庫找不到模型，會用給定的屬性來新增一筆記錄
firstOrNew : 假設找不到模型，將會回傳一個新的模型實例，但需要呼叫save來儲存
updateOrCreate : 如果有找到資料的話就更新，沒有的話就建立一筆資料

$api(已經有Eloquent資料)

$api->exists()	是否有資料，回傳 bool : true,false
$api->count() 	有多少資料，回傳 number
$api->first()	抓出一筆資料，回傳 Model 物件或是 null
$api->find()	搜尋primary key，回傳 Model 物件
$api->findOrFail()	如果沒有找到指定資料就 throw exception
$api->withTrashed()	取得包含已經刪除的資料（deleted_at not null）
```



## laravel DB使用方式

```
##select
	
#吸毒派

#多數 object
DB::table('table')->get();
= select * from table;

DB::table('table')->where('id','1')->get();
= select * from table where id = 1;

DB::table('table')->select('value')->where('id','1')->get();
= select value from table where id = 1;

#單行 object 單一
DB::table('table')->where('id','1')->first();

#單列 object 多數
DB::table('table')->where('id','1')->pluck('value');

#單值 object 單一
DB::table('table')->where('id','1')->value('value');

#轉 object -> array
DB::table('table')->get()->toArray();

#總筆數
DB::table('table')->count();

#運算
DB::table('table')->max('number'); 取最大值
DB::table('table')->min('number'); 取最小值
DB::table('table')->avg ('number'); 取平均值
DB::table('table')->sum ('number'); 取加總
```



## laravel seeder 資料填充

```
創建指令
php artisan make:seeder {名稱}
php artisan make:factory {名稱} --model={使用的model，選填}

創建指令範例
php artisan make:seeder china/CnMN140Seeder
php artisan make:factory MN140Factory --model=Models\\MN140

創建檔案
1. database/seeds/china/CnMNFI100Seeder.php
   1. 執行檔案位置
2. database/factories/MNFI100Factory.php
   1. DB資料結構處理

指令: php artisan db:seed --class=CnMNFI100Seeder
建議刷一次 composer dump-autoload
```



## laravel Carbon 時間參數

eq – 判斷兩個日期是否相等。相等: true，不相等false
ne - 判斷兩個日期是否不相等。相等: false，不相等true
gt – 判斷第一個日期是否比第二個日期大。
lt – 判斷第一個日期是否比第二個日期小。
gte – 判斷第一個日期是否大於等於第二個日期。
lte – 判斷第一個日期是否小於等於第二個日期。



## laravel 集合

isEmpty() : 查看集合是否為空，是:true，否:false
contains() : 方法用來判斷該集合是否含有指定的項目