<template>
  <div class="flex items-center justify-center p-4 min-h-screen bg-gradient-to-br from-[#0a1a3f] via-[#143d77] to-[#1c5fbf]">
    <div class="container max-w-lg">
      <div class="glass shadow-xl p-6 md:p-8 rounded-3xl fade-in">

        <h2 class="text-center text-3xl text-white font-bold mb-4">
          🔮 Life Path Number
        </h2>

        <label class="form-label text-blue-100 fw-semibold">Pilih Tanggal Lahir:</label>
        <input
          type="date"
          v-model="birthdate"
          class="form-control mb-3 rounded-lg shadow-sm"
        />

        <button
          @click="calculateLifePath"
          class="w-100 btn btn-primary py-2 rounded-lg btn-custom fw-bold"
        >
          Hitung Sekarang
        </button>

        <div v-if="error" class="mt-4 text-red-300 text-center font-semibold">
          {{ error }}
        </div>

        <div v-if="calculationSteps.length" class="result-box text-center text-white fade-in mt-4">
          <h4 class="font-bold text-blue-100">🔢 Proses Perhitungan:</h4>
          <p class="text-blue-100"><strong>{{ calculationSteps[0] }}</strong></p>
          <p v-for="(step, idx) in calculationSteps.slice(1)" :key="idx" class="text-blue-100">{{ step }}</p>
        </div>

        <div v-if="finalNumber" class="mt-4 text-center fade-in">
          <p class="text-2xl font-bold text-blue-200">✨ Life Path Number Anda: {{ finalNumber }}</p>
          <p class="text-blue-200 text-lg">{{ titles[finalNumber] }}</p>
          <p class="mt-2 text-blue-200">{{ meanings[finalNumber] }}</p>

          <div class="result-box text-left text-white mt-4">
            <h4 class="text-blue-100 font-bold mb-2">🌟 Informasi Lengkap</h4>
            <p><strong>🔹 Elemen:</strong> {{ elements[finalNumber] }}</p>
            <p><strong>🔹 Planet Pengaruh:</strong> {{ planets[finalNumber] }}</p>
            <p><strong>🔹 Warna Keberuntungan:</strong> {{ colors[finalNumber] }}</p>
            <p><strong>🔹 Hari Terbaik:</strong> {{ bestDays[finalNumber] }}</p>

            <h4 class="text-blue-100 font-bold mt-4">✨ Sifat Positif</h4>
            <p>{{ positiveTraits[finalNumber] }}</p>

            <h4 class="text-blue-100 font-bold mt-4">⚠ Sifat Negatif</h4>
            <p>{{ negativeTraits[finalNumber] }}</p>
          </div>

          <div class="result-box text-white mt-4">
            <h4 class="text-blue-100 font-bold">💘 Kecocokan</h4>
            <p><strong>✔ Cocok dengan:</strong> {{ compatibilityList[finalNumber] }}</p>
            <p><strong>❌ Tidak cocok dengan:</strong> {{ incompatibilityList[finalNumber] }}</p>
          </div>
        </div>

      </div>

      <footer class="text-2xl text-center text-white mt-6">
        Created ❤️ by Devis Wisley
      </footer>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import "bootstrap/dist/css/bootstrap.min.css";
import './style.css';

type LifePath = 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9;

