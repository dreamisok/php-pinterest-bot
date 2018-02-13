# Pinterest Bot for PHP

<p align="center">
    <img src="logo.jpg" alt="Pinterest PHP Bot">
</p>

##
<a href="https://travis-ci.org/seregazhuk/php-pinterest-bot"><img src="https://camo.githubusercontent.com/412fbc5888a7d03937daf554d662678477512375/68747470733a2f2f7472617669732d63692e6f72672f7365726567617a68756b2f7068702d70696e7465726573742d626f742e737667" alt="Build Status" data-canonical-src="https://travis-ci.org/seregazhuk/php-pinterest-bot.svg" style="max-width:100%;"></a>
<a href="https://codeclimate.com/github/seregazhuk/php-pinterest-bot"><img src="https://camo.githubusercontent.com/d30f6b154177d3a589af46e154b31ec66ef05128/68747470733a2f2f636f6465636c696d6174652e636f6d2f6769746875622f7365726567617a68756b2f7068702d70696e7465726573742d626f742f6261646765732f6770612e737667" alt="Code Climate" data-canonical-src="https://codeclimate.com/github/seregazhuk/php-pinterest-bot/badges/gpa.svg" style="max-width:100%;"></a>
<a href="https://scrutinizer-ci.com/g/seregazhuk/php-pinterest-bot/?branch=master"><img src="https://camo.githubusercontent.com/c05b7ed73a9c49224fb982888fc7ac643fbec5f6/68747470733a2f2f7363727574696e697a65722d63692e636f6d2f672f7365726567617a68756b2f7068702d70696e7465726573742d626f742f6261646765732f7175616c6974792d73636f72652e706e673f623d6d6173746572" alt="Scrutinizer Code Quality" data-canonical-src="https://scrutinizer-ci.com/g/seregazhuk/php-pinterest-bot/badges/quality-score.png?b=master" style="max-width:100%;"></a>
<a href="https://codeclimate.com/github/seregazhuk/php-pinterest-bot/coverage"><img src="https://camo.githubusercontent.com/704321dd17ccb2c5de0e954e1c29a8cefa2572be/68747470733a2f2f636f6465636c696d6174652e636f6d2f6769746875622f7365726567617a68756b2f7068702d70696e7465726573742d626f742f6261646765732f636f7665726167652e737667" alt="Test Coverage" data-canonical-src="https://codeclimate.com/github/seregazhuk/php-pinterest-bot/badges/coverage.svg" style="max-width:100%;"></a>
<a href="https://styleci.io/repos/39557985"><img src="https://styleci.io/repos/39557985/shield?branch=master" alt="StyleCI"></a>
<a href="https://packagist.org/packages/seregazhuk/pinterest-bot"><img class="spinned latest_stable_version_img" src="https://poser.pugx.org/seregazhuk/pinterest-bot/v/stable" style="display: inline;"></a>
<a href="https://packagist.org/packages/seregazhuk/pinterest-bot"><img src="https://camo.githubusercontent.com/8957462146926452326740b5c6255bbdb3abea67/68747470733a2f2f706f7365722e707567782e6f72672f7365726567617a68756b2f70696e7465726573742d626f742f646f776e6c6f616473" alt="Total Downloads" data-canonical-src="https://poser.pugx.org/seregazhuk/pinterest-bot/downloads" style="max-width:100%;"></a>

