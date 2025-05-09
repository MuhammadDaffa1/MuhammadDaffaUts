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
 
 
</template>

<style scoped>

</style>
