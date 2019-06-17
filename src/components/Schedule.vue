<template>
  <div class="hero-body">
    <div class="container">
      <div class="clock">
        <p class="is-size-1 has-text-grey-lighter">{{hariTanggalTahun}}</p>
        <p
          class="is-size-1 has-text-grey-lighter"
          style="padding-bottom: 50px;"
        >{{ hour }}:{{ minute }}:{{ second }}</p>
        <p class="is-size-1 has-text-grey-lighter">{{ jagaLab}}</p>
      </div>
      <div class="quotes">
        <p class="is-size-1 has-text-grey-lighter">Quote of the Day</p>
        <p class="is-size-1 has-text-grey-lighter">{{quotes}}</p>
        <p
          class="is-size-1 has-text-grey-lighter"
          style="padding-bottom: 50px;"
        >- {{author}}</p>
      </div>
      <div class="schedule">
        <p class="is-size-1 has-text-grey-lighter">Jadwal Jaga</p>
        <div class="aslab-jaga">
          <div v-if="workHours">
            <div class="columns">
              <div class="column">
                <div class="box">
                  <div class="content">
                    <p class="is-size-1 has-text-grey-lighter">{{aslabNow[0]}}</p>
                  </div>
                </div>
              </div>
              <div class="column">
                <div class="box">
                  <div class="content">
                    <p class="is-size-1 has-text-grey-lighter">{{aslabNow[1]}}</p>
                  </div>
                </div>
              </div>
              <div class="column">
                <div class="box">
                  <div class="content">
                    <p class="is-size-1 has-text-grey-lighter">{{aslabNow[2]}}</p>
                  </div>
                </div>
              </div>
            </div>
            <p class="is-size-1 has-text-grey-lighter">Aslab Pengganti</p>
            <div class="columns">
              <div class="column">
                <div class="box">
                  <div class="field">
                    <div class="select is-large is-danger">
                      <select
                        v-model="aslabPengganti1"
                        @change="updateAslab1($event)"
                      >
                        <option v-for="aslab in daftarAslab">
                          {{aslab}}
                        </option>
                      </select>
                    </div>
                  </div>
                </div>
              </div>
              <div class="column">
                <div class="box">
                  <div class="field">
                    <div class="select is-large is-danger">
                      <select
                        v-model="aslabPengganti2"
                        @change="updateAslab2($event)"
                      >
                        <option v-for="aslab in daftarAslab">
                          {{aslab}}
                        </option>
                      </select>
                    </div>
                  </div>
                </div>
              </div>
              <div class="column">
                <div class="box">
                  <div class="field">
                    <div class="select is-large is-danger">
                      <select
                        v-model="aslabPengganti3"
                        @change="updateAslab3($event)"
                      >
                        <option v-for="aslab in daftarAslab">
                          {{aslab}}
                        </option>
                      </select>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div
            v-else
            class="is-size-1 has-text-grey-lighter"
          >
            <div class="columns">
              <div class="column">
                <div class="box">
                  <div class="content">
                    <p class="is-size-1 has-text-grey-lighter">{{aslabNow}}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="history">
        <p
          class="is-size-1 has-text-grey-lighter"
          style="padding-top:200px; padding-bottom:25px;"
        >History Aslab Jaga {{day}}</p>
        <table class="table is-narrow">
          <thead>
            <tr>
              <th class="has-text-grey-lighter">Aslab 1</th>
              <th class="has-text-grey-lighter">Aslab 2</th>
              <th class="has-text-grey-lighter">Aslab 3</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="aslab in historyAslab">
              <td class="has-text-grey-lighter">{{aslab[0]}}</td>
              <td class="has-text-grey-lighter">{{aslab[1]}}</td>
              <td class="has-text-grey-lighter">{{aslab[2]}}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import { setInterval } from "timers";
