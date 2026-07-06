<?php
defined('BASEPATH') OR exit('No direct script access allowed');
$config['base_url'] = 'http://localhost/uts-pemweb3-manajemen-barang/';
$config['index_page'] = '';
$config['uri_protocol'] = 'REQUEST_URI';
$config['charset'] = 'UTF-8';
$config['enable_hooks'] = FALSE;
$config['permitted_uri_chars'] = 'a-z 0-9~%.:_\-';
$config['allow_get_array'] = TRUE;
$config['enable_csrf'] = FALSE;

<?php
defined('BASEPATH') OR exit('No direct script access allowed');
$active_group = 'default';
$query_builder = TRUE;
$db['default'] = array(
    'hostname' => 'localhost',
    'username' => 'root',
    'password' => '',
    'database' => 'db_manajemen_barang',
    'dbdriver' => 'mysqli',
    'char_set' => 'utf8'
);

<?php
defined('BASEPATH') OR exit('No direct script access allowed');
$autoload['libraries'] = array('database');
$autoload['helper'] = array('url');

<?php
defined('BASEPATH') OR exit('No direct script access allowed');
class Barang extends CI_Controller {
    public function __construct() {
        parent::__construct();
        $this->load->model('Model_barang');
    }
    public function index() {
        $data['total_barang'] = $this->Model_barang->hitungTotal();
        $this->load->view('template/header');
        $this->load->view('dashboard', $data);
    }
    public function tampil() {
        $data['barang'] = $this->Model_barang->ambilSemua();
        $this->load->view('template/header');
        $this->load->view('barang_tampil', $data);
    }
    public function tambah() {
        $this->load->view('template/header');
        $this->load->view('barang_tambah');
    }
    public function simpan() {
        $data = [
            'nama_barang' => $this->input->post('nama_barang'),
            'kategori' => $this->input->post('kategori'),
            'harga' => $this->input->post('harga'),
            'stok' => $this->input->post('stok'),
            'keterangan' => $this->input->post('keterangan')
        ];
        $this->Model_barang->simpanData($data);
        redirect('barang/tampil');
    }
}

<?php
defined('BASEPATH') OR exit('No direct script access allowed');
class Model_barang extends CI_Model {
    public function hitungTotal() {
        return $this->db->count_all('barang');
    }
    public function ambilSemua() {
        return $this->db->get('barang')->result();
    }
    public function simpanData($data) {
        return $this->db->insert('barang', $data);
    }
}

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>UTS Pemrograman Web III - Fatikhah Feby</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
</head>
<body>
<nav class="navbar navbar-inverse">
    <div class="container-fluid">
        <div class="navbar-header">
            <a class="navbar-brand" href="<?= base_url('barang') ?>">Manajemen Barang</a>
        </div>
        <ul class="nav navbar-nav">
            <li><a href="<?= base_url('barang') ?>">Dashboard</a></li>
            <li><a href="<?= base_url('barang/tampil') ?>">Data Barang</a></li>
            <li><a href="<?= base_url('barang/tambah') ?>">Tambah Barang</a></li>
        </ul>
    </div>
</nav>
<div class="container">

<div class="jumbotron text-center">
    <h2>DASHBOARD</h2>
    <hr>
    <h4>Jumlah Total Barang: <span class="label label-success"><?= $total_barang ?></span></h4>
    <br>
    <p><strong>Nama:</strong> Fatikhah Feby</p>
    <p><strong>Mata Kuliah:</strong> Pemrograman Web III</p>
    <p><strong>Tugas:</strong> UTS - Pertemuan 2</p>
</div>
</div></body></html>

<h3>Daftar Barang</h3>
<table class="table table-bordered table-striped">
    <thead>
        <tr><th>No</th><th>Nama</th><th>Kategori</th><th>Harga</th><th>Stok</th><th>Keterangan</th></tr>
    </thead>
    <tbody>
        <?php $no=1; foreach($barang as $b): ?>
        <tr>
            <td><?= $no++ ?></td>
            <td><?= $b->nama_barang ?></td>
            <td><?= $b->kategori ?></td>
            <td>Rp <?= number_format($b->harga,0,',','.') ?></td>
            <td><?= $b->stok ?></td>
            <td><?= $b->keterangan ?></td>
        </tr>
        <?php endforeach; ?>
    </tbody>
</table>
</div></body></html>

<h3>Form Tambah Barang</h3>
<form action="<?= base_url('barang/simpan') ?>" method="post">
    <div class="form-group">
        <label>Nama Barang</label>
        <input type="text" name="nama_barang" class="form-control" required>
    </div>
    <div class="form-group">
        <label>Kategori</label>
        <input type="text" name="kategori" class="form-control" required>
    </div>
    <div class="form-group">
        <label>Harga</label>
        <input type="number" name="harga" class="form-control" required>
    </div>
    <div class="form-group">
        <label>Stok</label>
        <input type="number" name="stok" class="form-control" required>
    </div>
    <div class="form-group">
        <label>Keterangan</label>
        <textarea name="keterangan" class="form-control"></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Simpan</button>
</form>
</div></body></html>

CREATE DATABASE IF NOT EXISTS db_manajemen_barang;
USE db_manajemen_barang;
CREATE TABLE barang (
    id_barang INT AUTO_INCREMENT PRIMARY KEY,
    nama_barang VARCHAR(100) NOT NULL,
    kategori VARCHAR(50) NOT NULL,
    harga DECIMAL(12,2) NOT NULL,
    stok INT NOT NULL,
    keterangan TEXT
);
INSERT INTO barang (nama_barang, kategori, harga, stok, keterangan) VALUES
('Laptop Asus', 'Elektronik', 8500000, 12, 'Laptop kantor'),
('Meja Belajar', 'Perabot', 750000, 8, 'Kayu jati'),
('Mouse Wireless', 'Aksesoris', 125000, 25, 'Tahan lama');

# UTS Pemrograman Web III
**Nama**: Fatikhah Feby
**NIM**: [Isi NIM Anda]
**Mata Kuliah**: Pemrograman Web III
**Tema**: Manajemen Barang
**Framework**: CodeIgniter 3
**Fitur**: Dashboard, Tampil Data, Tambah Data