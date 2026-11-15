<script setup>
import { ref, computed } from 'vue'

// State untuk form
const judul = ref('')
const kategori = ref('')
const sudahDibaca = ref(false)

// Daftar buku (array)
const daftarBuku = ref([])

// Fungsi untuk menambah buku
function tambahBuku() {
  // Validasi: judul dan kategori wajib diisi
  if (judul.value.trim() === '' || kategori.value === '') {
    alert('Judul dan kategori wajib diisi!')
    return
  }

  daftarBuku.value.push({
    id: Date.now(),
    judul: judul.value,
    kategori: kategori.value,
    sudahDibaca: sudahDibaca.value,
  })

  // Kosongkan form setelah disimpan
  judul.value = ''
  kategori.value = ''
  sudahDibaca.value = false
}

// Computed: jumlah buku yang sudah dibaca
const jumlahSudahDibaca = computed(() => {
  return daftarBuku.value.filter((buku) => buku.sudahDibaca).length
})
</script>

<template>
  <div class="container">
    <h2>Catatan Buku</h2>

    <!-- FORM INPUT -->
    <div class="field">
      <label>Judul Buku</label>
      <input v-model="judul" type="text" placeholder="Masukkan judul buku" />
    </div>

    <div class="field">
      <label>Kategori</label>
      <select v-model="kategori">
        <option value="">-- Pilih kategori --</option>
        <option value="Fiksi">Fiksi</option>
        <option value="Non-Fiksi">Non-Fiksi</option>
        <option value="Teknologi">Teknologi</option>
        <option value="Pendidikan">Pendidikan</option>
      </select>
    </div>

    <div class="field">
      <label>
        <input v-model="sudahDibaca" type="checkbox" />
        Sudah dibaca
      </label>
    </div>

    <button @click="tambahBuku">Tambah Buku</button>

    <hr />

    <!-- DAFTAR BUKU -->
    <h3>Daftar Buku</h3>

    <p v-if="daftarBuku.length === 0">Belum ada buku.</p>

    <ul>
      <li v-for="buku in daftarBuku" :key="buku.id">
        <strong>{{ buku.judul }}</strong>
        ({{ buku.kategori }}) -
        {{ buku.sudahDibaca ? 'Sudah dibaca ✅' : 'Belum dibaca ❌' }}
      </li>
    </ul>

    <!-- HASIL COMPUTED -->
    <p>
      Total buku: {{ daftarBuku.length }} |
      Sudah dibaca: <strong>{{ jumlahSudahDibaca }}</strong>
    </p>
  </div>
</template>

<style scoped>
.container {
  max-width: 400px;
  margin: 0 auto;
  text-align: left;
}
.field {
  margin-bottom: 12px;
}
label {
  display: block;
  margin-bottom: 4px;
}
input[type='text'],
select {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}
button {
  padding: 8px 16px;
  cursor: pointer;
}
ul {
  padding-left: 20px;
}
li {
  margin-bottom: 6px;
}
</style>
