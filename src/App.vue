<script lang="ts">
export interface IAppState {
  wins: number;
  losses: number;
  draws: number;

  computerChoice: string;
  playerChoice: string;
  verdict: string;
  showToast: boolean;
}

export interface IOutcomes {
  rock: {
    rock: string;
    paper: string;
    scissors: string;
  };
  paper: {
    rock: string;
    paper: string;
    scissors: string;
  };
  scissors: {
    rock: string;
    paper: string;
    scissors: string;
  };
}
</script>

<script setup lang="ts">
import { computed, onMounted, reactive, watch } from "vue";
import SelectButtonVue from "./components/SelectButton.vue";
import ToastNotificationVue from "./components/ToastNotifacation.vue";

const outcomes = <IOutcomes>{
  rock: {
    rock: "draw",
    paper: "loss",
    scissors: "win",
  },
  paper: {
    rock: "win",
    paper: "draw",
    scissors: "loss",
  },
  scissors: {
    rock: "loss",
    paper: "win",
    scissors: "draw",
  },
};

const state = reactive<IAppState>({
  wins: 0,
  losses: 0,
  draws: 0,

  computerChoice: "",
  playerChoice: "paper",
  verdict: "",
  showToast: false,
});

const play = (playerChoice: string) => {
  if (playerChoice === "") {
    state.verdict = "Error: No choice selected";
    return;
  }

  const choices = ["rock", "paper", "scissors"];
  const computerChoice = choices[Math.floor(Math.random() * 3)];
  // const outcome: string = outcomes[playerChoice][computerChoice];

  // if (outcome === "win") {
  //   state.wins++;
  //   state.verdict = "You win!";
  // } else if (outcome === "loss") {
  //   state.losses++;
  //   state.verdict = "You lose!";
  // } else {
  //   state.draws++;
  //   state.verdict = "It's a draw!";
  // }

  playerChoice === computerChoice
    ? (state.draws++, (state.verdict = "It's a draw !"))
    : (playerChoice === "rock" && computerChoice === "paper") ||
      (playerChoice === "paper" && computerChoice === "scissors") ||
      (playerChoice === "scissors" && computerChoice === "rock")
    ? (state.losses++, (state.verdict = "You lose !"))
    : (state.wins++, (state.verdict = "You win !"));
  saveGame();
};

const winPercentage = computed(() => {
  const total = state.wins + state.losses + state.draws;
  return total ? Math.round((state.wins / total) * 100) : 0;
});

const saveGame = () => {
  localStorage.setItem("wins", state.wins.toString());
  localStorage.setItem("losses", state.losses.toString());
  localStorage.setItem("draws", state.draws.toString());
};
const loadGame = () => {
  state.wins = parseInt(localStorage.getItem("wins") || "0");
  state.losses = parseInt(localStorage.getItem("losses") || "0");
  state.draws = parseInt(localStorage.getItem("draws") || "0");
};
const resetGame = () => {
  state.wins = 0;
  state.losses = 0;
  state.draws = 0;
  saveGame();
};

watch(
  () => state.verdict,
  (newVal) => {
    if (newVal !== "") {
      state.showToast = true;
      setTimeout(() => {
        state.showToast = false;
      }, 300);
      setTimeout(() => {
        state.verdict = "";
      }, 301);
    }
  }
);

onMounted(() => {
  loadGame();
  window.addEventListener("keypress", (key) => {
    if (key.key === "r") {
      resetGame();
    }
  });
});
</script>

<template>
  <Transition name="toast">
    <ToastNotificationVue :msg="state.verdict" v-if="state.showToast" />
  </Transition>

  <div
    class="bg-background min-h-screen flex flex-col items-center justify-between text-white p-10"
  >
    <!-- <button @click="play(state.playerChoice)">Play</button> -->
    <section>
      <h1 class="text-6xl font-black select-none">Rock, Paper, Scissors</h1>
    </section>
    <section class="flex">
      <div
        v-for="item in ['rock', 'paper', 'scissors']"
        :key="item"
        class="flex p-3"
      >
        <SelectButtonVue
          :choice="item"
          @click="state.verdict === '' ? play(item) : ''"
        />
      </div>
    </section>
    <section>
      <div class="select-none flex gap-3">
        <h1>Win: {{ state.wins }}</h1>
        <h1>Draws: {{ state.draws }}</h1>
        <h1>Lose: {{ state.losses }}</h1>
      </div>
      <h1>Your Win Percentage: %{{ winPercentage }}</h1>
    </section>
  </div>
</template>

<style>
/* enter transitions */
.toast-enter-from {
  opacity: 0;
  transform: translateY(-60px);
}
/* .toast-enter-to {
    opacity: 1;
    transform: translateY(0);
  } */
.toast-enter-active {
  transition: all 0.3s ease;
}
/* leave transitions */
/* .toast-leave-from {
    opacity: 1;
    transform: translateY(0);
  } */
.toast-leave-to {
  opacity: 0;
  transform: translateY(-60px);
}
.toast-leave-active {
  transition: all 0.3s ease;
}
</style>
