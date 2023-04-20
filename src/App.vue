<template>
  <div class="container">
    <h1 style="text-align: center;">Lacak Paketmu</h1>
    <form @submit.prevent="addKegiatan">
      <input type="text" v-model="newKegiatan" placeholder="Tambahkan No.Resi">
      <button>Tambahkan</button>
    </form>

    <br>

    <div>
      <button :class="{aktif: filter === 'all'}" @click="filter = 'all'">Semua</button>
      <button :class="{aktif: filter === 'aktif'}" @click="filter = 'aktif'">Belum Diterima</button>
      <button :class="{aktif: filter === 'selesai'}" @click="filter = 'selesai'">Sudah Diterima</button>
    </div>

    <br>

    <ul>
      <li v-for="kegiatan in filterAktifitas" :key="kegiatan.id">
        <div>
          <button @click="completeKegiatan(kegiatan)">Selesai</button>
          <button @click="deleteKegiatan(kegiatan)">Hapus</button>
        </div>
        <div>
          <span :class="{selesai: kegiatan.selesai}">{{ kegiatan.name }}</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      aktifitas: [],
      newKegiatan: '',
      filter: 'all'
    }
  },

  methods: {
    addKegiatan() {
      if (this.newKegiatan.trim() !== '') {
        this.aktifitas.push({
          id: Date.now(),
          name: this.newKegiatan.trim(),
          selesai: false
        });
        this.newKegiatan = '';
      }
    },

    deleteKegiatan(kegiatan) {
      const index = this.aktifitas.findIndex(a => a.id === kegiatan.id);
      if (index !== -1) {
        this.aktifitas.splice(index, 1);
      }
    },

    completeKegiatan(kegiatan) {
      kegiatan.selesai = true;
    }
  },

  computed: {
    filterAktifitas() {
      let sortedAktifitas = this.aktifitas.sort((a, b) => b.tanggal - a.tanggal);
      if (this.filter === 'aktif') {
        return sortedAktifitas.filter(a => !a.selesai);
      } else if (this.filter === 'selesai') {
        return sortedAktifitas.filter(a => a.selesai);
      } else {
        return sortedAktifitas;
      }
    }
  }
}

</script>

<style>

body {
  background-image: url('https://www.sap-express.id/blog/wp-content/uploads/2023/01/transportation-logistics-container-cargo-ship-cargo-plane-3d-rendering-illustration-1296x700.jpg');
  background-size: cover;
  background-position: center;
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 40px;
  background-color: #8f8f8f48;
  font-family: "Open Sans", sans-serif;
  color: #333;  
  border-radius: 15px;
}

h1 {
  text-align: center;
  font-size: 32px;
  font-weight: 600;
  margin-bottom: 40px;
  color: #fff ;
}

form {
  display: flex;
  margin-bottom: 20px;
}

input[type="text"] {
  flex-grow: 1;
  border: none;
  border-radius: 5px;
  padding: 16px;
  font-size: 18px;
  margin-right: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

button {
  background-color: #2c3e50;
  border: none;
  color: white;
  padding: 16px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 18px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
  margin: 2px;
}

button:hover {
  background-color: #e74c3c;
}

button.aktif {
  background-color: #e74c3c;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border-radius: 5px;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

span.selesai {
  text-decoration: line-through;
  color: #7f8c8d;
}

button.aktif {
    background-color: #0074D9;
    color: white;
  }

  button.selesai {
    text-decoration: line-through;
    color: grey;
  }
</style>