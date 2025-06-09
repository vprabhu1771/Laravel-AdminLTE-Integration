Great! Here's how you can implement **Permission CRUD** using `Spatie\Permission\Models\Permission` in Laravel, following the same structure used for Roles.

---

### ✅ 1. **Create the Seeder**

```bash
php artisan make:seeder PermissionSeeder
```

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Spatie\Permission\Models\Permission;

class PermissionSeeder extends Seeder
{
    public function run(): void
    {
        $permissions = ['manage users', 'manage roles', 'manage permissions'];

        foreach ($permissions as $perm) {
            Permission::firstOrCreate(['name' => $perm]);
        }
    }
}
```

Run the seeder:

```bash
php artisan db:seed --class=PermissionSeeder
```

---

### ✅ 2. **Create PermissionController**

```bash
php artisan make:controller PermissionController --resource
```

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Spatie\Permission\Models\Permission;

class PermissionController extends Controller
{
    public function index()
    {
        $permissions = Permission::all();
        return view('permissions.index', compact('permissions'));
    }

    public function create()
    {
        return view('permissions.create');
    }

    public function store(Request $request)
    {
        $request->validate(['name' => 'required|unique:permissions']);
        Permission::create(['name' => $request->name]);

        return redirect()->route('permissions.index')->with('success', 'Permission created successfully.');
    }

    public function edit(Permission $permission)
    {
        return view('permissions.edit', compact('permission'));
    }

    public function update(Request $request, Permission $permission)
    {
        $request->validate(['name' => 'required|unique:permissions,name,' . $permission->id]);
        $permission->update(['name' => $request->name]);

        return redirect()->route('permissions.index')->with('success', 'Permission updated successfully.');
    }

    public function destroy(Permission $permission)
    {
        $permission->delete();
        return redirect()->route('permissions.index')->with('success', 'Permission deleted successfully.');
    }
}
```

---

### ✅ 3. **Define Routes**

In `routes/web.php`:

```php
use App\Http\Controllers\PermissionController;

Route::resource('permissions', PermissionController::class);
```

---

### ✅ 4. **Create Blade Views**

Inside `resources/views/permissions/`:

#### 📄 `index.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Permissions')

@section('content')
    <h1>Permissions</h1>

    <a href="{{ route('permissions.create') }}" class="btn btn-primary mb-2">Add Permission</a>

    @if (session('success'))
        <div class="alert alert-success">{{ session('success') }}</div>
    @endif

    <table class="table table-bordered">
        <thead>
            <tr>
                <th>Name</th>
                <th width="150">Actions</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($permissions as $permission)
                <tr>
                    <td>{{ $permission->name }}</td>
                    <td>
                        <a href="{{ route('permissions.edit', $permission) }}" class="btn btn-sm btn-warning">Edit</a>
                        <form action="{{ route('permissions.destroy', $permission) }}" method="POST" style="display:inline;">
                            @csrf @method('DELETE')
                            <button class="btn btn-sm btn-danger" onclick="return confirm('Delete this permission?')">Delete</button>
                        </form>
                    </td>
                </tr>
            @endforeach
        </tbody>
    </table>
@endsection
```

#### 📄 `create.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Create Permission')

@section('content')
    <h1>Create Permission</h1>

    <form action="{{ route('permissions.store') }}" method="POST">
        @csrf
        @include('permissions.form')
    </form>
@endsection
```

#### 📄 `edit.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Edit Permission')

@section('content')
    <h1>Edit Permission</h1>

    <form action="{{ route('permissions.update', $permission) }}" method="POST">
        @csrf
        @method('PUT')
        @include('permissions.form', ['permission' => $permission])
    </form>
@endsection
```

#### 📄 `form.blade.php`

```blade
<div class="mb-3">
    <label for="name">Permission Name</label>
    <input type="text" name="name" class="form-control" value="{{ old('name', $permission->name ?? '') }}" required>
    @error('name') <small class="text-danger">{{ $message }}</small> @enderror
</div>

<button type="submit" class="btn btn-success">Save</button>
<a href="{{ route('permissions.index') }}" class="btn btn-secondary">Back</a>
```

---

### ✅ 5. **AdminLTE Menu Entry**

In `config/adminlte.php`:

```php
[
    'text' => 'Permissions',
    'url'  => 'permissions',
    'icon' => 'fas fa-lock',
    'can'  => 'manage permissions', // Optional
],
```

---

Let me know if you'd like to integrate **permission assignment to roles**, or **user-level permission/role management UI**.
