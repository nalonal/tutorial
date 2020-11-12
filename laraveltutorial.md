# Laravel Tutorial
Tutorial ini ditulis dengan maksud mendokumentasikan hasil belajar Laravel

# Daftar Isi
 - [Cara Installasi](https://github.com/nalonal/tutorial/blob/main/laraveltutorial.md#installasi)
 - [Cara Menggunakan Blade Template](https://github.com/nalonal/tutorial/blob/main/laraveltutorial.md#blading-templating)
 - [Cara Menggunakan CRUD](https://github.com/nalonal/tutorial/blob/main/laraveltutorial.md#metode-crud)
 - [Cara Menggunakan Seeder](https://github.com/nalonal/tutorial/blob/main/laraveltutorial.md#seeder-database)


# Installasi
Laravel Install
```sh
composer create-project laravel/laravel=8.0 projectapp --prefer-dist
```
atau misalnya habis download dari git jangan lupa instalasi
```sh
composer install
```
setelah itu bikin file .env (copy dari example)
install key laravel
```sh
php artisan key:generate
```
# Hide .env File form Hacker
create .htaccess file then fill with this
```sh
# Disable index view
Options -Indexes
# Hide a specific file
<Files .env>
    Order allow,deny
    Deny from all
</Files>
```

# Blade (Templating)
Master Blade
```sh
<!DOCTYPE html>
<html>
<head>
    @include('template.head')
    @include('template.script')
  <style type="text/css">
    .skin-purple-light .main-header .navbar {
       color: grey;
       background-color: white;
       min-height: 70px;
    }
  </style>
  <title></title>
</head>
<body class="sidebar-mini fixed wysihtml5-supported skin-purple-light navbar-light">
<div class="wrapper">
  @include('template.header')
  <!-- Left side column. contains the logo and sidebar -->
  @include('template.sidebar')
  <!-- Content Wrapper. Contains page content -->
  <div class="content-wrapper" style="margin-top: 30px">
    <!-- Main content -->
    @yield('konten')
    <!-- /.content -->
  </div>
  <!-- /.content-wrapper -->
  @include('template.footer')
  @include('template.modals')
</div>
<!-- ./wrapper -->
</body>
</html>
```

Component Template Blade (Example)
```sh
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <!-- Tell the browser to be responsive to screen width -->
  <meta content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" name="viewport">
  <!-- Bootstrap 3.3.7 -->
  <link rel="stylesheet" href="{{ url('') }}/template/bower_components/bootstrap/dist/css/bootstrap.min.css">
  <!-- Font Awesome -->
  <link rel="stylesheet" href="{{ url('') }}/template/bower_components/font-awesome/css/font-awesome.min.css">
  <!-- Ionicons -->
  <link rel="stylesheet" href="{{ url('') }}/template/bower_components/Ionicons/css/ionicons.min.css">
  <!-- Theme style -->
  <link rel="stylesheet" href="{{ url('') }}/template/dist/css/AdminLTE.min.css">
  <!-- AdminLTE Skins. Choose a skin from the css/skins
       folder instead of downloading all of them to reduce the load. -->
  <link rel="stylesheet" href="{{ url('') }}/template/dist/css/skins/_all-skins.min.css">
  <!-- Morris chart -->
  <link rel="stylesheet" href="{{ url('') }}/template/bower_components/morris.js/morris.css">
  <!-- jvectormap -->
  <link rel="stylesheet" href="bower_components/jvectormap/jquery-jvectormap.css">
  <!-- Date Picker -->
  <link rel="stylesheet" href="bower_components/bootstrap-datepicker/dist/css/bootstrap-datepicker.min.css">
  <!-- Daterange picker -->
  <link rel="stylesheet" href="{{ url('') }}/template/bower_components/bootstrap-daterangepicker/daterangepicker.css">
  <!-- bootstrap wysihtml5 - text editor -->
  <link rel="stylesheet" href="{{ url('') }}/template/plugins/bootstrap-wysihtml5/bootstrap3-wysihtml5.min.css">
  <link rel="icon" href="{{ url('') }}/template/gambar/logo.png">

  <!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
  <!-- WARNING: Respond.js doesnt work if you view the page via file:// -->
  <!--[if lt IE 9] -->
  <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
  <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
  <![endif]-->

  <!-- Google Font -->
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Source+Sans+Pro:300,400,600,700,300italic,400italic,600italic">
  <style type="text/css">
  body{
    font-family: Poppins,Helvetica,sans-serif;
  }
    .example-modal .modal {
      position: relative;
      top: auto;
      bottom: auto;
      right: auto;
      left: auto;
      display: block;
      z-index: 1;
    }

    .example-modal .modal {
      background: transparent !important;
    }
  </style>
```

Konten Blade (Example)
```sh
@extends('master')

@section('konten')
<section class="content">
    <h1> Hai </h1>
</section>
@endsection
```

# Metode CRUD

### Migrasi
Membuat migrasi
```sh
php artisan make:migration create_<nama_modelnya_nanti> --create=<nama_tabelnya_nanti>
php artisan make:migration create_m_activity_sifat --create=m_activity_sifat
```
Melakukan migrasi
```sh
php artisan migrate
```
(optional melakukan fresh migrasi)
```sh
php artisan migrate:fresh
```
Contoh isian migrasi
```sh
public function up(){
    Schema::create('m_activitysifat', function (Blueprint $table) {
        $table->id();
        $table->string('nama', 150);
        $table->string('deskripsi', 255)->nullable();
        $table->timestamps();
        $table->softDeletes();
    });
}

```

### Model
Membuat model
```sh
php artisan make:model <nama_model>
php artisan make:model MasterActivitySifat
```

Mengisi fillable
```sh
class M_Activity_SifatModel extends Model{
    use HasFactory;
    public $table = 'm_activitysifat';
    const CREATED_AT = 'created_at';
    const UPDATED_AT = 'updated_at';
    protected $dates = ['deleted_at'];
    public $incrementing = false;
    public $fillable = [
        'nama','deskripsi'
    ];
    /**
     * The attributes that should be casted to native types.
     *
     * @var array
     */
    protected $casts = [
        'id' => 'string',
        'nama' => 'string',
        'deskripsi' => 'string'
    ];
}
```

### Controller
membuat controller
```sh
php artisan make:controller <nama_controller> --resource --model=<nama_model>
php artisan make:controller m_activitysifat --resource --model=MasterActivitySifat
```

### Setting Route
```sh
use App\Http\Controllers\M_Activity_sifatController;
.....
Route::resource('/m_activitysifatpage', M_Activity_sifatController::class);
```

Isi controller & metode view
#### Index
Index Controller
```sh
public function index()
{
    $M_Activity_SifatModel = M_Activity_SifatModel::latest()->get();
    // print_r($M_Activity_SifatModel);
    return view('master.activitysifat.index', compact('M_Activity_SifatModel'));
    //
}
```
Index View
```sh
@foreach ($M_Activity_SifatModel as $activitysifat)
    <tr>
    <td>{{ $activitysifat->id }}</td>
    <td>{{ $activitysifat->nama }}</td>
    <td>{{ $activitysifat->deskripsi }}</td>
    <td>
        <form action="{{ route('m_activitysifatpage.destroy', $activitysifat->id) }}" method="POST">
            <a href="{{ route('m_activitysifatpage.edit', $activitysifat->id) }}" class="btn btn-sm btn-primary">EDIT</a>
            @csrf
            @method('DELETE')
            <button type="submit" class="btn btn-sm btn-danger">HAPUS</button>
        </form>
    </td>
    </tr>
@endforeach
```

#### Create & Store
Create & Store Controller
```sh
public function create()
{
    return view('master.activitysifat.create');
}
...
public function store(Request $request)
{
    M_Activity_SifatModel::create($request->all());
    return redirect()->route('m_activitysifatpage.index');
}
```

Create & Store View
```sh
...
<form method="POST" action="{{ route('m_activitysifatpage.update', $Data_Activity_Sifat->id) }}">
    @csrf
    @method('PUT')
    {{ $Data_Activity_Sifat->keterangan }}
</form>
...
```

#### Update
Update Controller
```sh
...
public function edit($id)
{
    $Data_Activity_Sifat = M_Activity_SifatModel::findOrFail($id);
    return view('master.activitysifat.edit', compact('Data_Activity_Sifat'));
}
...
public function update(Request $request, $id)
{
    $m_Activity_SifatModel = M_Activity_SifatModel::findOrFail($id);
    $m_Activity_SifatModel->update($request->all());
    return redirect()->route('m_activitysifatpage.index');
}
...
```

Update View
```sh
<form method="POST" action="{{ route('m_activitysifatpage.update', $Data_Activity_Sifat->id) }}">
    @csrf
    @method('PUT')
</form>
```

#### Delete
Delete Controller
```sh
public function destroy($id){
    $thisaction = M_Activity_SifatModel::findOrFail($id);
    $thisaction->delete();
    return redirect()->route('m_activitysifatpage.index')->with('success','Berhasil menghapus sifat surat');
}
```
Delete View (Lihat Index View)
```sh
<form action="{{ route('m_activitysifatpage.destroy', $activitysifat->id) }}" method="POST">
    <a href="{{ route('m_activitysifatpage.edit', $activitysifat->id) }}" class="btn btn-sm btn-primary">EDIT</a>
    ...
    @csrf
    @method('DELETE')
    <button type="submit" class="btn btn-sm btn-danger">HAPUS</button>
</form>
```

# Seeder Database
Membuat seeder
```sh
php artisan make:seeder <nama_seeder>
php artisan make:seeder MasterActivitySifatSeeder
```

Melakukan setting pada seeder
```sh
use App\Models\User;
.....
public function run()
{
    User::create([
        'name' => 'Hardik',
        'email' => 'admin@gmail.com',
        'password' => bcrypt('123456'),
    ]);
}
```

### Menjalankan seeder (Single Seeder)
```sh
php artisan db:seed --class=<nama_seeder>
php artisan db:seed --class=MasterActivitySifatSeeder
```
### Menjalankan seeder All
Isi File DatabaseSeeder.php
```sh
public function run()
{
    $this->call(MasterActivitySifatSeeder::class);
}
```
Jalankan Artisan
```sh
php artisan db:seed
```
