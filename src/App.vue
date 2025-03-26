<script setup>
import { ref, computed } from 'vue'
import quiz from './quiz.json'

//Sert comme index dynamique pour l'affichage des questions et des propositions dans le tableau grâce à la boucle v-for
const compteur = ref(0)

//Pour récupérer dynamiquement l'input séléctionné par l'utilisateur
const propositionChoisie = ref("")

//Score (ref car celui-ci bouge dynamiquement)
const score = ref(0)

//Message de fin
const messageDeFin = computed(() => score.value >= 3 ? quiz.message_succes : quiz.message_failure)

const isQuizzFinished = computed(() => compteur.value + 1 === quiz.questions.length)

// Ici je vérifie si la proposition choisie est égale a la bonne réponse, si celle-ci est égale j'ajoute 1 au score sinon je fais rien
const verifBonneReponse = () => {
  if (quiz.questions[compteur.value].bonne_reponse === propositionChoisie.value) {
    score.value += 1;
  }
  //ici je vérifie SI le nombre de qst est suppérieur au compteur (nombre de fois où l'utilisateur à valider), si il est suppérieur alors j'ajoute 1 au compteur (je passe à la prochaine question) et je rénitialise la proposition choisie SINON cela veut dire qu'on est arrivé à la fin du quiz. Une fois arriver à la fin du quiz, SI le score est suppérieur ou égale à 3 alors j'affiche le message de succès SINON j'affiche le message de failure
  if (compteur.value <= quiz.questions.length - 1) {
    compteur.value += 1;
    propositionChoisie.value = "";
  }
};
</script>

<template>
  <div class="titre">
    <h1>{{ quiz.titre }}</h1>
  </div>
    <template v-if="!isQuizzFinished">
      <div class="quiz">
        <div class="quiz-card">
          <div class="question-proposition">
            <h2>{{ quiz.questions[compteur].question }}</h2>
            <div class="propositions" v-for="propositions in quiz.questions[compteur].propositions" :key="propositions">
              <input type="radio" :value="propositions" :id="propositions" v-model="propositionChoisie" name="quiz-options">
              <p>{{ propositions }}</p>
            </div>
          </div>
          <div class="button">
            <button @click="verifBonneReponse">Validez</button>
          </div>
        </div>
      </div>
    </template>
    <div class="msgDeFin">
      <h2 v-if="isQuizzFinished">{{ messageDeFin }}</h2>
    </div>
</template>

<style scoped>

.titre{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 24px 12px;
}

.quiz-card {
  width: 100%;
  height: 400px;
  box-shadow: 0px 0px 30px rgba(0, 0, 0, 0.5);
  border-radius: 8px;
}

.question-proposition {
  display: flex;
  flex-direction: column;
  width: auto;
  padding: 8px 16px;
} 

button {
  padding: 8px 12px;
  width: 100%;
  background-color: black;
  color: white;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.5);
  border-radius: 6px;
  border: none;
}

.button {
  padding: 0px 26px;
}

.propositions {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 8px;
}

.msgDeFin {
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (min-width: 768px) {
  .quiz-card {
  width: 600px;
  height: 375px;
}

.question-proposition {
  padding: 12px 50px;
} 

.quiz {
  display: flex;
  align-items: center;
  justify-content: center;
}

.button {
  display: flex;
  justify-content: flex-end;
}

button {
  width: fit-content;
}
}
</style>
