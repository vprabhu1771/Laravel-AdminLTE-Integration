To apply **badge counts to all items in the `config/adminlte.php` menu config**, you need to **dynamically generate the menu** in a service provider **at runtime**, instead of defining a static menu in the config file.

### ✅ Why?

The `config/adminlte.php` is loaded **before** Laravel's request lifecycle (i.e. no `auth()`, `session()`, `DB` access). So we can't directly call `Model::count()` or similar functions in it.

---

### ✅ Solution: Dynamically build the menu in a Service Provider

---

### 🛠 Step 1: Remove `menu` from `config/adminlte.php`

In `config/adminlte.php`, **remove** the static `menu` definition or replace it with an empty array:

```php
'menu' => [],
```

---

### 🛠 Step 2: Create `AppServiceProvider` or use a new one like `AdminMenuServiceProvider`

```bash
php artisan make:provider AdminMenuServiceProvider
```

Then in `boot()`:

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

use Illuminate\Support\Facades\Event;
use Illuminate\Support\Facades\View;
use JeroenNoten\LaravelAdminLte\Events\BuildingMenu;

use Spatie\Permission\Models\Role;
use App\Models\School;
use App\Models\Student;

class AdminMenuServiceProvider extends ServiceProvider
{
    /**
     * Register services.
     */
    public function register(): void
    {
        //
    }

    /**
     * Bootstrap services.
     */
    public function boot(): void
    {
        Event::listen(BuildingMenu::class, function (BuildingMenu $event) {
            $event->menu->add(
                [
                    'text' => 'Roles',
                    'url' => 'admin/roles',
                    'icon' => 'fas fa-user-shield',
                    'label' => Role::count(),
                    'label_color' => 'primary',
                ],
                [
                    'text' => 'Schools',
                    'url' => 'admin/schools',
                    'icon' => 'fas fa-school',
                    'label' => School::count(),
                    'label_color' => 'info',
                ],
                [
                    'text' => 'Students',
                    'url' => 'admin/students',
                    'icon' => 'fas fa-users',
                    'label' => Student::count(),
                    'label_color' => 'success',
                ],
                // add more items here
            );
        });
    }
}
```

You can also create arrays like:

```php
$event->menu->add([
    'text' => 'Student Management',
    'icon' => 'fas fa-user-graduate',
    'submenu' => [
        [
            'text' => 'Enquiries',
            'url'  => 'admin/enquiries',
            'label' => \App\Models\Enquiry::count(),
            'label_color' => 'warning',
        ],
        [
            'text' => 'Admission Forms',
            'url'  => 'admin/admissions',
            'label' => \App\Models\Admission::count(),
            'label_color' => 'info',
        ],
    ],
]);
```

---

### 🛠 Step 3: Register the Provider in `config/app.php`

Add it to the `providers` array:

```php
App\Providers\AdminMenuServiceProvider::class,
```

---

### ✅ Final Output

Now your menu badges are **live counts**, dynamically generated on **every sidebar render**. 🎯

---

### 💡 Bonus: Cache counts for performance

If performance is a concern, cache the counts:

```php
'label' => cache()->remember('roles_count', now()->addMinutes(10), fn() => Role::count()),
```

---

Would you like a full working example with all sections like “Asset Management”, “Transport Management”, etc.?
