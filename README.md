# Introduction

Moodle - the world's open source learning platform

Moodle <https://moodle.org> is a learning platform designed to provide
educators, administrators and learners with a single robust, secure and
integrated system to create personalised learning environments.

You can download Moodle <https://download.moodle.org> and run it on your own
web server, ask one of our Moodle Partners <https://moodle.com/partners/> to
assist you, or have a MoodleCloud site <https://moodle.com/cloud/> set up for
you.

Moodle is widely used around the world by universities, schools, companies and
all manner of organisations and individuals.

Moodle is provided freely as open source software, under the GNU General Public
License <https://docs.moodle.org/dev/License>.

Moodle is written in PHP and JavaScript and uses an SQL database for storing
the data.

See <https://docs.moodle.org> for details of Moodle's many features.


# How to install

**ATENTION!** You must have the 'govdados01' data directory in order to run this moodle plataform.
  
## 1. Local machine 
### Installing the Apache, MySQL, PHP (LAMP) Stack on Ubuntu
See [this article from DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-22-04#step-1-installing-apache-and-updating-the-firewall) for details:

* **Installing Apache**

```sudo apt update```

```sudo apt install apache2```


```sudo ufw allow in "Apache"```: Allowing traffic only on port 80, use the Apache profile

* **Installing MySQL**

```sudo apt install mysql-server```

```sudo mysql_secure_installation```

* **Installing PHP**

```sudo apt install php libapache2-mod-php php-mysql```


* **Creating a Virtual Host for your Website**

```sudo mkdir /var/www/<your_domain>```

```sudo chown -R $USER:$USER /var/www/<your_domain>```

In ```sudo nano /etc/apache2/sites-available/<your_domain>.conf``` file, insert this code:

```
<VirtualHost *:80>
    ServerName your_domain
    ServerAlias www.your_domain 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```sudo a2ensite <your_domain>```

```sudo a2dissite 000-default```

```sudo apache2ctl configtest```

```sudo systemctl reload apache2```

#### Opcional phases (only testing)

* **Testing the Virtual Host** 

In ```nano /var/www/your_domain/index.html``` file, insert this code:

```
<html>
  <head>
    <title>your_domain website</title>
  </head>
  <body>
    <h1>Hello World!</h1>

    <p>This is the landing page of <strong>your_domain</strong>.</p>
  </body>
</html>
```

* **Testing PHP Processing on your Web Server Testing and Database Connection from PHP**

In ```nano /var/www/your_domain/info.php``` file, insert this code:
```
<?php
phpinfo();
```

```sudo mysql```

```CREATE DATABASE <example_database>;```

```CREATE USER 'example_user'@'%' IDENTIFIED BY 'password';```

```GRANT ALL ON example_database.* TO 'example_user'@'%';```

```exit```

For access the MySQL with ``<example-user>``: ```mysql -u example_user -p``` and insert this code for create a table:

```CREATE TABLE example_database.todo_list (
	item_id INT AUTO_INCREMENT,
	content VARCHAR(255),
	PRIMARY KEY(item_id) );
```

```INSERT INTO example_database.todo_list (content) VALUES ("My first important item");```

```exit```

in ```nano /var/www/your_domain/todo_list.php``` file, insert this code:
```
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>"; 
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```

Run this code for see the output: ```http://<your_domain_or_IP>/todo_list.php```


### Creating the Moodle Database

With root acess, after running ```mysql -u root -p``` (see [this forum](https://stackoverflow.com/questions/21944936/error-1045-28000-access-denied-for-user-rootlocalhost-using-password-y/42967789#42967789) for password troubleshooting), run this code for setting a moodle user:
```
CREATE DATABASE wethink_mood763;

CREATE USER 'moodleuser'@'%' IDENTIFIED BY 'password';

GRANT ALL ON wethink_mood763.* TO 'moodleuser'@'%';
```

After that, insert this code import all database data from `wethink_mood763.sql` file: 

```mysql -u moodleuser -p wethink_mood763 < wethink_mood763.sql```
