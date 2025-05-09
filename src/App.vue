<script setup>
import { ref, computed, onMounted } from 'vue';

const daftarTugas = ref([
  { nama: "Periksa kondisi alat pemadam", selesai: false },
  { nama: "Simulasi evakuasi gedung kantor", selesai: true },
  { nama: "Cek tekanan air di hidran", selesai: false },
  { nama: "Pelatihan penggunaan APAR", selesai: true }
]);

const tugasBaru = ref("");
const inputTugas = ref(null);

const sedangEdit = ref(false);
const indeksEdit = ref(null);
const tampilkanSelesai = ref(true);

const tambahTugas = () => {
  const bersih = tugasBaru.value.trim();
  if (bersih) {
    daftarTugas.value.push({ nama: bersih, selesai: false });
    tugasBaru.value = "";
    inputTugas.value?.focus();
  }
};

const hapusTugas = (indeks) => {
  daftarTugas.value.splice(indeks, 1);
  batalEdit();
};

const editTugas = (indeks) => {
  tugasBaru.value = daftarTugas.value[indeks].nama;
  sedangEdit.value = true;
  indeksEdit.value = indeks;
  inputTugas.value?.focus();
};

const perbaruiTugas = () => {
  const bersih = tugasBaru.value.trim();
  if (indeksEdit.value !== null && bersih) {
    daftarTugas.value[indeksEdit.value].nama = bersih;
    batalEdit();
  }
};

const batalEdit = () => {
  tugasBaru.value = "";
  sedangEdit.value = false;
  indeksEdit.value = null;
};

const tugasTersaring = computed(() =>
  tampilkanSelesai.value
    ? daftarTugas.value
    : daftarTugas.value.filter(tugas => !tugas.selesai)
);

onMounted(() => {
  inputTugas.value?.focus();
});
</script>

<template>
   <div id="app">
    <div class="container">
      <div class="card">
        <h2>🔥 Agenda Harian Tim Pemadam Kebakaran</h2>

        <label class="toggle-show">
          <input type="checkbox" v-model="tampilkanSelesai" />
          Tampilkan yang sudah selesai
        </label>

        <ul class="task-list">
          <li v-for="(tugas, indeks) in tugasTersaring" :key="indeks">
            <div class="task-content">
              <input type="checkbox" v-model="tugas.selesai" />
              <span :class="{ done: tugas.selesai }">{{ indeks + 1 }}. {{ tugas.nama }}</span>
            </div>
            <div class="action-buttons">
              <button class="edit-btn" @click="editTugas(indeks)">✏</button>
              <button class="delete-btn" @click="hapusTugas(indeks)">🗑</button>
            </div>
          </li>
        </ul>

        <div class="input-group">
          <input
            ref="inputTugas"
            v-model="tugasBaru"
            type="text"
            placeholder="Tambahkan agenda pemadam kebakaran"
          />
          <button v-if="sedangEdit" @click="perbaruiTugas">Perbarui</button>
          <button v-else @click="tambahTugas">Tambah</button>
          <button v-if="sedangEdit" class="cancel-btn" @click="batalEdit">Batal</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  font-family: 'Segoe UI', sans-serif;
  background: #f9f9f9;
  color: #1a1a1a;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.container {
  flex: 1;
  display: flex;
  justify-content: center;
  padding: 40px 20px;
  box-sizing: border-box;
}

.card {
  width: 100%;
  max-width: 540px;
  background: #ffffff;
  border-left: 8px solid #dc3545;
  padding: 30px 25px;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(220, 53, 69, 0.2);
}

h2 {
  color: #dc3545;
  text-align: center;
  margin-bottom: 25px;
}

.toggle-show {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  font-size: 15px;
  color: #343a40;
  gap: 8px;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  max-height: 300px;
  overflow-y: auto;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 0;
  border-bottom: 1px solid #dee2e6;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.done {
  text-decoration: line-through;
  color: #6c757d;
}

.action-buttons {
  display: flex;
  gap: 8px;
}

.edit-btn,
.delete-btn {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

.edit-btn {
  color: #007bff;
}

.delete-btn {
  color: #dc3545;
}

.input-group {
  display: flex;
  gap: 10px;
  margin-top: 25px;
  flex-wrap: wrap;
}

input[type="text"] {
  flex: 1;
  padding: 12px;
  border: 1px solid #ced4da;
  border-radius: 8px;
  font-size: 15px;
  background-color: #fff3f3;
  color: #212529;
}

button {
  padding: 10px 16px;
  font-size: 14px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s;
}

button:hover {
  opacity: 0.9;
}

button:not(.cancel-btn):not(.delete-btn):not(.edit-btn) {
  background-color: #007bff;
  color: white;
}

.cancel-btn {
  background-color: #ffc107;
  color: #212529;
}
</style>