import axios from "axios";
import JadwalAslab from "../JadwalAslab.js";
import DaftarAslab from "../DaftarAslab.js";
import Days from "../Days.js";
import { log } from "util";
import { async } from "q";
export default {
  name: "Schedule",
  data() {
    return {
      aslabPengganti1: null,
      aslabPengganti2: null,
      aslabPengganti3: null,
      daftarAslab: DaftarAslab,
      historyAslab: [],
      workHours: true,
      hour: "",
      minute: "",
      second: "",
      day: "",
      dayNumber: "",
      date: "",
      month: "",
      year: "",
      days: Days,
      months: [
        "Januari",
        "Februari",
        "Maret",
        "April",
        "Mei",
        "Juni",
        "Juli",
        "Agustus",
        "September",
        "Oktober",
        "November",
        "Desember"
      ],
      quotes: "",
      author: "",
      jadwalAslab: JadwalAslab
    };
  },
  methods: {
    updateAslab1: function(event) {
      this.aslabNow[0] = this.aslabPengganti1;
    },
    updateAslab2: function(event) {
      this.aslabNow[1] = this.aslabPengganti2;
    },
    updateAslab3: function(event) {
      this.aslabNow[2] = this.aslabPengganti3;
    },
    updateStates: async function() {
      let now = new Date();
      this.hour = this.leftPadding("" + now.getHours());
      this.minute = this.leftPadding("" + now.getMinutes());
      this.second = this.leftPadding("" + now.getSeconds());
      this.dayNumber = now.getDay();
      this.day = this.days[now.getDay()];
      this.date = now.getDate();
      this.month = this.months[now.getMonth()];
      this.year = now.getFullYear();
      if (now.getHours() === 0) {
        // Update quote when the it's midnight
        try {
          const response = await axios.get("http://quotes.rest/qod");
          this.quotes = response.data.contents.quotes[0]["quote"];
          this.author = response.data.contents.quotes[0]["author"];
        } catch (error) {
          console.log(error);
        }
      } else if (now.getMinutes() === 59 && this.workHours === true) {
        // Push history aslab dan kosongkan aslab pengganti
        this.historyAslab.push([...this.aslabNow]);
        this.aslabPengganti1 = null;
        this.aslabPengganti2 = null;
        this.aslabPengganti3 = null;
      } else if (now.getSeconds() % 5 === 0 && this.workHours === true) {
        // this part is for debugging purposes
        // this.historyAslab.push([...this.aslabNow]);
        // this.aslabPengganti1 = null;
        // this.aslabPengganti2 = null;
        // this.aslabPengganti3 = null;
      }
      // TODO: Add feature to clear the history before the day begins
    },
    leftPadding: function(str) {
      const leftPadding = "00";
      return leftPadding.substring(0, leftPadding.length - str.length) + str;
    }
  },
  mounted: function() {
    setInterval(this.updateStates, 1000);
  },
  computed: {
    jagaLab: function() {
      if (this.day === "Minggu") {
        return "Jaga lab untuk peminjaman/acara? Jangan lupa lapor ke laboran untuk penghonorannya ya";
      }
    },
    hariTanggalTahun: function() {
      return `${this.day}, ${this.date} ${this.month}, ${this.year}`;
    },
    greetings: function() {
      let hourInt = Number(this.hour);
      switch (true) {
        case hourInt >= 5 && hourInt < 11:
          return "Semangat pagi!";
          break;
        case hourInt >= 11 && hourInt < 18:
          return "Jangan lupa rekap bos, jaga kerapihan!";
        case hourInt >= 18 && hourInt < 5:
          return "Jam kerja sudah selesai bos, lembur?";
        default:
          return `Jam berapa ini bos ? Sekarang jam ${hourInt}`;
          break;
      }
    },
    aslabNow: function() {
      switch (this.dayNumber) {
        case 0:
          return ["Minggu", "Takde", "Jadwal"];
          break;
        case 1:
          if (this.jadwalAslab.senin.jam[Number(this.hour)]) {
            this.workHours = true;

            return this.jadwalAslab.senin.jam[Number(this.hour)];
          } else {
            this.workHours = false;
            return "Saat ini bukan jam kerja";
          }
          break;
        case 2:
          if (this.jadwalAslab.selasa.jam[Number(this.hour)]) {
            this.workHours = true;
            return this.jadwalAslab.selasa.jam[Number(this.hour)];
          } else {
            this.workHours = false;
            return "Saat ini bukan jam kerja";
          }
          break;
        case 3:
          if (this.jadwalAslab.rabu.jam[Number(this.hour)]) {
            this.workHours = true;
            return this.jadwalAslab.rabu.jam[Number(this.hour)];
          } else {
            this.workHours = false;
            return "Saat ini bukan jam kerja";
          }
          break;
        case 4:
          if (this.jadwalAslab.kamis.jam[Number(this.hour)]) {
            this.workHours = true;
            return this.jadwalAslab.kamis.jam[Number(this.hour)];
          } else {
            this.workHours = false;
            return "Saat ini bukan jam kerja";
          }
          break;
        case 5:
          if (this.jadwalAslab.jumat.jam[Number(this.hour)]) {
            this.workHours = true;
            return this.jadwalAslab.jumat.jam[Number(this.hour)];
          } else {
            this.workHours = false;
            return "Saat ini bukan jam kerja";
          }
          return;
          break;
        case 6:
          if (this.jadwalAslab.sabtu.jam[Number(this.hour)]) {
            this.workHours = true;
            return this.jadwalAslab.sabtu.jam[Number(this.hour)];
          } else {
            this.workHours = false;
            return "Saat ini bukan jam kerja";
          }
          return;
          break;
        default:
          return "the day counter broke";
          break;
      }
      return this.jadwalAslab;
    }
  },
  async created() {
    try {
      const response = await axios.get("https://quotes.rest/qod");
      this.quotes = response.data.contents.quotes[0]["quote"];
      this.author = response.data.contents.quotes[0]["author"];
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.table {
  margin-left: auto;
  margin-right: auto;
}
.option {
  background: hsla(0, 0%, 30%, 1);
}
</style>