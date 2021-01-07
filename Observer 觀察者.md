# Observer 觀察者

建立觀察者class : app/Observers/名稱Observer.php
	
時機點:
- creating($model)	: 創建前
- created($model)		: 創建後
- updating($model)	: 更新前
- updated($model)		: 更新後
- saving($model)		: 執行前
- saved($model)		: 執行後
- deleting($model)	: 刪除前
- deleted($model)		: 刪除後
- restoring($model)	: 軟刪除前
- restored($model)	: 軟刪除後

啟用方式: Model 的 boot 添加此class即可.

```php
protected static function boot()
{
	parent::boot();

	static::observe(名稱Observer::class);
}
```