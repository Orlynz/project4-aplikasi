<template>
  <div id="a">
    <h1 id="s">
      <marquee>Welcome To Library Orlynz :)</marquee>
    </h1>
    <div class="b">
      <form @submit.prevent="add" action="login.php" method="POST">
        <div class="d">
          <h4>
            Form Peminjaman Buku
            <hr size="2px" color="black" />
          </h4>
          <p>
            <label><b>Nama Siswa:</b></label
            ><br />
            <input
              style="width: 300px"
              placeholder="Masukkan Nama..."
              type="text"
              v-model="form.nama"
              required
            />
          </p>
          <p>
            <label><b>Judul Buku:</b></label
            ><br />
            <input
              style="width: 300px"
              placeholder="Judul Buku..."
              type="text"
              v-model="form.judul"
              required
            />
          </p>
          <p>
            <label><b>Tanggal Pinjam:</b> </label><br />
            <input
              style="width: 300px"
              placeholder="Tanggal Pinjam..."
              type="text"
              v-model="form.tanggalpinjam"
              required
            />
          </p>
          <p>
            <label><b>Tanggal Pengembalian:</b></label
            ><br />
            <input
              style="width: 300px"
              placeholder="Tanggal Pengembalian..."
              type="text"
              v-model="form.tanggalkembali"
              required
            />
          </p>
          <button
            class="btn btn-info"
            type="submit"
            id="f"
            v-show="!updateSubmit"
          >
            Add Pinjaman
          </button>
          <button
            class="btn btn-info"
            type="submit"
            id="f"
            v-show="updateSubmit"
            @click="update(form)"
          >
            Update
          </button>
        </div>
        <p><i>*Syarat & Ketentuan Berlaku</i></p>
      </form>

      <div class="c">
        <h4 id="ab">Tabel Daftar Buku</h4>
        <table class="table">
          <thead class="thead-dark">
            <tr>
              <th scope="col">No</th>
              <th scope="col">Nama Siswa</th>
              <th scope="col">Judul <br />Buku</th>
              <th scope="col">Tanggal Pinjam</th>
              <th scope="col">Tanggal Pengembalian</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody class="table-info">
            <tr v-for="user in Project4" :key="user.id">
              <td>
                <b>{{ user.id }}</b>
              </td>
              <td>{{ user.nama }}</td>
              <td>{{ user.judul }}</td>
              <td>
                {{ user.tanggalpinjam }}
              </td>
              <td>
                {{ user.tanggalkembali }}
              </td>
              <td>
                <b>
                  <u>
                    <button
                      style="
                        width: 115px;
                        text-align: center;
                        margin-bottom: 5px;
                      "
                      class="btn btn-success"
                      @click="edit(user)"
                    >
                      Perpanjang
                    </button>
                    <br />
                    <button
                      style="width: 115px; text-align: center"
                      class="btn btn-danger"
                      @click="del(user)"
                    >
                      Kembalikan
                    </button>
                  </u>
                </b>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
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
        nama: "",
        judul: "",
        tanggalpinjam: "",
        tanggalkembali: "",
      },
      Project4: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/Project4")
        .then((res) => {
          this.Project4 = res.data; //respon dari rest api dimasukan ke users
        })
        .catch((err) => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/Project4", this.form).then((res) => {
        this.load();
        this.form.nama = "";
        this.form.judul = "";
        this.form.tanggalpinjam = "";
        this.form.tanggalkembali = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.nama = user.nama;
      this.form.judul = user.judul;
      this.form.tanggalpinjam = user.tanggalpinjam;
      this.form.tanggalkembali = user.tanggalkembali;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/Project4/" + form.id, {
          nama: this.form.nama,
          judul: this.form.judul,
          tanggalpinjam: this.form.tanggalpinjam,
          tanggalkembali: this.form.tanggalkembali,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.nama = "";
          this.form.judul = "";
          this.form.tanggalpinjam = "";
          this.form.tanggalkembali = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/Project4/" + user.id).then((res) => {
        this.load();
        let index = this.Project4.indexOf(form.nama);
        this.Project4.splice(index, 1);
      });
    },
  },
};
</script>
<style>
body {
  background-color: lightblue;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
#a {
  border-radius: 25px;
  background-color: skyblue;
  margin-left: 50px;
  margin-right: 50px;
  margin-top: 30px;
  margin-bottom: 30px;
  padding-top: 30px;
  padding-bottom: 30px;
}
.b {
  display: flex;
  text-align: left;
  padding-left: 30px;
}
.c {
  width: 900px;
  margin-left: 40px;
  margin-right: 40px;
  padding-top: 20px;
  font-family: "Courier New", Courier, monospace;
  text-align: center;
}
.d {
  padding: 20px;
  margin-top: 28px;
  border: 2px dotted black;
}
#s {
  text-align: center;
  color: white;
  padding: 15px;
  font-family: cursive;
  margin-left: 150px;
  margin-right: 150px;
  border: 2px dashed black;
  border-radius: 20px;
}
#f {
  width: 300px;
}
#ab {
  text-align: left;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
</style>