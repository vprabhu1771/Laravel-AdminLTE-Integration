To use **DataTables** in **Laravel AdminLTE**, you must follow **3 essential steps**:

---

## ✅ 1. Install the DataTables Plugin

Run this Artisan command in your terminal:

```bash
php artisan adminlte:plugins install --plugin=datatables --plugin=datatablesPlugins
```

This will:

* Download and store DataTables assets inside `public/vendor/`
* Include tools like **Export**, **Print**, **PDF**, etc.

---

## ✅ 2. Enable DataTables in `config/adminlte.php`

Find the `plugins` section in `config/adminlte.php` and **enable DataTables** like this:

```php
'plugins' => [
    'Datatables' => [
        'active' => true,
        'files' => [
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables/js/jquery.dataTables.min.js',
            ],
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables/js/dataTables.bootstrap4.min.js',
            ],
            [
                'type' => 'css',
                'asset' => true,
                'location' => 'vendor/datatables/css/dataTables.bootstrap4.min.css',
            ],
        ],
    ],

    'DatatablesPlugins' => [
        'active' => true,
        'files' => [
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/buttons/js/dataTables.buttons.min.js',
            ],
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/buttons/js/buttons.bootstrap4.min.js',
            ],
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/jszip/jszip.min.js',
            ],
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/pdfmake/pdfmake.min.js',
            ],
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/pdfmake/vfs_fonts.js',
            ],
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/buttons/js/buttons.html5.min.js',
            ],
            [
                'type' => 'js',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/buttons/js/buttons.print.min.js',
            ],
            [
                'type' => 'css',
                'asset' => true,
                'location' => 'vendor/datatables-plugins/buttons/css/buttons.bootstrap4.min.css',
            ],
        ],
    ],
],
```

---

## ✅ 3. Use DataTables in Your Blade View

Update your table setup in `resources/views/shops/index.blade.php` like this:

```blade
<table id="shops-table" class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Address</th>
            <th>Actions</th>
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
                            onclick="return confirm('Are you sure?')">Delete</button>
                    </form>
                </td>
            </tr>
        @endforeach
    </tbody>
</table>
```

Add this to your JS section:

```blade
@section('js')
<script>
    $(function () {
        $('#shops-table').DataTable({
            responsive: true,
            dom: 'Bfrtip',
            buttons: [
                'copy', 'csv', 'excel', 'pdf', 'print'
            ]
        });
    });
</script>
@stop
```

---

## 🔁 Summary

| Step | Action                                                                                     |
| ---- | ------------------------------------------------------------------------------------------ |
| ✅ 1  | Run: `php artisan adminlte:plugins install --plugin=datatables --plugin=datatablesPlugins` |
| ✅ 2  | Enable DataTables in `config/adminlte.php`                                                 |
| ✅ 3  | Initialize DataTables in Blade view with `$('#your-table').DataTable()`                    |

---

Let me know if you'd like to use **AJAX-powered DataTables** or **server-side pagination** for large datasets. 
