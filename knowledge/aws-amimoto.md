## wp-setup

```
$ wp-setup ドメイン名
```


## ベーシック認証

.htpasswd ファイルを生成

```
$ htpasswd -c .htpasswd user
$ New password:
$ Re-type new password:
```

nginx `/etc/nginx/conf.d/example.com.conf` の設定

```
location / {
    try_files $uri @wordpress;

    # Pass all .php files onto a php-fpm/php-fcgi server.
    location ~ \.php$ {
        try_files $uri @wordpress;

        ...
        ...
        ...

    }

    # Basic authentication
    auth_basic "User ID and password are required";
    auth_basic_user_file /var/www/vhosts/mgn.m-g-n.me/.htpasswd;
}
```

ログイン画面にベーシック認証を掛ける場合

```
location ~* /wp-login\.php|/wp-admin/((?!admin-ajax\.php).)*$ {
    index index.php index.html index.htm;

    # Basic authentication
    auth_basic "User ID and password are required";
    auth_basic_user_file /var/www/vhosts/mgn.m-g-n.me/.htpasswd;

}
```

最後に nginx を再起動。普通は `sudo` が必要。

```
$ service nginx restart
```

参考: [WordPressの管理画面にアクセス制限をかけたい](http://ja.amimoto-ami.com/faq/wordpress-admin-access-control/)
