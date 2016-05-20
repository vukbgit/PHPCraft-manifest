### Prerequisites
* Apache2:
  * allow .htacces files use

### Basic folders and files
* into site root make a folder named __private__ with the subfolders __conf__ and __procedures__
* make file __private/.htaccess__ and prevent direct access to contained files:
```
# Order Deny,Allow
deny from all
```
* make the main configuration file which contains informations likely to be needed by every application (i.e. domains definition, database account), for portability it's useful to name it 'application', it has a tree structure so a php or json file is needed (use json if it has to be red by different technologies like PHP and js).
  make __private/application-name/conf/application.php__ (values to be adapted)
  ```php
  <?php
return [
    'contacts' => [
        'tech' => 'tech-email-address' //
    ],
    'domains' => [
        'dev.your-domain.tld' => [
            'environment' => 'development'
        ],
        'your-domain.tld' => [
            'environment' => 'production'
        ]
    ],
    'basePath' => '/',
    'languages' => ['it','en'],
    'database' => [
        'driver' => '_mysql_',
        'host' => '_localhost_',
        'username' => '_db-username_',
        'password' => '_db-password_',
        'database' => '_db-name_'
    ]
];
  ```
* make the bootstrap file __private/procedures/application-name.bootstrap.php__ with some test code:
```php
<?php
echo 'test bootstrap file';
```
* into site root make a folder named __public__ with subfolder __application-name__
* make file __public/index.php__:
  * store the name of the application (from now on 'application-name') and the relative path from __index.php__ to the top level of application. These informations are used from this point till the end of the script so they must be defined here
  * include the bootstrap file
```php
<?php
define('APPLICATION','application-name');
define('PATH_TO_ROOT','../../');
//bootstrap
require PATH_TO_ROOT . 'private/' . APPLICATION . '/' . $configuration['domains'][$_SERVER['HTTP_HOST']]['environment'] . '.bootstrap.php';
```
### __.htaccess__
```
Options +FollowSymlinks
RewriteEngine on
#default domain page
RewriteRule ^$ http://your-domain.tld/default-application-subject [R,L]
#requests pointing to real files are not redirected
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ public/application-name/index.php [NC]
```
### bootstrap file

  * configuration information specific to the domain logic ar that can take many forms (i.e. address that can be split or compact), use the most appropriate file type, in this case .ini:
  make __private/application-name/conf/application-name.ini__
  ```ini
phone = '__1234.5678.90__'
a-certain-property = 'its-value'
  ```