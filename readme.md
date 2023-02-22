# Common Problems

## File URL

### Can't load JS / CSS / images

For asset folder in root folder (outside application), use base_url() for every link

```
<? echo base_url(); ?>/asset_folder/more_folder/my_style.css
```

### Can't use base_url()

open autoload.php, make sure already included "url"

```
$autoload['helper'] = array('url')
```

### Remove index.php

open config.php

```
$config['index_page'] = '';
```

(apache, or nginx just make this just in case) make .htaccess in root folder

```
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    Options -Indexes
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php/$1 [L]
</IfModule>
```

### Dynamic base url

open config.php

```
$config['base_url'] = ((isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == "on") ? "https" : "http");
$config['base_url'] .= "://" . $_SERVER['HTTP_HOST'];
$config['base_url'] .= str_replace(basename($_SERVER['SCRIPT_NAME']), "", $_SERVER['SCRIPT_NAME']);
```

## NGINX code igniter problem resolve

### Laragon can't access controller manually or remove index.php (ex : localhost/codeigniter/index.php/welcome/index or codeigniter.test/welcome/index)

make codeigniter.conf in X:\laragon\etc\nginx\alias

```
location ~ [^/]\.php(?:$|/) {
	fastcgi_split_path_info ^(.+\.php)(.*)$;
	# fastcgi_pass   unix:/var/run/php/php7.4-fpm.sock;
	fastcgi_pass php_upstream;
	include        fastcgi_params;
	fastcgi_index  index.php;
	fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
	fastcgi_param  PATH_INFO  $fastcgi_path_info;
}

# set expiration of assets to MAX for caching
location ~* \.(ico|css|js|gif|jpe?g|png)(\?[0-9]+)?$ {
	expires max;
	log_not_found off;
}
```

This fix also applies for W/O using HMVC.


Please notice you still can't access <http://localhost/codeigniter/welcome>
but you can access <http://codeigniter.test/welcome> which what you want in production using your domain name.

## PHP version >8.0
### str_replace(): Passing null to parameter

just add ?? '' like below

```
$var = str_replace($something_1 ?? '', $something_2 ?? '')
```
