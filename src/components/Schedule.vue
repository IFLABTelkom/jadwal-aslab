<template>
  <div>
    <button class="button is-primary">hey</button>
    <div class="quotes">
      <p>Quotes of the Day</p>
      <p>{{`"${quotes}"`}}</p>
      <p>- {{author}}</p>
    </div>
    <div class="clock">
      <p>{{hariTanggalTahun}}</p>
      <p>{{ hour }}:{{ minute }}:{{ second }}</p>
      <p>{{ jagaLab}}</p>
    </div>
    <div class="schedule">
      <p>Aslab Jaga Saat Ini</p>
      <h1>{{day}}</h1>
      <div class="aslab-jaga">
        <div v-if="workHours">
          <div>{{aslabNow[0]}}</div>
          <div>{{aslabNow[1]}}</div>
          <div>{{aslabNow[2]}}</div>
        </div>
        <div v-else>
          {{aslabNow}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { setInterval } from "timers";
import axios from "axios";
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
      jadwalAslab: {
        senin: {
          jam: {
            6: ["KHM", "AFG", "VIN"],
            7: ["KHM", "AFG", "VIN"],
            8: ["KHM", "AFG", "VIN"],
            9: ["KHM", "AFG", "VIN"],
            10: ["KHM", "AFG", "VIN"],
            11: ["KHM", "AFG", "VIN"],
            12: ["VIN", "AFG", "RAH"],
            13: ["VIN", "AFG", "RAH"],
            14: ["VIN", "AFG", "RAH"],
            15: ["KHM", "AFG", "RAH"],
            16: ["KHM", "AFG", "RAH"],
            17: ["KHM", "AFG", "RAH"]
          }
        },
        selasa: {
          jam: {
            6: ["AFG", "RAH", "KHM"],
            7: ["AFG", "RAH", "KHM"],
            8: ["AFG", "RAH", "KHM"],
            9: ["AFG", "RAH", "KHM"],
            10: ["AFG", "RAH", "KHM"],
            11: ["AFG", "RAH", "HRD"],
            12: ["AFG", "RAH", "HRD"],
            13: ["AFG", "VIN", "HRD"],
            14: ["AFG", "VIN", "HRD"],
            15: ["AFG", "VIN", "HRD"],
            16: ["AFG", "VIN", "HRD"],
            17: ["AFG", "VIN", "HRD"]
          }
        },
        rabu: {
          jam: {
            6: ["RAH", "VIN", "KHM"],
            7: ["RAH", "VIN", "KHM"],
            8: ["RAH", "VIN", "KHM"],
            9: ["RAH", "VIN", "KHM"],
            10: ["RAH", "VIN", "KHM"],
            11: ["RAH", "VIN", "KHM"],
            12: ["AFG", "VIN", "KHM"],
            13: ["AFG", "VIN", "KHM"],
            14: ["AFG", "VIN", "KHM"],
            15: ["AFG", "VIN", "KHM"],
            16: ["AFG", "VIN", "KHM"],
            17: ["AFG", "VIN", "KHM"]
          }
        },
        kamis: {
          jam: {
            6: ["RAH", "AFG", "KHM"],
            7: ["RAH", "AFG", "KHM"],
            8: ["RAH", "AFG", "KHM"],
            9: ["RAH", "AFG", "KHM"],
            10: ["RAH", "AFG", "KHM"],
            11: ["RAH", "AFG", "KHM"],
            12: ["RAH", "AFG", "KHM"],
            13: ["RAH", "VIN", "KHM"],
            14: ["RAH", "VIN", "KHM"],
            15: ["RAH", "VIN", "HRD"],
            16: ["RAH", "VIN", "HRD"],
            17: ["RAH", "VIN", "HRD"]
          }
        },
        jumat: {
          jam: {
            7: ["RAH", "AFG", "VIN"],
            8: ["RAH", "AFG", "VIN"],
            9: ["RAH", "AFG", "VIN"],
            10: ["RAH", "AFG", "VIN"],
            13: ["RAH", "KHM", "VIN"],
            14: ["RAH", "KHM", "VIN"],
            15: ["RAH", "KHM", "VIN"],
            16: ["RAH", "KHM", "VIN"],
            17: ["RAH", "KHM", "VIN"]
          }
        },
        sabtu: {
          jam: {
            12: ["MFS", "GSU", "MFA"],
            13: ["MFS", "GSU", "MFA"],
            14: ["MFS", "GSU", "MFA"]
          }
        }
      }
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
          return this.jadwalAslab.senin.jam[Number(this.hour)];
          break;
        case 2:
          return this.jadwalAslab.selasa.jam[Number(this.hour)];
          break;
        case 3:
          return this.jadwalAslab.rabu.jam[Number(this.hour)];
          break;
        case 4:
          return this.jadwalAslab.kamis.jam[Number(this.hour)];
          break;
        case 5:
          if (this.jadwalAslab.jumat.jam[Number(this.hour)]) {
            return this.jadwalAslab.jumat.jam[Number(this.hour)];
          } else {
            this.workHours = false;
            return `Tidak ada jadwal jaga pada pukul ${
              this.hour
            } - ${this.hour.replace(/.$/, Number(this.hour) + 1)}`;
          }
          return;
          break;
        case 6:
          return this.jadwalAslab.sabtu.jam[Number(this.hour)];
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
</style>