To set up the Laravel-AdminLTE package by jeroennoten, follow these steps:

---

### ✅ Requirements

* **Laravel**: Version 8 or higher
* **PHP**: Version 7.3 or higher([github.com][1], [github.com][2])

---

### 📦 Installation Steps

1. **Install the Package via Composer**

   In your Laravel project directory, run:

   ```bash
   composer require jeroennoten/laravel-adminlte
   ```



2. **Publish Package Assets**

   To publish the necessary assets, configuration, and views, execute:

   ```bash
   php artisan adminlte:install
   ```



You can customize the installation using options:

* `--force`: Overwrite existing files
* `--interactive`: Guide through the installation process
* `--only=auth_views`: Install only authentication views([github.com][2], [github.com][3], [github.com][4])

To check the installation status:

```bash
php artisan adminlte:status
```



3. **Optional: Install Authentication Scaffolding**

   If you plan to use AdminLTE-styled authentication views:

   ```bash
   composer require laravel/ui
   php artisan ui bootstrap --auth
   php artisan adminlte:install --only=auth_views
   ```



This sets up login, registration, and password reset views styled with AdminLTE.&#x20;

4. **Set Up Your Blade Views**

   To utilize the AdminLTE layout in your views:

   ```blade
   @extends('adminlte::page')
   ```



For a custom layout, create a base layout file (e.g., `resources/views/layouts/app.blade.php`) and extend it in your views.&#x20;

---

### ⚙️ Configuration

The package provides a configuration file located at `config/adminlte.php`. Key configuration options include:

* **Title and Logo**: Set the dashboard title and logo.
* **User Menu**: Enable and customize the user menu.
* **URLs**: Define routes for dashboard, logout, login, and registration.
* **Plugins**: Manage JavaScript and CSS plugins.([github.com][5], [github.com][6])

For detailed configuration options, refer to the [Basic Configuration](https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Basic-Configuration) and [Plugins Configuration](https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Plugins-Configuration) sections of the documentation.

---

### 🧭 Menu Configuration

Customize the sidebar and top navigation menus by editing the `menu` array in `config/adminlte.php`. Each menu item can have attributes like:([github.com][7])

* `text`: Display text
* `url` or `route`: Navigation link
* `icon`: Font Awesome icon class
* `submenu`: Nested menu items([github.com][4], [github.com][8], [github.com][7])

For a comprehensive guide, visit the [Menu Configuration](https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Menu-Configuration) documentation.([github.com][7])

---

### 🧪 Testing the Setup

After completing the installation and configuration:([github.com][9])

1. Start the Laravel development server:

   ```bash
   php artisan serve
   ```



2. Navigate to `http://localhost:8000` in your browser.

You should see the AdminLTE dashboard integrated into your Laravel application.([github.com][10])

---

For more detailed information and advanced customization, refer to the [Laravel-AdminLTE Documentation](https://github.com/jeroennoten/Laravel-AdminLTE/wiki).

[1]: https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Usage?utm_source=chatgpt.com "Usage · jeroennoten/Laravel-AdminLTE Wiki - GitHub"
[2]: https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Installation/78a59b2f1166e3b43303d9e6fde1d4d1dbcd2498?utm_source=chatgpt.com "Installation · jeroennoten/Laravel-AdminLTE Wiki - GitHub"
[3]: https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Installation?utm_source=chatgpt.com "Installation · jeroennoten/Laravel-AdminLTE Wiki - GitHub"
[4]: https://github.com/SergeyHub/laravel-admin-lte?utm_source=chatgpt.com "GitHub - SergeyHub/laravel-admin-lte: Example how use jeroennoten ..."
[5]: https://github.com/jeroennoten/Laravel-AdminLTE/wiki?utm_source=chatgpt.com "Home · jeroennoten/Laravel-AdminLTE Wiki - GitHub"
[6]: https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Basic-Configuration?utm_source=chatgpt.com "Basic Configuration · jeroennoten/Laravel-AdminLTE Wiki - GitHub"
[7]: https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Menu-Configuration?utm_source=chatgpt.com "Menu Configuration · jeroennoten/Laravel-AdminLTE Wiki - GitHub"
[8]: https://github.com/jeroennoten/Laravel-AdminLTE/wiki/Installation/003add8d51f20ec65200a26fa21e30ba80b2a85b?utm_source=chatgpt.com "Installation · jeroennoten/Laravel-AdminLTE Wiki - GitHub"
[9]: https://github.com/jeroennoten/Laravel-AdminLTE/issues/745?utm_source=chatgpt.com "Intractive installation doesn't work · Issue #745 · jeroennoten/Laravel ..."
[10]: https://github.com/jeroennoten/Laravel-AdminLTE?utm_source=chatgpt.com "Easy AdminLTE integration with Laravel - GitHub"
