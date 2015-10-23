# belongsToThrough [![](https://img.shields.io/travis/znck/belongs-to-through.svg)](https://travis-ci.org/znck/belongs-to-through) [![](https://img.shields.io/packagist/v/znck/belongs-to-through.svg)](https://packagist.org/packages/znck/belongs-to-through) [![](https://img.shields.io/packagist/dt/znck/belongs-to-through.svg)](https://packagist.org/packages/znck/belongs-to-through)  [![](https://img.shields.io/packagist/l/znck/belongs-to-through.svg)](http://znck.mit-license.org) [![](https://www.codacy.com/project/badge/005c3669e57442a198f3a4ffe5e5c9e2)](https://www.codacy.com/app/znck/belongs-to-through)

Adds belongsToThrough relation to laravel models

## Installation

First, pull in the package through Composer.

```js
"require": {
    "znck/belongs-to-through": "~1.0"
}
```

## Usage

Within your eloquent model class add following line

```php
class User extends Model {
    use \Znck\Eloquent\Traits\BelongsToThrough;
    ...
}
```

## Example: 
Consider a blog application. In this app, a country can have many users and a user can have many articles. So, `hasManyThrough` provides easy way to access articles from a country.

```php 
class Country extends Model {
    use \Znck\Eloquent\Traits\BelongsToThrough;
    
    public function articles () {
        return $this->hasManyThrough(Article::class, User::class);
    }
}
```

If we are accessing the country of the article, then we have to use `$article->user->country`.

```php
Class Article extends Model {
    use \Znck\Eloquent\Traits\BelongsToThrough;
    
    public function country() {
        return $this->belongsToThrough(Country::class, User::class);
    }
}
```

Now, the magic: `$article->country`

> And more deeper

```php

use Illuminate\Database\Eloquent\Model;
use Znck\Eloquent\Traits\BelongsToThrough;

class Country extends Model
{

}

class State extends Model
{

}

class District extends Model
{
    use BelongsToThrough;

    public function country()
    {
        return $this->belongsToThrough(Country::class, State::class);
    }
}

class City extends Model
{
    use BelongsToThrough;

    public function country()
    {
        return $this->belongsToThrough(Country::class, [State::class, District::class]);
    }
}

$city = City::find(1);

$city->country;
```
