## Apache Configuration for Laravel

Laravel requires `mod_rewrite` enabled and the `DocumentRoot` pointed specifically to the `public/` directory of your project for security and proper routing.

### 1. Configure the Virtual Host

Open the Apache default configuration file (or your custom site configuration file):

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Replace or update the file contents with the production-ready template below. Make sure to update `/var/www/html/your-project-folder` to your actual project path:

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    # Crucial: Point this directly to Laravel's public directory
    DocumentRoot /var/www/html/your-project-folder/public

    <Directory /var/www/html/your-project-folder/public>
        Options Indexes FollowSymLinks
        # Allows Laravel's public/.htaccess file to handle URL routing
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog \${APACHE_LOG_DIR}/laravel_error.log
    CustomLog \${APACHE_LOG_DIR}/laravel_access.log combined
</VirtualHost>
```

### 2. Enable Rewrite Module & Apply Changes

Run the following commands to enable URL rewriting, test your configuration syntax, and safely restart the Apache web server:

```bash
# 1. Enable the Apache rewrite module (required for clean Laravel URLs)
sudo a2enmod rewrite

# 2. Test configuration files for syntax errors
sudo apache2ctl configtest

# 3. If "Syntax OK" is returned, restart Apache to apply changes
sudo systemctl restart apache2
```

### 3. File Permissions (Troubleshooting 500 Errors)
If you encounter a `500 Internal Server Error` after setup, ensure Apache has permission to write to Laravel's storage and cache directories:

```bash
sudo chown -R www-data:www-data /var/www/html/your-project-folder/storage
sudo chown -R www-data:www-data /var/www/html/your-project-folder/bootstrap/cache
```
