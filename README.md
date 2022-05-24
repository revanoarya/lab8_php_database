## Nama     : Denas Aria Pamungkas
## NIM      : 312010089
## Kelas    : TI.20.A.1
## Matkul   : Pemograman Web

## PERTEMUAN 10 Lab 8 Web <b>

Dalam pertemuan 10 ini kita akan mempelajari PHP Data Base dengan beberapa program code PHP nya.

## PHP Data Base <b>

## 1. Menjalankan MySQL Server <b>
![1](https://user-images.githubusercontent.com/101621068/169969524-c2839425-6db1-47cf-aaad-e1f32698d17d.png)

## 2. Membuat Data Base <b>
### Membuat Tabel Latihan 1 <b>

### Contoh Codingan <b>
```html
CREATE DATABASE latihan1;
CREATE TABLE data_barang (
id_barang int(10) auto_increment Primary Key,
kategori varchar(30),
nama varchar(30),
gambar varchar(100),
harga_beli decimal(10,0),
harga_jual decimal(10,0),
stok int(4)
);
```
### Menambahkan Data <b>
![2](https://user-images.githubusercontent.com/101621068/169970817-dce566ea-8cb0-44fb-be5d-d09cc676dd6e.png)

![3](https://user-images.githubusercontent.com/101621068/169973753-1fe1ce06-6fa9-42c7-bdf6-572030a9d5ba.png)

## 3. Membuat Program CRUD
Buat folder lab8_php_database pada root directory web server (d:\xampp\htdocs)

![4](https://user-images.githubusercontent.com/101621068/169974439-9f133e64-f907-4035-945c-811f7d8df9ec.png)


## 4. Membuat File Koneksi Data Base <b>
Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL:
http://localhost/lab8_php_database/

![5](https://user-images.githubusercontent.com/101621068/169974749-34370ce9-2077-4c40-91b8-86e5732d0648.png)

## 5. Membuat file koneksi database <b>
![6](https://user-images.githubusercontent.com/101621068/169975095-f1f3373d-ef6c-4599-86d0-1c6e4c54d924.png)

### Contoh Codingan <b>
```html
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
echo "Koneksi ke server gagal.";
die();
} //else echo "Koneksi berhasil";
?>
```
## 6. Membuat file index untuk menampilkan data (Read) <b>
![7](https://user-images.githubusercontent.com/101621068/169975484-cb50bc6b-dab2-4448-800f-847db96fd1ae.png)

### Contoh Codingan <b>
```html
<?php
include("koneksi.php");
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Data Barang</title>
</head>
<body>
<div class="container">
<h1>Data Barang</h1>
<div class="main">
<table>
<tr>
<th>Gambar</th>
<th>Nama Barang</th>
<th>Katagori</th>
<th>Harga Jual</th>
<th>Harga Beli</th>
<th>Stok</th>
<th>Aksi</th>
</tr>
<?php if($result): ?>
<?php while($row = mysqli_fetch_array($result)): ?>
<tr>
<td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
$row['nama'];?>"></td>
<td><?= $row['nama'];?></td>
<td><?= $row['kategori'];?></td>
<td><?= $row['harga_beli'];?></td>
<td><?= $row['harga_jual'];?></td>
<td><?= $row['stok'];?></td>
<td><?= $row['id_barang'];?></td>
</tr>
<?php endwhile; else: ?>
<tr>
<td colspan="7">Belum ada data</td>
</tr>
<?php endif; ?>
</table>
</div>
</div>
</body>
</html>
```

## 7. Menambah Data (Create) <b>
![11](https://user-images.githubusercontent.com/101621068/169976521-af4f4101-961b-442b-b85d-cceee78b947b.png)
![8](https://user-images.githubusercontent.com/101621068/169977258-fc203cd2-3e40-494e-a3cc-18c330219d62.png)

### Contoh Codingan <b>
```html
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
$nama = $_POST['nama'];
$kategori = $_POST['kategori'];
$harga_jual = $_POST['harga_jual'];
$harga_beli = $_POST['harga_beli'];
$stok = $_POST['stok'];
$file_gambar = $_FILES['file_gambar'];
$gambar = null;
if ($file_gambar['error'] == 0)
{
$filename = str_replace(' ', '_',$file_gambar['name']);
$destination = dirname(__FILE__) .'/gambar/' . $filename;
if(move_uploaded_file($file_gambar['tmp_name'], $destination))
{
$gambar = 'gambar/' . $filename;;
}
}
$sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli,
stok, gambar) ';
$sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}',
'{$harga_beli}', '{$stok}', '{$gambar}')";
$result = mysqli_query($conn, $sql);
header('location: index.php');
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Tambah Barang</title>
</head>
<body>
<div class="container">
<h1>Tambah Barang</h1>
<div class="main">
<form method="post" action="tambah.php"
enctype="multipart/form-data">
<div class="input">
<label>Nama Barang</label>
<input type="text" name="nama" />
</div>
<div class="input">
<label>Kategori</label>
<select name="kategori">
<option value="Komputer">Komputer</option>
<option value="Elektronik">Elektronik</option>
<option value="Hand Phone">Hand Phone</option>
</select>
</div>
<div class="input">
<label>Harga Jual</label>
<input type="text" name="harga_jual" />
</div>
<div class="input">
<label>Harga Beli</label>
<input type="text" name="harga_beli" />
</div>
<div class="input">
<label>Stok</label>
<input type="text" name="stok" />
</div>
<div class="input">
<label>File Gambar</label>
<input type="file" name="file_gambar" />
</div>
<div class="submit">
<input type="submit" name="submit" value="Simpan" />
</div>
</form>
</div>
</div>
</body>
</html>
```

## 8. Mengubah Data (Update) <b>
![12](https://user-images.githubusercontent.com/101621068/169978236-65cbcbe8-87a8-42f4-9bf4-15bf478ce2ce.png)

### Contoh Codingan <b>
```html
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
$id = $_POST['id'];
$nama = $_POST['nama'];
$kategori = $_POST['kategori'];
$harga_jual = $_POST['harga_jual'];
$harga_beli = $_POST['harga_beli'];
$stok = $_POST['stok'];
$file_gambar = $_FILES['file_gambar'];
$gambar = null;
if ($file_gambar['error'] == 0)
{
$filename = str_replace(' ', '_', $file_gambar['name']);
$destination = dirname(__FILE__) . '/gambar/' . $filename;
if (move_uploaded_file($file_gambar['tmp_name'], $destination))
{
$gambar = 'gambar/' . $filename;;
}
}
$sql = 'UPDATE data_barang SET ';
$sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
$sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok
= '{$stok}' ";
if (!empty($gambar))
$sql .= ", gambar = '{$gambar}' ";
$sql .= "WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
}
$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);
function is_select($var, $val) {
if ($var == $val) return 'selected="selected"';
return false;
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Ubah Barang</title>
</head>
<body>
<div class="container">
<h1>Ubah Barang</h1>
<div class="main">
<form method="post" action="ubah.php"
enctype="multipart/form-data">
<div class="input">
<label>Nama Barang</label>
<input type="text" name="nama" value="<?php echo
$data['nama'];?>" />
</div>
<div class="input">
<label>Kategori</label>
<select name="kategori">
<option <?php echo is_select
('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
<option <?php echo is_select
('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
<option <?php echo is_select
('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
</select>
</div>
<div class="input">
<label>Harga Jual</label>
<input type="text" name="harga_jual" value="<?php echo
$data['harga_jual'];?>" />
</div>
<div class="input">
<label>Harga Beli</label>
<input type="text" name="harga_beli" value="<?php echo
$data['harga_beli'];?>" />
</div>
<div class="input">
<label>Stok</label>
<input type="text" name="stok" value="<?php echo
$data['stok'];?>" />
</div>
<div class="input">
<label>File Gambar</label>
<input type="file" name="file_gambar" />
</div>
<div class="submit">
<input type="hidden" name="id" value="<?php echo
$data['id_barang'];?>" />
<input type="submit" name="submit" value="Simpan" />
</div>
</form>
</div>
</div>
</body>
</html>
```

## 9. Menghapus Data (Delete) <b>
Jika ingin menghapus data yang sudah tertera di Url dengan codingan berikut :

### Contoh Codingan <b>
```html
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```

## 10. Menambahkan Style CSS pada Data <b>
![10](https://user-images.githubusercontent.com/101621068/169979599-0076f308-d405-4149-956e-a181ab7dad5a.png)

### Contoh Codingan CSS <b>
```html
body{
    background-color: whitesmoke;
    font-family: 'Courier New', Courier, monospace;
}
table{
    border-collapse: collapse;
    margin-top: 15px;
}
th{
    background-color: aqua;
}
th,td{
    border: 1px solid black;
}

.input{
    padding: 5px;
}
.input label{
    display: inline-block;
    width: 100px;
}
```

# TERIMAKASIH
