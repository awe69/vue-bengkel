<template>
  <v-img
    src="https://img3.goodfon.com/original/1920x1080/b/a1/material-desing-color-704.jpg"
    aspect-ratio="2">
    <v-container color="white">
      <v-row>
        <v-col>
          <v-layout align-center justify-center style="margin-top:50px">
            <v-flex xs12 sm8 md4>
              <v-card class="elevation-12">
                <v-toolbar dark color="red darken-4">
                  <v-toolbar-title>Register to Atma Bengkel</v-toolbar-title>
                  <v-spacer></v-spacer>

                </v-toolbar>
                <v-card-text>
                  <v-form>
                    <v-text-field v-model="nama" label="Name" type="text"></v-text-field>
                    <v-text-field v-model="username" label="Username" type="text"></v-text-field>
                    <v-text-field v-model="tgl_lahir" label="Birhdate" type="date"></v-text-field>
                    <v-text-field v-model="email" label="Email" type="email"></v-text-field>
                    <v-text-field id="password" v-model="password" label="Password" type="password" name="password"
                      required>
                    </v-text-field>
                    <v-text-field id="confirmPassword" v-model="confirmPassword" label="Confirm Password"
                      name="confirmPassword" type="password" :rules="[comparePasswords]"></v-text-field>
                  </v-form>
                </v-card-text>
                <v-card-text>
                  Have an account? <a href="/">Login here</a>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="error" type='submit' @click="registerUser()">REGISTER</v-btn>
                </v-card-actions>
              </v-card>
            </v-flex>
            <v-snackbar v-model="snackbar" :color="color" :multi-line="true" :timeout="3000">
              {{ text }}
              <v-btn dark text @click="snackbar = false">Close</v-btn>
            </v-snackbar>
          </v-layout>
        </v-col>
      </v-row>
    </v-container>
    <v-footer padless :inset="footer.inset" app absolute>
      <v-card class="flex" flat tile>
        <v-card-text class="py-2 text-left">
          ©{{ new Date().getFullYear() }} — Created with <v-icon>mdi-heart</v-icon> by Kelompok 6 PAW™ — Raditya Dimas
          Bagus Santoso 9234 — Alexander Rivelino Aldo Aldiero 9395 — Yanuarius Hermawan 9454 — Alisha Aileen 9457
        </v-card-text>
      </v-card>
    </v-footer>
  </v-img>
</template>

<script>
  export default {
    data() {
      return {
        footer: {
          inset: false,
        },
        email: '',
        username: '',
        tgl_lahir: '',
        nama: '',
        password: '',
        confirmPassword: '',
        snackbar: false,
        color: null,
        text: ''
      }
    },
    computed: {
      comparePasswords() {
        return this.password !== this.confirmPassword ? 'Passwords do not match' : ''
      }
    },
    methods: {
      registerUser() {
        this.user = new FormData();
        this.user.append('username', this.username);
        this.user.append('email', this.email);
        this.user.append('tgl_lahir', this.tgl_lahir);
        this.user.append('nama', this.nama);
        this.user.append('password', this.password);
        var url = this.$apiUrl + "/User"
        this.load = true
        this.$http.post(url, this.user).then(response => {
          this.snackbar = true; //mengaktifkan snackbar               
          this.color = 'green'; //memberi warna snackbar               
          this.text =
          'Succeed Register, Check your email inbox for the verification code!'; //memasukkan pesan ke snackbar               
          this.load = false;
          this.dialog = false;
        }).catch(error => {
          this.errors = error
          this.snackbar = true;
          this.text = 'Try Again';
          this.color = 'red';
          this.load = false;
        })
      }
    }
  }
</script>