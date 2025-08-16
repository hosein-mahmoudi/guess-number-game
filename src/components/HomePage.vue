<template >
  <div class="body">
    <div dir="rtl" class="game-container">
      <div class="icon-wrap">🎯</div>
      <div class="game-title">بازی حدس عدد</div>
      <div class="game-subtitle">
        یک عدد بین <strong>۱ تا ۱۰۰</strong> حدس بزن
      </div>

      <input
        type="number"
        @focus="clearInput"
        class="guess-input mb-3 w-100"
        v-model="userNumber"
        placeholder="یک عدد وارد کنید مثلا 25
"
        min="1"
        max="100"
        :disabled="gameOver"
      />

      <button
        @click="checkNumber"
        class="btn btn-warning btn-main w-100 mb-2"
        :disabled="gameOver"
      >
        🚀 ارسال حدس
      </button>

      <div v-if="attempts > 0" class="mt-2">تعداد حدس‌ها: {{ attempts }}</div>
      <div v-if="message" class="message-box" :class="{
          'win-message': isWin,
          'lose-message': isLose
        }">{{ message }}</div>

      <button @click="resetGame" class="btn btn-outline-light w-100 reset-btn">
        بازی جدید
      </button>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import confetti from "canvas-confetti";

export default {
  setup() {
    const userNumber = ref(null);
    let message = ref("");
    let attempts = ref(0);
    const gameOver = ref(false);
    const isWin = ref(false);
    const isLose = ref(false);
    const random = Math.floor(Math.random() * 100) + 1;

    message.value = null;
    function checkNumber() {
      if (attempts.value < 5) {
        if (
          userNumber.value === null ||
          userNumber.value < 1 ||
          userNumber.value > 100
        ) {
          message.value = "لطفا عددی بین 1 تا 100 وارد کنید.";
          return;
        }
        if (userNumber.value == random) {
          message.value = "تبریک عدد مورد نظر را درست وارد کردید.";
           isWin.value = true;
          runConfetti();
          gameOver.value = true;
        } else if (userNumber.value < random) {
          message.value = "عددی بزرگتر وارد کنید";
        } else {
          message.value = "عددی کوچکتر وارد کنید";
        }

        attempts.value++;
      }
      if (attempts.value === 5 && userNumber.value !== random) {
        message.value = "فرصت شما تمام شد، بازی جدید را شروع کنید.";
         isLose.value = true;
        gameOver.value = true;
      }
    }
    //خالی کردن ورودی
    function clearInput() {
      userNumber.value = null;
    }
    //بازی جدید
    function resetGame() {
      window.location.reload();
    }
    //  انیمنشن برای برنده شدن کاربر
    function runConfetti() {
      confetti({
        particleCount: 150,
        spread: 70,
        origin: { y: 0.6 },
      });
    }
    return {
      checkNumber,
      message,
      userNumber,
      resetGame,
      attempts,
      clearInput,
      gameOver,isWin, isLose
    };
  },
};
</script>

<style >
@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-5px); }
  40%, 80% { transform: translateX(5px); }
}



.win-message {
  background-color: #4caf50;
  color: white;
  animation: pulse 0.8s infinite;
  padding: 8px;
  border-radius: 8px;
}

.lose-message {
  background-color: #e53935;
  color: white;
  animation: shake 0.5s ease-in-out;
   padding: 8px;
  border-radius: 8px;
  
}
</style>
