Teste Longevo

# configurações para subir o ambiente:
```
dbuser: longevo
passwd: longevo
dbname: longevo
porta:  5432
host:   127.0.0.1
```

As pastas de cache, logs e sessão foram movidas para fora da aplicação
O app esta configurado para user como pasta 'var' o seguinte diretorio:
/var/longevo/

# execute os seguintes comandos:
```
mkdir /var/longevo
mkdir /var/longevo/cache
mkdir /var/longevo/logs/
mkdir /var/longevo/session

#para distros baseadas em centOS
chown -r httpd:httpd /var/longevo

#distros baseadas em debian
chown -r www-data:www-data /var/longevo
```

# Config do virtualhost no apache:
```
<VirtualHost *:80>
    ServerName longevo.app
    ServerAlias www.longevo.app

    DocumentRoot /var/www/html/longevo/web
    <Directory /var/www/html/longevo/web>
        AllowOverride All
        Order Allow,Deny
        Allow from All

        <IfModule mod_rewrite.c>
            Options -MultiViews
            RewriteEngine On
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteRule ^(.*)$ app.php [QSA,L]
        </IfModule>

    </Directory>

    # uncomment the following lines if you install assets as symlinks
    # or run into problems when compiling LESS/Sass/CoffeeScript assets
    # <Directory /var/www/project>
    #     Options FollowSymlinks
    # </Directory>

    <Directory /var/www/html/longevo/web/bundles>
        <IfModule mod_rewrite.c>
            RewriteEngine Off
        </IfModule>
    </Directory>

    ErrorLog /var/log/httpd/app_error.log
    CustomLog /var/log/httpd/project_access.log combined
</VirtualHost>
```

Estou disponibilizando tambem uma box do vagrant com todo o ambiente configurado. O arquivo [longevo.box](https://drive.google.com/file/d/0B9wgAoJqgzGeQnFSOVlGWk1SN3c/view?usp=sharing).
É uma box do CentOS para vagrant do tipo virtual box, todo o ambiente esta configurado, só precisa adicionar no hosts da maquina externa:
127.0.0.1 longevo.app
segue o VagrantFile dela:
```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network :forwarded_port, guest: 80, host: 4567
end
```
para acessar a a aplicação: longevo.app:4567

Qualquer duvida me envie um email.
