Here's a **complete example** of implementing CRUD for `Shop` using:

* `Route::resource('shops', ShopController::class)`
* Laravel **AdminLTE** layout
* **jQuery DataTables** (already included in AdminLTE)

---

## ✅ Step 1: Migration (if needed)

```bash
php artisan make:model Shop -m
```

**database/migrations/xxxx\_xx\_xx\_create\_shops\_table.php**:

```php
public function up()
{
    Schema::create('shops', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('address')->nullable();
        $table->timestamps();
    });
}
```

```bash
php artisan migrate
```

---

## ✅ Step 2: Route

**routes/web.php**

```php
use App\Http\Controllers\ShopController;

Route::resource('shops', ShopController::class);
```

---

## ✅ Step 3: Controller

```bash
php artisan make:controller ShopController --resource
```

**app/Http/Controllers/ShopController.php**

```php
namespace App\Http\Controllers;

use App\Models\Shop;
use Illuminate\Http\Request;

class ShopController extends Controller
{
    public function index()
    {
        $shops = Shop::all();
        return view('shops.index', compact('shops'));
    }

    public function create()
    {
        return view('shops.create');
    }

    public function store(Request $request)
    {
        $request->validate([
            'name' => 'required',
            'address' => 'nullable',
        ]);

        Shop::create($request->only('name', 'address'));
        return redirect()->route('shops.index')->with('success', 'Shop created!');
    }

    public function show(Shop $shop)
    {
        return view('shops.show', compact('shop'));
    }

    public function edit(Shop $shop)
    {
        return view('shops.edit', compact('shop'));
    }

    public function update(Request $request, Shop $shop)
    {
        $request->validate([
            'name' => 'required',
            'address' => 'nullable',
        ]);

        $shop->update($request->only('name', 'address'));
        return redirect()->route('shops.index')->with('success', 'Shop updated!');
    }

    public function destroy(Shop $shop)
    {
        $shop->delete();
        return redirect()->route('shops.index')->with('success', 'Shop deleted!');
    }
}
```

---

## ✅ Step 4: Views

### 1. `resources/views/shops/index.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Shops')

@section('content_header')
    <h1>Shops</h1>
@stop

@section('content')
    <a href="{{ route('shops.create') }}" class="btn btn-primary mb-3">Add Shop</a>

    @if (session('success'))
        <div class="alert alert-success">{{ session('success') }}</div>
    @endif

    <table id="shops-table" class="table table-bordered table-striped">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Address</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($shops as $shop)
                <tr>
                    <td>{{ $shop->id }}</td>
                    <td>{{ $shop->name }}</td>
                    <td>{{ $shop->address }}</td>
                    <td>
                        <a href="{{ route('shops.edit', $shop) }}" class="btn btn-sm btn-warning">Edit</a>
                        <form action="{{ route('shops.destroy', $shop) }}" method="POST" style="display:inline;">
                            @csrf @method('DELETE')
                            <button type="submit" class="btn btn-sm btn-danger"
                                    onclick="return confirm('Delete this shop?')">Delete</button>
                        </form>
                    </td>
                </tr>
            @endforeach
        </tbody>
    </table>
@stop

@section('js')
    <script>
        $(function () {
            $('#shops-table').DataTable();
        });
    </script>
@stop
```

---

### 2. `resources/views/shops/create.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Add Shop')

@section('content_header')
    <h1>Add Shop</h1>
@stop

@section('content')
    <form action="{{ route('shops.store') }}" method="POST">
        @csrf
        <div class="form-group">
            <label for="name">Shop Name</label>
            <input type="text" name="name" class="form-control" required value="{{ old('name') }}">
        </div>
        <div class="form-group">
            <label for="address">Address (optional)</label>
            <input type="text" name="address" class="form-control" value="{{ old('address') }}">
        </div>
        <button type="submit" class="btn btn-success">Save</button>
    </form>
@stop
```

---

### 3. `resources/views/shops/edit.blade.php`

```blade
@extends('adminlte::page')

@section('title', 'Edit Shop')

@section('content_header')
    <h1>Edit Shop</h1>
@stop

@section('content')
    <form action="{{ route('shops.update', $shop) }}" method="POST">
        @csrf @method('PUT')
        <div class="form-group">
            <label for="name">Shop Name</label>
            <input type="text" name="name" class="form-control" required value="{{ old('name', $shop->name) }}">
        </div>
        <div class="form-group">
            <label for="address">Address (optional)</label>
            <input type="text" name="address" class="form-control" value="{{ old('address', $shop->address) }}">
        </div>
        <button type="submit" class="btn btn-success">Update</button>
    </form>
@stop
```

---

## ✅ Step 5: Model

**app/Models/Shop.php**

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Shop extends Model
{
    use HasFactory;

    protected $fillable = ['name', 'address'];
}
```

---

## ✅ Optional: Sidebar Menu

In `config/adminlte.php` under `'menu'`:

```php
[
    'text' => 'Shops',
    'url'  => 'shops',
    'icon' => 'fas fa-store',
],
```

---

Let me know if you want to make the table AJAX powered or apply the same for `Product`, `Order`, etc.
