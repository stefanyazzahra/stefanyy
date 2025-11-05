<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CRUD Sederhana Data Siswa</title>
    <style>
        /* CSS Sederhana untuk tampilan */
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
        button { padding: 5px 10px; cursor: pointer; }
        .form-container { margin-bottom: 20px; padding: 15px; border: 1px solid #ccc; }
        .hidden { display: none; }
    </style>
</head>
<body>

    <h1>📝 Data Siswa (CRUD)</h1>

    <div class="form-container">
        <h2>Tambah/Edit Siswa</h2>
        <form id="siswaForm">
            <input type="hidden" id="siswaId" value="">

            <label for="nama">Nama:</label><br>
            <input type="text" id="nama" required><br><br>

            <label for="kelas">Kelas:</label><br>
            <input type="text" id="kelas" required><br><br>

            <label for="nilai">Nilai:</label><br>
            <input type="number" id="nilai" required><br><br>

            <button type="submit" id="submitBtn">Tambah Siswa</button>
            <button type="button" id="cancelBtn" class="hidden">Batal Edit</button>
        </form>
    </div>

    <hr>
    <h2>Daftar Siswa</h2>
    <table>
        <thead>
            <tr>
                <th>ID</th>
                <th>Nama</th>
                <th>Kelas</th>
                <th>Nilai</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody id="siswaTableBody">
            </tbody>
    </table>

    <script src="index.html"></script> 
    <script>
        // Data siswa sementara (sebagai array objek)
let dataSiswa = [];
let nextId = 1;

// Referensi DOM elemen
const siswaForm = document.getElementById('siswaForm');
const siswaTableBody = document.getElementById('siswaTableBody');
const siswaIdInput = document.getElementById('siswaId');
const namaInput = document.getElementById('nama');
const kelasInput = document.getElementById('kelas');
const nilaiInput = document.getElementById('nilai');
const submitBtn = document.getElementById('submitBtn');
const cancelBtn = document.getElementById('cancelBtn');

// ===================================
// FUNGSI UTAMA CRUD
// ===================================

// READ: Menampilkan data siswa ke tabel
function renderSiswa() {
    siswaTableBody.innerHTML = ''; // Kosongkan tabel
    
    // Looping untuk setiap siswa
    dataSiswa.forEach(siswa => {
        const row = siswaTableBody.insertRow();
        row.insertCell().textContent = siswa.id;
        row.insertCell().textContent = siswa.nama;
        row.insertCell().textContent = siswa.kelas;
        row.insertCell().textContent = siswa.nilai;

        // Kolom Aksi (Edit dan Delete)
        const actionCell = row.insertCell();

        // Tombol EDIT
        const editBtn = document.createElement('button');
        editBtn.textContent = 'Edit';
        editBtn.onclick = () => editSiswa(siswa.id);
        actionCell.appendChild(editBtn);

        // Tombol DELETE
        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'Hapus';
        deleteBtn.style.marginLeft = '5px';
        deleteBtn.onclick = () => deleteSiswa(siswa.id);
        actionCell.appendChild(deleteBtn);
    });
}

// CREATE / UPDATE: Menangani submit form
siswaForm.addEventListener('submit', function(event) {
    event.preventDefault();

    const id = siswaIdInput.value;
    const nama = namaInput.value;
    const kelas = kelasInput.value;
    const nilai = parseInt(nilaiInput.value);

    // Cek apakah ID ada (mode UPDATE) atau kosong (mode CREATE)
    if (id) {
        // UPDATE
        const index = dataSiswa.findIndex(siswa => siswa.id == id);
        if (index > -1) {
            dataSiswa[index] = { id: parseInt(id), nama, kelas, nilai };
        }
        resetForm();
    } else {
        // CREATE
        const newSiswa = {
            id: nextId++,
            nama: nama,
            kelas: kelas,
            nilai: nilai
        };
        dataSiswa.push(newSiswa);
        siswaForm.reset(); // Bersihkan form
    }
    
    renderSiswa(); // Perbarui tampilan
});

// UPDATE: Memuat data siswa ke form untuk diedit
function editSiswa(id) {
    const siswa = dataSiswa.find(s => s.id === id);
    if (siswa) {
        siswaIdInput.value = siswa.id;
        namaInput.value = siswa.nama;
        kelasInput.value = siswa.kelas;
        nilaiInput.value = siswa.nilai;
        
        submitBtn.textContent = 'Simpan Perubahan'; // Ganti teks tombol
        cancelBtn.classList.remove('hidden'); // Tampilkan tombol Batal
    }
}

// DELETE: Menghapus data siswa berdasarkan ID
function deleteSiswa(id) {
    if (confirm(Yakin ingin menghapus siswa dengan ID ${id}?)) {
        dataSiswa = dataSiswa.filter(siswa => siswa.id !== id);
        renderSiswa(); // Perbarui tampilan
    }
}

// Fungsi untuk mereset form setelah UPDATE
function resetForm() {
    siswaForm.reset();
    siswaIdInput.value = '';
    submitBtn.textContent = 'Tambah Siswa';
    cancelBtn.classList.add('hidden');
}

// Event listener untuk tombol Batal
cancelBtn.addEventListener('click', resetForm);

// Panggil fungsi renderSiswa() saat halaman pertama kali dimuat
document.addEventListener('DOMContentLoaded', renderSiswa);
    </script>

</body>
</html>
