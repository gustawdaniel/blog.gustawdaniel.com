---
title: Xss attack using script style and image
tags:
- xss
- hacking
- attack

---

## About project

This article describes examples of xss attack. Usage script tag
is probably most known case, but there also other possibilities.
You can change content of website on your own using image tag or
pure css.

This is educational material and you should remember than
hacking is illegal if you are caught red-handed. :)

## Website code

To present attack we create simple website based on php. I very 
like separate logic and view in code, but for simplicity and 
to minimize number of lines of code we mixed in and all website
code is placed in index.php. To get vulnerability website have to
be able to save text from user to database and display it on 
screen without filtering of this.

Again for the case of simplicity and clarity we abandon best 
practices and use json file instead of database. First file of
our project is `db.json`

> db.json

```json
["First comment","Second one"]
```

To save comment send by user php script do the following things:

> index.php

```php
<?php
$comments = json_decode(file_get_contents('db.json'));

if($_SERVER["REQUEST_METHOD"] === "POST") {
    $comments[] = $_POST["comment"];
    file_put_contents('db.json', json_encode($comments));
}
```

- Read content of `db.json` file and convert it to php array.
- Check if user send request by method POST - it means send form
- If yes
  - Append comment send by user to array
  - Override file `db.json` by json encoding array with new comment

Independent from method of request script goes on and display
form adn list of comments

> index.php

```php
echo '<form action="" method="post">
    <input type="text" name="comment">
    <input type="submit" value="send">
</form>
<ul>';

foreach ($comments as $comment) {
    echo "<li>".$comment."</li>";
}
echo '</ul>';
```

Created website looks as the following

[![Zrzut_ekranu_z_2018-02-20_09-06-53.png](https://s17.postimg.org/be5low4db/Zrzut_ekranu_z_2018-02-20_09-06-53.png)](https://postimg.org/image/s1x3rdz4r/)

It is fully functional, allow to add comment, save it in json 
and display list of comments. If users want to add text, not 
hack it could be end of our adventure. But we should assume 
that at least one user of website want to hack it. :)

## How to hack it?

This flow of data - saving on server and displaying on client
make possible xss attack if text is not properly filtered. XSS 
means Cross-site scripting and enables attackers to inject 
client-side scripts into web pages viewed by other users.

Appended executable code is interpreted by browser, not server
so we cannot to conquer server by it, but can exchange client
behaviour. Exemplary benefits for attacker are ths following:

- stealing cookies (session) - taking control over (logged in) session of victim
- dynamic change of website content
- enabling key logger in browser

Script can be stored on server or included in 
link. In our case wa want to save script to json file by typing
comment. We are interested in change content of website to
"Hacked by Daniel". In any case of presented below attack method
website will looks like this:

[![Zrzut_ekranu_z_2018-02-20_09-30-24.png](https://s17.postimg.org/t6r5wnqpr/Zrzut_ekranu_z_2018-02-20_09-30-24.png)](https://postimg.org/image/padu0o5q3/)

### Script

The simplest way is append script that dynamically after sile load
change his content to required. Try add comment:

```html
<script>document.querySelector('html').innerHTML="Hacked By Daniel"</script>
```

This code select `html` - it means all page, and chane his contend
using `innerHTML` property.

### Style

Other method works even if javascript tags are stripped and
javascript is disabled in browser.

```html
<style>html::before {content: "Hacked By Daniel";} body {display: none;}</style>
```

We defined two rules for styling of website. First one tell browser
to append text `Hacked By Daniel` before body of website. Second one
to do not display body.

### Image

Of course if we block `script` tag and `style` tag in our comments
it is not enough, because of we can run script also in other tags.

```html
<img src=undefined onerror='document.querySelector("html").innerHTML="Hacked By Daniel"'>
```

This is example of image that have invalid address. If address is 
invalid browser run script being value of attribute `onerror`.

## How to defense?

To defend this attack we need to filter comments of our users 
and strip html tags. We can do it changing code in  index.php` 
as below

```diff
-      $comments[] = $_POST["comment"];
+      $comments[] = htmlspecialchars($_POST["comment"]);
```

After applying this fix text written in form will be 
displayed in comments lists literally equal text typed by user,
and will be not interpreted as html tag.

[![Zrzut_ekranu_z_2018-02-20_09-55-15.png](https://s17.postimg.org/yyra3jl1b/Zrzut_ekranu_z_2018-02-20_09-55-15.png)](https://postimg.org/image/yyra3jl17/)

## Summary

We showed simple examples of xss attack. If you use framework like
Symfony then framework have security mechanism build in his structure,
but you should remember about `htmlspecialchars` function if you
write in pure php.