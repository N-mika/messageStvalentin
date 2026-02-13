<template>
  <div class="relative overflow-hidden min-h-screen">

    <!-- Coeurs animés -->
    <span v-for="i in 15" :key="i" class="heart relative z-11" :style="{ left: Math.random() * 100 + '%' }">❤️</span>

    <div class="flex items-center justify-center min-h-screen px-4 relative z-10">

      <div class="bg-white rounded-2xl shadow-xl p-6 max-w-sm w-full text-center">

        <h1 class="text-2xl font-bold text-pink-600 mb-4">💝 Cadeau virtuel</h1>

        <!-- Génération -->
        <div v-if="!receiverName">
          <div v-if="!receiverName">

            <p class="text-gray-600 text-sm mb-4">
              Crée un petit cadeau virtuel rempli d’amour 💖
              Entre ton prénom, génère un lien, et partage un message spécial.
            </p>
          </div>
          <input v-model="nameInput" placeholder="Entre ton prénom" class="w-full border rounded-lg px-3 py-2 mb-3" />

          <button @click="generateLink" class="w-full bg-pink-500 text-white py-2 rounded-lg">
            Créer le cadeau
          </button>

          <div v-if="shareLink" class="mt-4">
            <input :value="shareLink" readonly class="w-full border rounded px-2 py-1 text-sm mb-2" />

            <button @click="copyLink" class="w-full bg-rose-400 text-white py-2 rounded">
              Copier le lien
            </button>
          </div>

        </div>

        <!-- Lecture -->
        <div v-else>

          <!-- <img src="/gift.jpg" class="w-32 mx-auto mb-4 animate-bounce" /> -->
          <div class="flex justify-center my-6">
            <div class="heart-shape animate-pulse"></div>
          </div>

          <h2 class="text-lg font-semibold mb-2">
            Message de {{ receiverName }} pour toi
          </h2>

          <p class="text-pink-600 text-xl font-medium my-4">
            {{ randomMessage }}
          </p>

          <button @click="reset" class="bg-pink-500 text-white px-4 py-2 rounded">
            Créer mon propre cadeau
          </button>

        </div>

      </div>

    </div>
    <footer class="absolute bottom-0 w-full text-center py-3 text-black text-sm opacity-80">
      <p>
        Created by <span class="font-semibold">Mika Tieko</span> · Built with VueJs ❤️
      </p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { messagesData } from './message'

const messages: string[] = messagesData

const nameInput = ref("")
const shareLink = ref("")
const receiverName = ref<string | null>(null)
const randomMessage = ref<string | undefined>("")
const audio = ref<HTMLAudioElement | null>(null)

function generateLink() {
  if (!nameInput.value) return
  shareLink.value =
    `${window.location.origin}?name=${encodeURIComponent(nameInput.value)}`
}

function copyLink() {
  navigator.clipboard.writeText(shareLink.value)
  alert("Lien copié 💖")
}

function reset() {
  window.location.href = window.location.origin
}

onMounted(() => {
  const params = new URLSearchParams(window.location.search)
  const name = params.get("name")

  if (name) {
    receiverName.value = name
    randomMessage.value =
      messages[Math.floor(Math.random() * messages.length)]

    setTimeout(() => {
      audio.value?.play()
    }, 500)
  }
})
</script>

<style scoped>
.heart {
  position: absolute;
  top: -20px;
  animation: fall 8s linear infinite;
  font-size: 20px;
  opacity: 0.7;
}

@keyframes fall {
  0% {
    transform: translateY(-10px);
  }

  100% {
    transform: translateY(110vh);
  }
}

.heart-shape {
  width: 80px;
  height: 80px;
  background: #ec4899;
  position: relative;
  transform: rotate(-45deg);
}

.heart-shape::before,
.heart-shape::after {
  content: "";
  width: 80px;
  height: 80px;
  background: #ec4899;
  border-radius: 50%;
  position: absolute;
}

.heart-shape::before {
  top: -40px;
  left: 0;
}

.heart-shape::after {
  left: 40px;
  top: 0;
}

.heart-float {
  animation: float 3s ease-in-out infinite;
}

@keyframes float {

  0%,
  100% {
    transform: rotate(-45deg) translateY(0);
  }

  50% {
    transform: rotate(-45deg) translateY(-15px);
  }
}
</style>
