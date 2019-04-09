<template>
  <div class="hero-body">
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
          style="padding-bottom: 100px;"
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
            {{aslabNow}}
          </div>
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
            6: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "VIN"],
            7: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "VIN"],
            8: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "VIN"],
            9: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "VIN"],
            10: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "VIN"],
            11: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "VIN"],
            12: ["VIN", "AFG, S.Si.Kom.", "RAH"],
            13: ["VIN", "AFG, S.Si.Kom.", "RAH"],
            14: ["VIN", "AFG, S.Si.Kom.", "RAH"],
            15: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "RAH"],
            16: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "RAH"],
            17: ["KHM, S.Kom.", "AFG, S.Si.Kom.", "RAH"]
          }
        },
        selasa: {
          jam: {
            6: ["AFG, S.Si.Kom.", "RAH", "KHM, S.Kom."],
            7: ["AFG, S.Si.Kom.", "RAH", "KHM, S.Kom."],
            8: ["AFG, S.Si.Kom.", "RAH", "KHM, S.Kom."],
            9: ["AFG, S.Si.Kom.", "RAH", "KHM, S.Kom."],
            10: ["AFG, S.Si.Kom.", "RAH", "KHM, S.Kom."],
            11: ["AFG, S.Si.Kom.", "RAH", "HRD"],
            12: ["AFG, S.Si.Kom.", "RAH", "HRD"],
            13: ["AFG, S.Si.Kom.", "VIN", "HRD"],
            14: ["AFG, S.Si.Kom.", "VIN", "HRD"],
            15: ["AFG, S.Si.Kom.", "VIN", "HRD"],
            16: ["AFG, S.Si.Kom.", "VIN", "HRD"],
            17: ["AFG, S.Si.Kom.", "VIN", "HRD"]
          }
        },
        rabu: {
          jam: {
            6: ["RAH", "VIN", "KHM, S.Kom."],
            7: ["RAH", "VIN", "KHM, S.Kom."],
            8: ["RAH", "VIN", "KHM, S.Kom."],
            9: ["RAH", "VIN", "KHM, S.Kom."],
            10: ["RAH", "VIN", "KHM, S.Kom."],
            11: ["RAH", "VIN", "KHM, S.Kom."],
            12: ["AFG, S.Si.Kom.", "VIN", "KHM, S.Kom."],
            13: ["AFG, S.Si.Kom.", "VIN", "KHM, S.Kom."],
            14: ["AFG, S.Si.Kom.", "VIN", "KHM, S.Kom."],
            15: ["AFG, S.Si.Kom.", "VIN", "KHM, S.Kom."],
            16: ["AFG, S.Si.Kom.", "VIN", "KHM, S.Kom."],
            17: ["AFG, S.Si.Kom.", "VIN", "KHM, S.Kom."]
          }
        },
        kamis: {
          jam: {
            6: ["RAH", "AFG, S.Si.Kom.", "KHM, S.Kom."],
            7: ["RAH", "AFG, S.Si.Kom.", "KHM, S.Kom."],
            8: ["RAH", "AFG, S.Si.Kom.", "KHM, S.Kom."],
            9: ["RAH", "AFG, S.Si.Kom.", "KHM, S.Kom."],
            10: ["RAH", "AFG, S.Si.Kom.", "KHM, S.Kom."],
            11: ["RAH", "AFG, S.Si.Kom.", "KHM, S.Kom."],
            12: ["RAH", "AFG, S.Si.Kom.", "KHM, S.Kom."],
            13: ["RAH", "VIN", "KHM, S.Kom."],
            14: ["RAH", "VIN", "KHM, S.Kom."],
            15: ["RAH", "VIN", "HRD"],
            16: ["RAH", "VIN", "HRD"],
            17: ["RAH", "VIN", "HRD"]
          }
        },
        jumat: {
          jam: {
            7: ["RAH", "AFG, S.Si.Kom.", "VIN"],
            8: ["RAH", "AFG, S.Si.Kom.", "VIN"],
            9: ["RAH", "AFG, S.Si.Kom.", "VIN"],
            10: ["RAH", "AFG, S.Si.Kom.", "VIN"],
            13: ["RAH", "KHM, S.Kom.", "VIN"],
            14: ["RAH", "KHM, S.Kom.", "VIN"],
            15: ["RAH", "KHM, S.Kom.", "VIN"],
            16: ["RAH", "KHM, S.Kom.", "VIN"],
            17: ["RAH", "KHM, S.Kom.", "VIN"]
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