[![Support me with some coffee](https://img.shields.io/badge/donate-paypal-orange.svg)](https://www.paypal.me/seregazhuk)


This PHP library will help you to work with your Pinterest account without using any API account credentials. 

There is a lot of pain using Pinterest API: to get an access to it you need to go to register a developer account, then 
register your application, then wait for confirmation, and only then you will get an access token. Furthermore, the
public API is very poor and has a very limited set of features. 

With this library you have the entire set of functions, which available on Pinterest website. And there is **no need to register an
application to receive an access token**. Just use your account login and password, like you do it in your browser. But even your account is not required,
if you don't use such operations as creating pins, writing comments or sending messages!

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Examples](#examples)
- [Account](docs/account.md)
- [Boards](docs/boards.md)
- [Pins](docs/pins.md)
- [Pinners](docs/pinners.md)
- [Interests and topics](docs/interests-and-topics.md)
- [Search](docs/search.md)
- [Inbox](docs/inbox.md)
- [Keywords](docs/keywords.md)
- [Errors handling](#errors-handling)
- [Use proxy](#use-proxy)
- [Custom request settings](#custom-request-settings)
- [Cookies](#cookies)
- [Pagination](#pagination)


## Installation

### Dependencies
Library requires CURL extension and PHP 5.5.9 or above.

The recommended way to install this library is via [Composer](https://getcomposer.org). 
[New to Composer?](https://getcomposer.org/doc/00-intro.md)

```
composer require seregazhuk/pinterest-bot
```

## Quick Start

```php 
// You may need to amend this path to locate Composer's autoloader
require('vendor/autoload.php'); 

use seregazhuk\PinterestBot\Factories\PinterestBot;

$bot = PinterestBot::create();

// Login
$bot->auth->login('mypinterestlogin', 'mypinterestpassword');

// Get lists of your boards
$boards = $bot->boards->forUser('yourUserName');

// Create a pin
$bot->pins->create('http://exmaple.com/image.jpg', $boards[0]['id'], 'Pin description');
```

*Note*: Some methods use pinterest navigation through results (with bookmarks), for example, get user followers/following, pins
likes/dislikes, search and other feed queries. This means that for every batch of results there will be a 
request to Pinterest. These methods return a [Pagination](#pagination) object with Pinterest api results.

**How to avoid banned from Pinterest**: don't bee too aggressive making pins or writing comments.
Try to put some timeouts with `$bot->wait($seconds)` calls, so you will behave like a real person and not a bot,
creating hundreds of pins in a minute.

## Examples
Here you can find articles with some examples of the most common tasks that can be done with the bot:

 - [Automate pinning](http://seregazhuk.github.io/2017/03/25/build-pinterest-bot-with-php-auto-pin/) ([source code](examples/auto_pins.php))
 - [Multiple Accounts and Proxy](http://seregazhuk.github.io/2017/03/28/build-printerest-bot-with-php-multiple-accounts/) ([source code](examples/multiple_accounts_and_proxy.php))
 - [Comments, Likes And Repins](http://seregazhuk.github.io/2017/03/30/build-pinterest-bot-with-php-comments-and-repins/) ([source code](examples/comments_likes_repins.php))
 - [Followers](http://seregazhuk.github.io/2017/04/01/build-pinterest-bit-with-php-followers/) ([source code](examples/followers.php))
 - [Parsing Pins](http://seregazhuk.github.io/2017/04/04/build-pinterest-bot-with-php-parsing-pins/) ([source code](examples/pins_parser.php))


## Errors handling
You can check for occurred errors after requests with method *getLastError()*. It returns
string that contains error from you last request to API:
 
```php
$error = $bot->getLastError();
echo $error;
```

## Use proxy

To set up proxy settings use *useProxy* method:
```php
$bot->getHttpClient()->useProxy('192.168.1.1', '12345');
```

By default it uses *http* proxy without authentication. If your 
proxy requires authentication, pass auth string as the third parameter:

```php
$bot->getHttpClient()->useProxy('192.168.1.1', '12345', 'username:password');
```

Use *socks* proxy:

```php
$bot->getHttpClient()->useSocksProxy('192.168.1.1', '12345');

// With authentication
$bot->getHttpClient()->useSocksProxy('192.168.1.1', '12345', 'username:password');
```

If you need to stop sending requests via proxy:
```php
$bot->getHttpClient()->dontUseProxy();
```

Check if bot uses proxy:
```php
if($bot->getHttpClient()->usesProxy()) {
    // ...
}
```

## Custom request settings

It is possible to add some additional Curl options for bot requests. For example, you can
set proxy and User Agent like this:

```php
$bot->getHttpClient()->setOptions([
    CURLOPT_PROXY => 'xx.xx.xxx.xx:xxxx',
    CURLOPT_PROXYTYPE => CURLPROXY_HTTP // Or CURLPROXY_SOCKS5,
    CURLOPT_USERAGENT => 'Your_User_Agent',
]);
```

With every request Pinterest returns an array with your current client info, with such info as 
OS, browser, IP and others:

```php
$info = $bot->getClientInfo();
```

By default it uses client info from the last request. To reload client context pass `true` argument:

```php
// Force to reload client info
$info = $bot->getClientInfo(true);
```

You can get an url of the last visited page:
```php
$url = $bot->getHttpClient()->getCurrentUrl();
```

## Cookies

Current bot cookies are available through getHttpClient and cookie/cookies methods.
All cookies:

```php
$cookies = $bot->getHttpClient()->cookies();
```

Cookie value by name:
```php
$someCookieValue = $bot->getHttpClient()->cookie('cookieName');
```

By default cookie files are stored in your system temp directory. You can set custom path to store cookies. 
**Notice!** This path must have write permissions:

```php
$bot->getHttpClient()->setCookiesPath($yourCustomPathForCookies);

$currentPath = $bot->getHttpClient()->getCookiesPath();
```

Remove your cookies:
```php
$bot->getHttpClient()->removeCookies();
```

Visit (click) a link. For example, when Pinterest sends you email with some link, and you want bot to visit it:
```
$bot->user->visitPage($url);
```

## Pagination

Most of methods use Pinterest pagination. For example, when you run `$bot->pins->search('query')`, Pinterest returns
only 20 results for request, you cannot get all the pins at once with only one request. So these methods return
`Pagination` object. You can iterate over it to get results:

```php
$pagination = $bot->pins->search('query');

foreach ($pagination as $pin) {
    // ...
}
 ```

Or you can grab all results at once as an array, but it will require some time, to loop through all Pinterest pages to get these results:
```php
$pagination = $bot->pins->search('query');

$results = $pagination->toArray();
// Or
$results = $bot->pins->search('query')->toArray();
```

By default methods return the first 50 results. For example, `$bot->pins->search('query')` will return only first 50
pins. But you can specify another limit num as a second argument. Or pass 0
for no limit. For example,
```php
foreach ($bot->pins->search('query', 20) as $pin) {
    // ...
}
```

Will return only 20 pins of the search results.

Limit and offset in results:
```php
// Skip first 50 results
$results = $bot->pins
    ->search('query')
    ->skip(50)
    ->get();

// Skip first 50 results, and then take 20
$results = $bot->pins
    ->search('query')
    ->take(20)
    ->skip(50)
    ->get();
```

To get all results pass `0` in `take()` method.

## How can I thank you?
Why not star the GitHub repo? I'd love the attention!
And you can donate project on PayPal.

[![Support me with some coffee](https://img.shields.io/badge/donate-paypal-orange.svg)](https://www.paypal.me/seregazhuk)

Thanks! 
