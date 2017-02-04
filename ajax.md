- [Introduction](#introduction)
- [Basics of AJAX](#basics-of-ajax)
    + [Listening for actions](#listening-for-action)
    + [Actions response](#action-response)
    + [Calling actions from JavaScript](#calling-action-from-javascript)
    + [Protecting actions from CSRF with nonces](#protecting-actions-from-csrf-with-nonces)


<a name="introduction"></a>
## [Introduction](#introduction)

The [AJAX](https://codex.wordpress.org/Glossary#AJAX) component allows for defining various actions, which you can afterward hit with HTTP requests in order to create smooth and dynamic JavaScript components for your application.

<a name="basics-of-ajax"></a>
## [Basics of AJAX](#basics-of-ajax)

All application AJAX actions should be defined inside `app\Http\ajaxes.php` file. This file is included on application bootstrap by the `App\Providers\HttpServiceProvider` class.

<a name="listening-for-actions"></a>
### [Listening for actions](#listening-for-actions)

All you have to do, in order to create action listener, is to call `listen` method on `Ajax` facade.

```php
Ajax::listen($action, $callback);
```

##### Listening only for autorized or unauthorized actions

By default, AJAX listens for both authorized and unauthorized actions. However, you can narrow that with the third argument.

```php
// Responds only to logged in users
Ajax::listen($action, $callback, ['accessibility' => 'authorized']);

// Responds only to not logged in users
Ajax::listen($action, $callback, ['accessibility' => 'unauthorized']);
```

<a name="actions-response"></a>
### [Action response](#actions-response)

The response callbacks are your field for action. This is where you define what your AJAX actions do. However, remember one thing - you must return results from the callback.

[alert type="info"]Responses, that returns `Array` or `Illuminate\Support\Collection`, will automatically be converted to the JSON.[/alert]

```php
Ajax::listen('five_latest_movies', function() {
    return Post::type('movies')->query([
        'posts_per_page' => '5',
        'order' => 'DESC',
        'orderby' => 'date',
    ]);
});
```

#### Response via Closure

For simpler actions, the closure may be entirely sufficient.

```php
Ajax::listen('action', function () {
    return ['message' => 'Out of rum. Why? Why are we out of rum?'];
});
```

Of course, closure arguments will be automatically resolved from the container.

```php
Ajax::listen('action', function (Posts $posts) {
    return $posts->all();
});
```

#### Response via Controller

As same as routes, you can also target your application controllers methods.

```php
// app/Http/ajaxes.php

Ajax::listen('action', 'HomeController@action');
```

```php
<?php

namespace App\Http\Controllers;

use Assely\Routing\Controller;

class HomeController extends Controller
{
    /**
     * Ajax action response.
     *
     * @return array
     */
    public function action() {
        return ['message' => 'Hide the rum!'];
    }
}
}
```

<a name="calling-actions-from-javascript"></a>
### [Calling actions from JavaScript](#calling-actions-from-javascript)

Finally, we can reach actions from the frontend. As example we will use `jQuery.ajax()` to perform an asynchronous HTTP request to our action endpoint.

[alert type="warning"]You have to pass nonce token with your AJAX request, otherwise action will return an error message.[/alert]

```js
jQuery.ajax({
    url: Assely.ajax.url,
    dataType: 'json',
    data: {
        // Ajax action name.
        action: 'action',
        // Security nonce token
        nonce: Assely.ajax.nonce,
        // The value you want to pass
        key: 'value'
    }
}).done(function(data, status, response) {
    console.log(data);
});
```

<a name="protecting-actions-from-csrf-with-nonces"></a>
### [Protecting actions from CSRF with nonces](#protecting-actions-from-csrf-with-nonces)

Action calls **must** include a nonce token for protecting against CSRF attacks. Along with action name pass `Assely.ajax.nonce` value in `nonce` named key.

```js
{
    action: 'action',
    nonce: Assely.ajax.nonce
}
```