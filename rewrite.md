- [Introduction](#introduction)
- [Getting started](#getting-started)
- [Basics of Rewriting](#basics-of-rewriting)
    + [Creating Rewrite Rule](#creating-rewrite-rule)
    + [Creating Endpoint](#creating-endpoint)


<a name="introduction"></a>
## [Introduction](#introduction)

The [WordPress Rewrite API](https://developer.wordpress.org/files/2014/10/template-hierarchy.png) allows developers to programmatically specify new, custom rewrite rules. However, it is really unclear and unreadable. The Rewrite component closes all of this messy code into one easy to follow API.

<a name="basics-of-rewriting"></a>
## [Basics of Rewriting](#basics-of-rewriting)

All application custom rewrite rules should be defined inside `app\Http\rewrites.php` file. This file is included on application bootstrap by the `App\Providers\HttpServiceProvider` class.

[alert type="warning"]Remember to flush rewrite cache after creating or removing rules. You can do that with `wp assely:clear rewrites` command or manually resave "Permalinks" settings in admin panel.[/alert]

<a name="creating-rewrite-rule"></a>
### [Creating Rewrite Rule](#creating-rewrite-rule)

The most basic custom rewrite rule is a static URI. You can define it with `rule` method and desired path string as argument:

```php
Rewrite::rule('favourite/movies/top10');
```

#### Creating Rewrite Rule with Parameter

Often you will need to define dynamical segments of the URI. For example, to display a list of your favorite movies from the specific date range. You may do so by defining a rule with parameters:

```php
Rewrite::rule('favourite/movies/{from}/{to}');
```

Additionally, you can constrain the format of parameters with the `where` method. It takes an array of parameters names and regular expressions.

```php
Rewrite::rule('favourite/movies/{from}/{to}')->where([
    'from' => '([0-9]{4})',
    'to' => '([0-9]{4})'
]);
```

<a name="creating-endpoint"></a>
### [Creating Endpoint](#creating-endpoint)

Endpoints allow you to create a group of extra rewrite rules which will be applied to a group of all already created rules within the specifed place (like permalinks or pages).

Let’s assume you want to create links to JSON representation of movies. To do that, call `endpoint` method with `json` endpoint name. You also need to specify endpoint place with `to` method.

[alert type="info"]Descriptions of all available places you can find in the [Codex](https://codex.wordpress.org/Rewrite_API/add_rewrite_endpoint#Available_Places)[/alert]

```php
Rewrite::endpoint('json')->to(EP_PERMALINK);
```
