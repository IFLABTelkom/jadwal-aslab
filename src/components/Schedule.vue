<template>
  <div
    class="hero-body"
    id="container1"
  >
    <div class="container">
      <div class="clock">
        <p class="is-size-1 has-text-grey-lighter">{{hariTanggalTahun}}</p>
        <p
          class="is-size-1 has-text-grey-lighter"
          style="padding-bottom: 150px;"
        >{{ hour }}:{{ minute }}:{{ second }}</p>
        <p class="is-size-1 has-text-grey-lighter">{{ jagaLab}}</p>
      </div>
      <div class="quotes">
        <p class="is-size-1 has-text-grey-lighter">Quotes of the Day</p>
        <p class="is-size-1 has-text-grey-lighter">{{quotes}}</p>
        <p
          class="is-size-1 has-text-grey-lighter"
          style="padding-bottom: 150px;"
        >- {{author}}</p>
      </div>
      <div class="schedule">
        <p class="is-size-1 has-text-grey-lighter">Aslab Jaga</p>
        <p class="is-size-1 has-text-grey-lighter">{{day}}</p>
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
    </div>
  </div>
</template>

<script>
import { setInterval } from "timers";
import axios from "axios";
import JadwalAslab from "../JadwalAslab.js";
import Days from "../Days.js";
export default {
  name: "Schedule",
  data() {
    return {
      workHours: true,
      hour: "",
      minute: "",
      second: "",
      day: "",
      dayNumber: "",
      date: "",
      month: "",
      year: "",
      days: ["Minggu", "Senin", "Selasa", "Rabu", "Kamis", "Jumat", "Sabtu"],
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
    updateClock: function() {
      let now = new Date();
      this.hour = this.leftPadding("" + now.getHours());
      this.minute = this.leftPadding("" + now.getMinutes());
      this.second = this.leftPadding("" + now.getSeconds());
      this.dayNumber = now.getDay();
      this.day = this.days[now.getDay()];
      this.date = now.getDate();
      this.month = this.months[now.getMonth()];
      this.year = now.getFullYear();
    },
    leftPadding: function(str) {
      const leftPadding = "00";
      return leftPadding.substring(0, leftPadding.length - str.length) + str;
    }
  },
  mounted: function() {
    setInterval(this.updateClock, 1000);
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
      const response = await axios.get("http://quotes.rest/qod");
      this.quotes = response.data.contents.quotes[0]["quote"];
      this.author = response.data.contents.quotes[0]["author"];
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
.hero-body {
  overflow: hidden;
}
</style>