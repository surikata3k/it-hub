# Moodle installation (local)

Install Moodle on Ubuntu 26.04 with Apache, MySQL and PHP 8.5, reachable at `http://moodle.local`.

## 1. Install packages

```bash
sudo apt update
sudo apt install apache2 mysql-server git php8.5 libapache2-mod-php8.5 php8.5-mysql php8.5-cli php8.5-curl php8.5-gd php8.5-mbstring php8.5-xml php8.5-zip php8.5-intl php8.5-bcmath php8.5-soap
```

## 2. Configure `php.ini`

```bash
sudo nano /etc/php/8.5/apache2/php.ini
```

```ini
max_input_vars = 5000
pcre.jit = 0
```

## 3. Download Moodle

Clone the stable branch into `/var/www/moodle` and create the data folder outside the web root:

```bash
sudo git clone --depth 1 -b MOODLE_XXX_STABLE https://github.com/moodle/moodle.git /var/www/moodle
sudo mkdir /var/www/moodledata
```

!!! tip
    Replace `MOODLE_XXX_STABLE` with the current stable branch, for example `MOODLE_501_STABLE` for Moodle 5.1. Check that the version supports your PHP version in the [release notes](https://moodledev.io/general/releases).

## 4. Permissions

```bash
sudo chown -R www-data:www-data /var/www/moodle
sudo chown -R www-data:www-data /var/www/moodledata
sudo chmod -R 750 /var/www/moodledata
```

## 5. Add the local host

```bash
sudo nano /etc/hosts
```

Add the line:

```text title="/etc/hosts"
127.0.0.1 moodle.local
```

## 6. Create the database and user

```bash
sudo mysql
```

```sql
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 7. Configure the virtual host

```bash
sudo nano /etc/apache2/sites-available/moodle.conf
```

Since Moodle 5.1 the web root is the `public` folder, and `FallbackResource` sends the new routed URLs to `r.php`:

```apache title="/etc/apache2/sites-available/moodle.conf"
<VirtualHost *:80>
    ServerName moodle.local
    DocumentRoot /var/www/moodle/public

    <Directory /var/www/moodle/public>
        AllowOverride None
        Require all granted
        FallbackResource /r.php
    </Directory>
</VirtualHost>
```

Enable the site and restart Apache:

```bash
sudo a2dissite 000-default
sudo a2ensite moodle
sudo systemctl restart apache2
```

## 8. Run the installer

Open `http://moodle.local` in the browser and follow the steps:

- **Data directory:** `/var/www/moodledata`
- **Database driver:** MySQL (native/mysqli)
- **Database:** `moodle`, user `moodleuser`, password `123456`

The installer checks the server requirements, creates the tables and asks for the administrator account.
