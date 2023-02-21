# Common Problems

## File URL

### Can't load JS / CSS / images

For asset folder in root folder (outside application), use base_url() for every link

```
<? echo base_url(); ?>/asset_folder/my_style.css
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

make .htaccess in root folder

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```

### Dynamic base url

open config.php

```
$config['base_url'] = ((isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == "on") ? "https" : "http");
$config['base_url'] .= "://" . $_SERVER['HTTP_HOST'];
$config['base_url'] .= str_replace(basename($_SERVER['SCRIPT_NAME']), "", $_SERVER['SCRIPT_NAME']);
```

## NGINX code igniter problem resolve

### Can't access controller manually (ex : localhost/codeigniter/index.php/welcome/index )

Open 00-default.conf

search for this line near line 20

```
location ~ \.php$ { ... }
```

and then append this below

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
```

This fix also applies for W/O using HMVC.
