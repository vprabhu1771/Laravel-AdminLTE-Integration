Here is a clean and complete **README** to help you (or your team) install and configure **Select2 with Bootstrap 4 theme** in a **Laravel AdminLTE** project.

---

## 📘 Laravel AdminLTE: Select2 with Bootstrap 4 Theme Setup

This guide shows how to install and configure **Select2** with the **Bootstrap 4 theme** inside a Laravel project using the [jeroennoten/laravel-adminlte](https://github.com/jeroennoten/laravel-adminlte) package.

---

### ✅ Step 1: Install Select2 plugin via Artisan

Run the following Artisan command to install the Select2 plugin using AdminLTE:

```bash
php artisan adminlte:plugins install --plugin=select2
```

✅ If successful, you'll see:

```
Select 2 with Bootstrap 4 Theme     | select2                 | Installed
```

---

### ✅ Step 2: Verify `config/adminlte.php`

Ensure the plugin is activated in your `config/adminlte.php` file:

```php
'plugins' => [
    'Select2' => [
        'active' => true,
        'files' => [
            [
                'type' => 'js',
                'asset' => false,
                'location' => '//cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/js/select2.min.js',
            ],
            [
                'type' => 'css',
                'asset' => false,
                'location' => '//cdn.jsdelivr.net/npm/select2@4.1.0-rc.0/dist/css/select2.min.css',
            ],
        ],
    ],
],
```

> ℹ️ Note: By default, AdminLTE uses **CDN** to load Select2 assets.

---

### ✅ Step 3: Add Bootstrap 4 Theme for Select2 (CDN)

Select2 **doesn't include the Bootstrap 4 theme by default**, so you need to include it manually:

```blade
@push('css')
<link href="https://cdn.jsdelivr.net/npm/@ttskch/select2-bootstrap4-theme@1.5.2/dist/select2-bootstrap4.min.css" rel="stylesheet">
@endpush
```

> Add this in your blade file (like `resources/views/vendor/adminlte/page.blade.php` or a specific view).

---

### ✅ Step 4: Create Select2 Form Field

Example usage in a Blade form:

```blade
<select class="form-control select2" style="width: 100%;">
    <option selected>Option 1</option>
    <option>Option 2</option>
</select>
```

---

### ✅ Step 5: Initialize Select2 with Bootstrap 4 Theme

```blade
@push('js')
<script>
    $(function () {
        $('.select2').select2({
            theme: 'bootstrap4'
        });
    });
</script>
@endpush
```

---

### ✅ Final View Example

```blade
@extends('adminlte::page')

@section('title', 'Select2 Example')

@section('content')
    <div class="card">
        <div class="card-body">
            <label for="example">Select Example</label>
            <select class="form-control select2" id="example" style="width: 100%;">
                <option value="1">Option One</option>
                <option value="2">Option Two</option>
            </select>
        </div>
    </div>
@endsection

@push('css')
<link href="https://cdn.jsdelivr.net/npm/@ttskch/select2-bootstrap4-theme@1.5.2/dist/select2-bootstrap4.min.css" rel="stylesheet">
@endpush

@push('js')
<script>
    $(function () {
        $('.select2').select2({
            theme: 'bootstrap4'
        });
    });
</script>
@endpush
```

---

### 📦 Optional: Install Select2 and Bootstrap 4 Theme Locally via NPM

If you prefer to load assets locally instead of from CDN:

```bash
npm install select2 bootstrap4-select2-theme
```

Then import them in `resources/js/app.js`:

```js
import 'select2';
import 'select2/dist/css/select2.css';
import 'bootstrap4-select2-theme/dist/select2-bootstrap4.min.css';
```

Compile with:

```bash
npm run dev
```

---

Let me know if you'd like this as a `README.md` file download.
