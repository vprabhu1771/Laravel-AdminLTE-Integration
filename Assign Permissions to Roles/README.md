Here's how to build **Role-Permission Assignment UI** and **User-Role Assignment UI** using **Spatie Laravel-Permission** with Laravel and AdminLTE.

---

## ✅ 1. Assign Permissions to Roles

### 🔹 Step 1: Update `RoleController`

Add methods to show and save permissions:

```php
use Spatie\Permission\Models\Permission;

public function showPermissions(Role $role)
{
    $permissions = Permission::all();
    $rolePermissions = $role->permissions->pluck('id')->toArray();

    return view('roles.permissions', compact('role', 'permissions', 'rolePermissions'));
}

public function updatePermissions(Request $request, Role $role)
{
    $permissionIds = $request->permissions ?? [];

    // Fetch the permission models by ID
    $permissions = Permission::whereIn('id', $permissionIds)->get();

    // Assign permissions correctly
    $role->syncPermissions($permissions);

    return redirect()->route('roles.index')->with('success', 'Permissions updated for role.');
}
```

### 🔹 Step 2: Add Routes

In `web.php`:

```php
Route::get('roles/{role}/permissions', [RoleController::class, 'showPermissions'])->name('roles.permissions');
Route::post('roles/{role}/permissions', [RoleController::class, 'updatePermissions'])->name('roles.permissions.update');
```

### 🔹 Step 3: Create Blade View — `resources/views/roles/permissions.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Assign Permissions')

@section('content')
<h1>Permissions for Role: {{ $role->name }}</h1>

<form action="{{ route('roles.permissions.update', $role) }}" method="POST">
    @csrf

    <div class="mb-3">
        @foreach ($permissions as $permission)
            <div class="form-check">
                <input class="form-check-input" type="checkbox" name="permissions[]" value="{{ $permission->id }}"
                    {{ in_array($permission->id, $rolePermissions) ? 'checked' : '' }}>
                <label class="form-check-label">{{ $permission->name }}</label>
            </div>
        @endforeach
    </div>

    <button class="btn btn-primary">Save</button>
    <a href="{{ route('roles.index') }}" class="btn btn-secondary">Back</a>
</form>
@endsection
```

---

## ✅ 2. Assign Roles to Users

### 🔹 Step 1: Create `UserRoleController`

```bash
php artisan make:controller UserRoleController
```

```php
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Spatie\Permission\Models\Role;
use Illuminate\Http\Request;

class UserRoleController extends Controller
{
    public function index()
    {
        $users = User::with('roles')->get();
        return view('users.index', compact('users'));
    }

    public function edit(User $user)
    {
        $roles = Role::all();
        $userRoles = $user->roles->pluck('id')->toArray();
        return view('users.edit', compact('user', 'roles', 'userRoles'));
    }

    public function update(Request $request, User $user)
    {
        $user->syncRoles($request->roles ?? []);
        return redirect()->route('users.index')->with('success', 'Roles updated for user.');
    }
}
```

### 🔹 Step 2: Add Routes

```php
use App\Http\Controllers\UserRoleController;

Route::get('users', [UserRoleController::class, 'index'])->name('users.index');
Route::get('users/{user}/edit', [UserRoleController::class, 'edit'])->name('users.edit');
Route::put('users/{user}', [UserRoleController::class, 'update'])->name('users.update');
```

---

### 🔹 Step 3: Views

#### 📄 `resources/views/users/index.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'User Roles')

@section('content')
    <h1>Users & Roles</h1>

    @if (session('success'))
        <div class="alert alert-success">{{ session('success') }}</div>
    @endif

    <table class="table table-bordered">
        <thead>
            <tr>
                <th>Name</th>
                <th>Roles</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($users as $user)
                <tr>
                    <td>{{ $user->name }}</td>
                    <td>{{ implode(', ', $user->getRoleNames()->toArray()) }}</td>
                    <td><a href="{{ route('users.edit', $user) }}" class="btn btn-sm btn-warning">Edit Roles</a></td>
                </tr>
            @endforeach
        </tbody>
    </table>
@endsection
```

#### 📄 `resources/views/users/edit.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Edit User Roles')

@section('content')
<h1>Assign Roles to: {{ $user->name }}</h1>

<form action="{{ route('users.update', $user) }}" method="POST">
    @csrf
    @method('PUT')

    @foreach ($roles as $role)
        <div class="form-check">
            <input class="form-check-input" type="checkbox" name="roles[]" value="{{ $role->id }}"
                {{ in_array($role->id, $userRoles) ? 'checked' : '' }}>
            <label class="form-check-label">{{ $role->name }}</label>
        </div>
    @endforeach

    <button class="btn btn-success mt-3">Save</button>
    <a href="{{ route('users.index') }}" class="btn btn-secondary mt-3">Back</a>
</form>
@endsection
```

---

## ✅ 3. Add AdminLTE Menu Items

In `config/adminlte.php`:

```php
[
    'text' => 'User Roles',
    'url'  => 'users',
    'icon' => 'fas fa-user',
],
[
    'text' => 'Role Permissions',
    'url'  => 'roles',
    'icon' => 'fas fa-user-shield',
],
[
    'text' => 'Permissions',
    'url'  => 'permissions',
    'icon' => 'fas fa-lock',
],
```

---

Let me know if you want:

* A combined UI for assigning **permissions directly to users**
* Ability to **assign multiple roles/permissions on user registration**
* A full **Filament UI** alternative if you're using FilamentPHP
