<template>
  <div style="background-color: lime">
    <div
      style="
        margin-left: 150px;
        margin-right: 150px;
        border: 5px dashed black;
        border-radius: 20px;
        background-color: orange;
      "
    >
      <marquee direction="right"
        ><h1 style="text-align: center">
          SELAMAT DATANG DI TOKO BUKU ONLINE
        </h1></marquee
      >
    </div>
    <div
      style="
        margin-left: 150px;
        width: 350px;
        margin-right: 150px;
        border: 5px dashed black;
        border-radius: 20px;
        background-color: orange;
        height: 100px;
        margin-top: 50px;
      "
    >
      <marquee
        ><h1 style="margin-left: 20px; margin-top: 20px">
          Form Peminjaman Buku
        </h1></marquee
      >
    </div>
    <div
      style="
        margin-left: 548px;
        margin-right: 195px;
        border: 5px dashed black;
        border-radius: 20px;
        background-color: orange;
      "
    >
      <marquee direction="up"
        ><h1 style="margin-left: 230px; margin-top: 15px">
          Tabel Daftar Buku
        </h1></marquee
      >
    </div>
    <br />
    <center>
      <form @submit.prevent="add">
        <center>
          <label style="padding: 10px; margin-right: 1215px" for=""
            >Nama Siswa:</label
          >
          <input
            required
            style="width: 300px; height: 25px; margin-right: 1000px"
            type="text"
            name="Judul_Buku"
            v-model="form.name"
          /><br /><br />
          <label
            style="padding: 10px; padding-right: 15px; margin-right: 1215px"
            for=""
            >Judul Buku:</label
          >
          <input
            required
            style="width: 300px; height: 25px; margin-right: 1000px"
            type="text"
            name="pengarang"
            v-model="form.pengarang"
          /><br /><br />
          <label
            style="padding: 5px; padding-right: 6px; margin-right: 1200px"
            for=""
            >Tanggal Pinjam:</label
          >
          <input
            required
            style="width: 300px; height: 25px; margin-right: 1000px"
            type="text"
            nmae="tahun_terbit"
            v-model="form.terbit"
          /><br /><br />
          <label style="padding: 18px; margin-right: 1153px" for=""
            >Tanggal Pengembalian:</label
          >
          <input
            required
            style="width: 300px; height: 25px; margin-right: 1000px"
            type="text"
            name="kategori"
            v-model="form.kategori"
          /><br /><br />
          <button
            style="
              padding: 10px;
              margin-right: 1000px;
              width: 310px;
              background-color: lightblue;
            "
            type="submit"
            v-show="!updateSubmit"
          >
            Add Peminjaman</button
          ><button
            style="
              padding: 10px;
              margin-right: 1000px;
              width: 310px;
              background-color: lightblue;
            "
            type="button"
            v-show="updateSubmit"
            @click="update(form)"
          >
            Perpanjang
          </button>
        </center>
        <br /><br />
      </form>
    </center>
    <center>
      <table
        border="10"
        style="
          padding: 5px;
          margin-top: -350px;
          margin-left: 350px;
          background-color: orange;
        "
      >
        <thead>
          <tr style="margin-left: 100px">
            <th style="padding: 20px">No</th>
            <th>Nama Siswa</th>
            <th>Judul Buku</th>
            <th>Tanggal Pinjam</th>
            <th>Tanggal Pengembalian</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <th>{{ user.id }}</th>
            <th>{{ user.name }}</th>
            <th>{{ user.pengarang }}</th>
            <th>{{ user.terbit }}</th>
            <th>{{ user.kategori }}</th>
            <th>
              {{ user.action }}
              <a
                style="background-color: green; color: white; padding: 2px"
                @click="edit(user)"
                >Perpanjang</a
              >
              |
              <a
                style="background-color: red; color: white; padding: 2px"
                @click="del(user)"
                >Kembali</a
              >
            </th>
          </tr>
        </tbody>
      </table>
    </center>
    <br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
  </div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
export default {
  data() {
    return {
      form: {
        id: "",
        name: "",
      },
      users: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/users")
        .then((res) => {
          this.users = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/users", this.form).then((res) => {
        this.load();
        this.form.id = "";
        this.form.name = "";
        this.form.pengarang = "";
        this.form.terbit = "";
        this.form.kategori = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.name = user.name;
      this.form.pengarang = user.pengarang;
      this.form.terbit = user.terbit;
      this.form.kategori = user.kategori;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/users/" + form.id, {
          name: this.form.name,
          pengarang: this.form.pengarang,
          terbit: this.form.terbit,
          kategori: this.form.kategori,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.name = "";
          this.form.pengarang = "";
          this.form.terbit = "";
          this.form.kategori = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/users/" + user.id).then((res) => {
        this.load();
        let index = this.users.indexOf(form.name);
        this.users.splice(index, 1);
      });
    },
  },
};
</script>