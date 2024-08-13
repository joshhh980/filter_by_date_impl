# DRY implementation of a filter by date feature

Had to implement a filter by date feature on multiple endpoints in a point of sale web app/web site and because the dates had to be initialized if null, I used a trait that had a scope to make the feature available across multiple models to keep the code DRY

```php
<?php

namespace App\Models\Traits;

trait HasDateFilter
{
    public function scopeFilterByDate($query)
    {

    }
}
```

The request method is global can be accessed in models so the initializing could be done once in the trait and not repeated across multiple files. 

```php
<?php

namespace App\Models\Traits;

use Carbon\Carbon;

trait HasDateFilter
{
    public function scopeFilterByDate($query)
    {
        $date_input = request()
            ->input("date");
        $end_date = request()
            ->input("endDate");
        if(!$date_input || $date_input == "null" || $date_input == "undefined"){
            $date_input = Carbon::now();
        }
        if(!$end_date || $end_date == "null" || $end_date == "undefined"){
            $end_date = Carbon::now();
        }
    }
}
```

The table name of the model could also be accessed via the 'getTable' method making the scope dynamic

```php
<?php

namespace App\Models\Traits;

use Carbon\Carbon;

trait HasDateFilter
{
    public function scopeFilterByDate($query)
    {
        $date_input = request()
            ->input("date");
        $end_date = request()
            ->input("endDate");
        if(!$date_input || $date_input == "null" || $date_input == "undefined"){
            $date_input = Carbon::now();
        }
        if(!$end_date || $end_date == "null" || $end_date == "undefined"){
            $end_date = Carbon::now();
        }
        $table = "{$this->getTable()}.created_at";
        // query here ....
        return $query;
    }
}
```