export default defineComponent({
  name: "App",
  setup() {
    const birthdate = ref<string>("");
    const error = ref<string>("");
    const calculationSteps = ref<string[]>([]);
    const finalNumber = ref<LifePath | null>(null);

    const titles: Record<LifePath, string> = {
      1: "The Leader",
      2: "The Peacemaker",
      3: "The Creator",
      4: "The Builder",
      5: "The Adventurer",
      6: "The Nurturer",
      7: "The Thinker",
      8: "The Powerhouse",
      9: "The Humanitarian",
    };

    const meanings: Record<LifePath, string> = {
      1: "Pemimpin alami, mandiri, ambisius, dan penuh inovasi.",
      2: "Diplomatis, sensitif, lembut, penyayang, ahli bekerja sama.",
      3: "Kreatif, ceria, komunikatif, penuh ide segar.",
      4: "Disiplin, stabil, pekerja keras, berorientasi sistem.",
      5: "Bebas, fleksibel, petualang, menyukai perubahan.",
      6: "Penyayang, peduli, harmonis, pelindung keluarga.",
      7: "Analitis, spiritual, pemikir mendalam, mencari kebenaran.",
      8: "Ambisius, kuat secara finansial, pemimpin kuat.",
      9: "Dermawan, humanis, punya empati besar pada dunia.",
    };

    const elements: Record<LifePath, string> = {
      1: "🔥 Fire",
      2: "💧 Water",
      3: "🔥 Fire",
      4: "🌍 Earth",
      5: "💨 Air",
      6: "🌍 Earth",
      7: "💧 Water",
      8: "🔥 Fire",
      9: "💨 Air",
    };

    const planets: Record<LifePath, string> = {
      1: "☀ Sun",
      2: "🌙 Moon",
      3: "🌟 Jupiter",
      4: "🪐 Saturn",
      5: "☿ Mercury",
      6: "🌍 Venus",
      7: "🔭 Neptune",
      8: "♄ Saturn",
      9: "♂ Mars",
    };

    const colors: Record<LifePath, string> = {
      1: "Merah, Emas",
      2: "Putih, Silver",
      3: "Kuning, Orange",
      4: "Hijau, Coklat",
      5: "Biru Muda, Cyan",
      6: "Pink, Hijau Pastel",
      7: "Biru Tua, Indigo",
      8: "Hitam, Ungu",
      9: "Merah Tua, Biru Navy",
    };

    const bestDays: Record<LifePath, string> = {
      1: "Senin & Minggu",
      2: "Senin & Jumat",
      3: "Kamis",
      4: "Sabtu",
      5: "Rabu",
      6: "Jumat",
      7: "Senin",
      8: "Sabtu",
      9: "Selasa",
    };

    const positiveTraits: Record<LifePath, string> = {
      1: "Mandiri, percaya diri, inovatif, kompetitif, motivator kuat.",
      2: "Empati tinggi, diplomatis, penyayang, penengah konflik, setia.",
      3: "Kreatif, humoris, ekspresif, optimis, mudah bergaul.",
      4: "Stabil, disiplin, detail-oriented, pekerja keras, dapat dipercaya.",
      5: "Berani, fleksibel, petualang, cepat beradaptasi, energik.",
      6: "Peduli, penuh kasih, harmonis, bertanggung jawab, protektif.",
      7: "Bijaksana, analitis, intuitif, logis, filosofis.",
      8: "Ambisius, berbakat bisnis, tegas, organisator hebat, visioner.",
      9: "Dermawan, humanis, idealis, penuh empati, inspiratif.",
    };

    const negativeTraits: Record<LifePath, string> = {
      1: "Keras kepala, individualis, terlalu dominan, cepat marah.",
      2: "Terlalu sensitif, mudah cemas, bergantung pada orang lain.",
      3: "Moody, tidak fokus, boros, terlalu dramatis.",
      4: "Kaku, perfeksionis, kurang fleksibel, keras hati.",
      5: "Tidak konsisten, impulsif, mudah bosan, sulit komitmen.",
      6: "Terlalu protektif, suka mengatur, mudah tersinggung.",
      7: "Terlalu tertutup, overthinking, sulit percaya orang lain.",
      8: "Terlalu ambisius, materialistis, keras, dominan.",
      9: "Terlalu emosional, mudah dimanfaatkan, terlalu idealis.",
    };

    const compatibilityList: Record<LifePath, string> = {
      1: "3 (The Creator), 5 (The Adventurer), 7 (The Thinker)",
      2: "6 (The Nurturer), 8 (The Powerhouse), 9 (The Humanitarian)",
      3: "1 (The Leader), 5 (The Adventurer), 7 (The Thinker)",
      4: "2 (The Peacemaker), 6 (The Nurturer), 8 (The Powerhouse)",
      5: "1 (The Leader), 3 (The Creator), 7 (The Thinker)",
      6: "2 (The Peacemaker), 4 (The Builder), 9 (The Humanitarian)",
      7: "1 (The Leader), 3 (The Creator), 5 (The Adventurer)",
      8: "2 (The Peacemaker), 4 (The Builder), 6 (The Nurturer)",
      9: "2 (The Peacemaker), 6 (The Nurturer), 8 (The Powerhouse)",
    };

    const incompatibilityList: Record<LifePath, string> = {
      1: "2 (The Peacemaker), 4 (The Builder), 8 (The Powerhouse)",
      2: "1 (The Leader), 5 (The Adventurer), 7 (The Thinker)",
      3: "4 (The Builder), 6 (The Nurturer), 8 (The Powerhouse)",
      4: "3 (The Creator), 5 (The Adventurer), 9 (The Humanitarian)",
      5: "2 (The Peacemaker), 4 (The Builder), 6 (The Nurturer)",
      6: "1 (The Leader), 5 (The Adventurer), 7 (The Thinker)",
      7: "2 (The Peacemaker), 4 (The Builder), 8 (The Powerhouse)",
      8: "1 (The Leader), 3 (The Creator), 5 (The Adventurer)",
      9: "1 (The Leader), 4 (The Builder), 7 (The Thinker)",
    };

    const reduceSteps = (num: number) => {
      const steps: string[] = [];
      while (num > 9) {
        const digits = num.toString().split("").map(Number);
        const sum = digits.reduce((a, b) => a + b, 0);
        steps.push(`${digits.join(" + ")} = ${sum}`);
        num = sum;
      }
      return num as LifePath;
    };

    const calculateLifePath = () => {
      error.value = "";
      calculationSteps.value = [];
      finalNumber.value = null;

      if (!birthdate.value) {
        error.value = "⚠ Silakan pilih tanggal lahir!";
        return;
      }

      const digits = birthdate.value.replace(/-/g, "").split("").map(Number);
      const total = digits.reduce((a, b) => a + b, 0);
      const firstStep = `${digits.join(" + ")} = ${total}`;
      const final = reduceSteps(total);

      calculationSteps.value = [firstStep];
      let temp = total;
      while (temp > 9) {
        const digitsArr = temp.toString().split("").map(Number);
        const sum = digitsArr.reduce((a, b) => a + b, 0);
        calculationSteps.value.push(`${digitsArr.join(" + ")} = ${sum}`);
        temp = sum;
      }

      finalNumber.value = final;
    };

    return {
      birthdate,
      error,
      calculationSteps,
      finalNumber,
      titles,
      meanings,
      elements,
      planets,
      colors,
      bestDays,
      positiveTraits,
      negativeTraits,
      compatibilityList,
      incompatibilityList,
      calculateLifePath,
    };
  },
});
</script>

<style scoped>
body {
  margin: 0;
}

.glass {
  background: rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(14px);
  border-radius: 22px;
  padding: 32px;
  border: 1px solid rgba(255, 255, 255, 0.25);
}

.result-box {
  background: rgba(255, 255, 255, 0.18);
  padding: 15px;
  border-radius: 14px;
  border: 1px solid rgba(255, 255, 255, 0.25);
}

.fade-in {
  animation: fadeIn 0.6s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>