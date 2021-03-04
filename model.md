# model

[TOC]

## [模型設定]

| code                  | 解釋                       | 範例                                                         |
| --------------------- | -------------------------- | ------------------------------------------------------------ |
| protected $table      | 資料表                     | `'TE100'`                                                    |
| protected $primaryKey | 資料表.主鍵                | `'TE100_ID'`                                                 |
| protected $fillable   | 要改變的數值               | ` ['TE100_ID','content','amount']`                           |
| protected $guarded    | 黑名單<br />fillable的相反 | ` ['test']`                                                  |
| protected $dates      | 時間戳參數                 | `['created_at','updated_at']`                                |
| protected $casts      | 變數型態                   | `['TE100_ID' => 'integer','amount'=>'float']`<br /><br />integer、real、float、double、string、boolean、object、array |
| protected $hidden     | 隱藏參數                   | `['security_questions']`                                     |
| public $timestamps    | 自動時間戳記               | 預設開啟，false:關閉                                         |
| protected $appends    | 新增屬性                   | `['is_admin']`                                               |

## [刪除方法]

| code          | 解釋                 | 範例                                                         |
| ------------- | -------------------- | ------------------------------------------------------------ |
| withTrashed() | 強制查詢軟刪除資料   | `User::withTrashed()->where('account_id', 1)->get();`        |
| onlyTrashed() | 只查詢被軟刪除資料   | `User::onlyTrashed()->where('account_id', 1)->get();`        |
| restore()     | 軟刪除資料恢復       | `User::withTrashed()->where('account_id', 1)->restore();`    |
| forceDelete() | 軟刪除資料，實體刪除 | `User::withTrashed()->where('account_id', 1)->forceDelete();` |
| trashed()     | 確認是否被軟刪除     | `if ($user->trashed()){...}`                                 |

## [關聯方法]

| code          | 解釋         | 範例                                                         |
| ------------- | ------------ | ------------------------------------------------------------ |
| hasOne        | 一對一       | `public function phone() { return $this->hasOne('App\Phone'); }`<br /><br />`SQL:`<br />`select * from users where id = 1`<br />`select * from phones where user_id = 1` |
| belongsTo     | 一對一，關聯 | `public function user() { return $this->belongsTo({model}, {本地鍵值}, {關聯鍵值}); }`<br />`public function user() { return $this->belongsTo('App\User', 'local_key', 'parent_key'); }` |
| hasMany       | 一對多       | `public function comments() { return $this->hasMany('App\Comment'); }` |
| hasMany       | 一對多，關聯 | `public function comments() { return $this->hasMany({model}, {關聯鍵值}, {本地鍵值}); }`<br />`public function comments() { return $this->hasMany('App\Comment', 'foreign_key', 'local_key'); }` |
| belongsToMany | 多對多       | `public function roles() { return $this->belongsToMany('App\Role'); }` |
|               |              |                                                              |

## [新增屬性資料]

``` php 
protected $appends = ['is_admin'];

public function getIsAdminAttribute() // get{大駝峰命名}Attribute
{
    return $this->attributes['admin'] == 'yes';
}
```

