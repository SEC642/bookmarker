This is a vulnerable app used in SEC642 to demonstrate Mass Assignment

CakePHP Install:

sudo apt-get update -y
sudo apt-get install python-software-properties -y
sudo apt-get install aptitude php5 curl apache2 php5-intl -y
sudo apt-get install mysql-server mysql-client -y
sudo apt-get install php-mysql -y
curl -s https://getcomposer.org/installer | php
sudo a2enmod rewrite

Database tables have to inserted correctly so do the following in Mysql:


create database bookmarker;
use bookmarker;
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    username VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
    created DATETIME,
    modified DATETIME
);
CREATE TABLE bookmarks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(50),
    description TEXT,
    url TEXT,
    created DATETIME,
    modified DATETIME,
    FOREIGN KEY user_key (user_id) REFERENCES users(id)
);
CREATE TABLE tags (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    created DATETIME,
    modified DATETIME,
    UNIQUE KEY (title)
);
CREATE TABLE bookmarks_tags (
    bookmark_id INT NOT NULL,
    tag_id INT NOT NULL,
    PRIMARY KEY (bookmark_id, tag_id),
    FOREIGN KEY tag_key(tag_id) REFERENCES tags(id),
    FOREIGN KEY bookmark_key(bookmark_id) REFERENCES bookmarks(id)
);
create database test_bookmarker;
use test_bookmarker;
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    username VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
    created DATETIME,
    modified DATETIME
);
CREATE TABLE bookmarks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(50),
    description TEXT,
    url TEXT,
    created DATETIME,
    modified DATETIME,
    FOREIGN KEY user_key (user_id) REFERENCES users(id)
);
CREATE TABLE tags (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    created DATETIME,
    modified DATETIME,
    UNIQUE KEY (title)
);
CREATE TABLE bookmarks_tags (
    bookmark_id INT NOT NULL,
    tag_id INT NOT NULL,
    PRIMARY KEY (bookmark_id, tag_id),
    FOREIGN KEY tag_key(tag_id) REFERENCES tags(id),
    FOREIGN KEY bookmark_key(bookmark_id) REFERENCES bookmarks(id)
);




Here is my apache.conf file:

Change the <directory> tags to look like this:
<Directory />
        Options FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>

<Directory /usr/share>
        AllowOverride None
</Directory>

<Directory /var/www/>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
</Directory>

modify this file /etc/apache2/site-enabled/000-default.conf:

	DocumentRoot /var/www/html/webroot

I have my 'app' running in /var/www/html/

now in the /var/www/html/config/app.php change this section because the password and username will not WORK.

   'Datasources' => [
                'default' => [
                        'className' => 'Cake\Database\Connection',
                        'driver' => 'Cake\Database\Driver\Mysql',
                        'persistent' => false,
                        'host' => 'localhost',
                        'username' => 'root',
                        'password' => '',
                        'database' => 'bookmarker',
                        'encoding' => 'utf8',
                        'timezone' => 'UTC',
                        'cacheMetadata' => true,


    'test' => [
                        'className' => 'Cake\Database\Connection',
                        'driver' => 'Cake\Database\Driver\Mysql',
                        'persistent' => false,
                        'host' => 'localhost',
                        'username' => 'root',
                        'password' => '',
                        'database' => 'test_bookmarker',

App should run!
