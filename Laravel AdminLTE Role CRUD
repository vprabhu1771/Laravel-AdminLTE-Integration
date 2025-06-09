To implement CRUD (Create, Read, Update, Delete) functionality for **Roles** in a Laravel project using **Spatie Permission** and **Laravel-AdminLTE**, here's a complete example to get you started:

---

### ✅ 1. **Run the Seeder**

Your `RoleSeeder` is already correct. Run it:

```bash
php artisan db:seed --class=RoleSeeder
```

---

### ✅ 2. **Create RoleController**

```bash
php artisan make:controller RoleController --resource
```

Update the controller:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Spatie\Permission\Models\Role;

class RoleController extends Controller
{
    public function index()
    {
        $roles = Role::all();
        return view('roles.index', compact('roles'));
    }

    public function create()
    {
        return view('roles.create');
    }

    public function store(Request $request)
    {
        $request->validate(['name' => 'required|unique:roles']);
        Role::create(['name' => $request->name]);

        return redirect()->route('roles.index')->with('success', 'Role created successfully.');
    }

    public function edit(Role $role)
    {
        return view('roles.edit', compact('role'));
    }

    public function update(Request $request, Role $role)
    {
        $request->validate(['name' => 'required|unique:roles,name,' . $role->id]);
        $role->update(['name' => $request->name]);

        return redirect()->route('roles.index')->with('success', 'Role updated successfully.');
    }

    public function destroy(Role $role)
    {
        $role->delete();
        return redirect()->route('roles.index')->with('success', 'Role deleted successfully.');
    }
}
```

---

### ✅ 3. **Define Routes**

In `routes/web.php`:

```php
use App\Http\Controllers\RoleController;

Route::resource('roles', RoleController::class);
```

---

### ✅ 4. **Create Blade Views**

Create views in `resources/views/roles/`.

#### 📄 `index.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Roles')

@section('content')
    <h1>Roles</h1>

    <a href="{{ route('roles.create') }}" class="btn btn-primary mb-2">Add Role</a>

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
            @foreach ($roles as $role)
                <tr>
                    <td>{{ $role->name }}</td>
                    <td>
                        <a href="{{ route('roles.edit', $role) }}" class="btn btn-sm btn-warning">Edit</a>
                        <form action="{{ route('roles.destroy', $role) }}" method="POST" style="display:inline;">
                            @csrf @method('DELETE')
                            <button class="btn btn-sm btn-danger" onclick="return confirm('Delete this role?')">Delete</button>
                        </form>
                    </td>
                </tr>
            @endforeach
        </tbody>
    </table>
@endsection
```

#### 📄 `create.blade.php` and `edit.blade.php`

You can reuse the same form in both views.

```blade
{{-- create.blade.php --}}
@extends('adminlte::page')

@section('title', 'Create Role')

@section('content')
    <h1>Create Role</h1>

    <form action="{{ route('roles.store') }}" method="POST">
        @csrf
        @include('roles.form')
    </form>
@endsection
```

```blade
{{-- edit.blade.php --}}
@extends('adminlte::page')

@section('title', 'Edit Role')

@section('content')
    <h1>Edit Role</h1>

    <form action="{{ route('roles.update', $role) }}" method="POST">
        @csrf
        @method('PUT')
        @include('roles.form', ['role' => $role])
    </form>
@endsection
```

#### 📄 `form.blade.php`

```blade
<div class="mb-3">
    <label for="name">Role Name</label>
    <input type="text" name="name" class="form-control" value="{{ old('name', $role->name ?? '') }}" required>
    @error('name') <small class="text-danger">{{ $message }}</small> @enderror
</div>

<button type="submit" class="btn btn-success">Save</button>
<a href="{{ route('roles.index') }}" class="btn btn-secondary">Back</a>
```

---

### ✅ 5. **AdminLTE Menu Setup**

Edit `config/adminlte.php`:

```php
[
    'text' => 'Roles',
    'url'  => 'roles',
    'icon' => 'fas fa-user-shield',
    'can'  => 'manage roles', // Optional if you're using permissions
],
```

---

### ✅ 6. **Add Gate or Middleware (Optional)**

You can protect this controller using middleware or policies if needed.

---

Let me know if you also want **permissions management** integrated with each role.
