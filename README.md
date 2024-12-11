# Install Multiple PHP Versions on Fedora

This guide will walk you through installing multiple PHP versions (PHP 7.4, PHP 8.0, and PHP 8.2) on Fedora, and configuring the `alternatives` system to switch between them easily.

## Requirements

Make sure your system is updated and you have the necessary tools installed:

```bash
sudo dnf update -y
sudo dnf install -y dnf-utils
```

## Step 1: Add Remi Repository

First, you need to install the **Remi** repository, which provides multiple PHP versions.

1. Install `dnf-utils` to manage repositories:

   ```bash
   sudo dnf install -y dnf-utils
   ```

2. Add the Remi repository for Fedora:

   ```bash
   sudo dnf install -y https://rpms.remirepo.net/fedora/remi-release-$(rpm -E %fedora).rpm
   ```

3. Enable the Remi repository:

   ```bash
   sudo dnf config-manager --set-enabled remi
   ```

## Step 2: Enable PHP Modules

You can enable the PHP modules for versions 7.4, 8.0, and 8.2:

```bash
sudo dnf module enable php:remi-8.* -y
```

This will ensure that the PHP modules for all versions are available for installation.

## Step 3: Install Multiple PHP Versions

Now, install the PHP versions you need (PHP 7.4, 8.0, and 8.2) along with commonly used PHP extensions.

1. **Install PHP 7.4:**

   ```bash
   sudo dnf install php74 php74-cli php74-fpm php74-common php74-mysqlnd php74-json php74-opcache -y
   ```

2. **Install PHP 8.0:**

   ```bash
   sudo dnf install php80 php80-cli php80-fpm php80-common php80-mysqlnd php80-json php80-opcache -y
   ```

3. **Install PHP 8.2:**

   ```bash
   sudo dnf install php82 php82-cli php82-fpm php82-common php82-mysqlnd php82-json php82-opcache -y
   ```

## Step 4: Configure Alternatives

To switch between multiple PHP versions, you need to configure **alternatives** to handle different versions of PHP.

### 1. Remove Existing PHP (if exists)

If you already have PHP installed, remove the existing PHP binary to avoid conflicts:

```bash
sudo rm /usr/bin/php
```

### 2. Add PHP Versions to Alternatives

Add each PHP version to the alternatives system:

```bash
sudo alternatives --install /usr/bin/php php /usr/bin/php74 1
sudo alternatives --install /usr/bin/php php /usr/bin/php80 2
sudo alternatives --install /usr/bin/php php /usr/bin/php82 3
```

The `1`, `2`, and `3` numbers are priority values. The lower the number, the higher the priority.

### 3. Select Active PHP Version

To choose the active PHP version, use the following command:

```bash
sudo alternatives --config php
```

You will see a prompt like this:

```bash
There is 3 programs that provide 'php'.
  Selection    Command
  -----------------------------------------------
  *+ 1         /usr/bin/php74
     2         /usr/bin/php80
     3         /usr/bin/php82
Enter to keep the current selection[+], or type selection number:
```

Enter the number corresponding to the version you want to use. For example, enter `2` to select PHP 8.0.

## Step 5: Verify PHP Version

To confirm the active PHP version, run:

```bash
php -v
```

This will display the currently active PHP version, for example:

```bash
PHP 8.2.26 (cli) (built: Nov 19 2024 17:11:09) (NTS gcc x86_64)
Zend Engine v4.2.26, Copyright (c) Zend Technologies
    with Zend OPcache v8.2.26, Copyright (c), by Zend Technologies
```

## Optional: Configure PHP-FPM for Web Servers

If you're using **PHP-FPM** with Nginx or Apache, ensure each PHP version has its own PHP-FPM configuration. For example, you can start PHP-FPM for PHP 8.2 with:

```bash
sudo systemctl start php82-fpm
sudo systemctl enable php82-fpm
```

Repeat for other PHP versions as needed (PHP 7.4, PHP 8.0).

## Conclusion

With these steps, you can install multiple PHP versions on Fedora and switch between them easily using `alternatives`. This allows you to run different PHP versions for different projects or environments.

If you encounter any issues, feel free to open an issue or ask for help.
