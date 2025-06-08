```
php artisan adminlte:install --only=main_views
```

```
php artisan adminlte:install --only=auth_views
```

Then edit the file:

```
<!-- resources/views/vendor/adminlte/auth/login.blade.php -->

<x-adminlte-input name="shop_id" label="Shop ID" type="number" required autofocus />

<x-adminlte-input name="email" label="Email" type="email" required />

<x-adminlte-input name="password" label="Password" type="password" required />
```
