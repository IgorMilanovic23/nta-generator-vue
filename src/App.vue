<script setup>
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
import { ref } from 'vue'
const decreeDate = ref("")
const writtenExamArrangements = ref(false)
const writtenExamExtraTime = ref(0)
const writtenExamSeparateRoomMulti = ref(false)
const writtenExamSeparateRoomSingle = ref(false)
const writtenExamSeatChoice = ref(false)
const writtenExamHearingProtection = ref(false)
const writtenExamNoGrammar = ref(false)
const writtenExamOther = ref("")
const oralExamArrangements = ref(false)
const oralExamExtraTime = ref(0)
const oralExamExtraPreparationTime = ref("")
const oralExamOther = ref("")
const generalAgreements = ref("")

function sleep(miliseconds) {
   var currentTime = new Date().getTime();

   while (currentTime + miliseconds >= new Date().getTime()) {
   }
}

function copyToClipboard() {
  var textFinal =
  `Decree - Reasonable Adjustments due to a Disability (${decreeDate.value})
  ============================================================\n\n`
  if(writtenExamArrangements.value == true) {
    textFinal = textFinal + (`> For written examinations [$SP]:\n`)
    if(writtenExamExtraTime.value) {
      textFinal = textFinal + `--- Student will receive ${writtenExamExtraTime.value}% extra time.\n`
    }
    if(writtenExamSeparateRoomMulti.value) {
      textFinal = textFinal + `--- Student may take the exam in a separate room with a maximum of 10 other students\n`
    }
    if(writtenExamSeparateRoomSingle.value) {
      textFinal = textFinal + `--- Student may take the exam in a separate room alone\n`
    }
    if(writtenExamSeatChoice.value) {
      textFinal = textFinal + `--- Student has free seating choice in the examination room\n`
    }
    if(writtenExamHearingProtection.value) {
      textFinal = textFinal + `--- Student may use hearing protection (no communication capabilities)\n`
    }
    if(writtenExamNoGrammar.value) {
      textFinal = textFinal + `--- Spelling and grammar mistakes are not taken into account during grading \n`
    }
    if(writtenExamOther.value) {
      textFinal = textFinal + `--- ${writtenExamOther.value}\n`
    }
  }

  if(oralExamArrangements.value == true) {
    textFinal = textFinal + (`\n> For oral examinations [$MP]:\n`)
    if(oralExamExtraTime.value) {
      textFinal = textFinal + `--- Student will receive ${oralExamExtraTime.value}% extra time.\n`
    }
    if(oralExamExtraPreparationTime.value) {
      textFinal = textFinal + `--- Student will receive ${oralExamExtraPreparationTime.value}% extra time during exam preparation.\n`
    }
    if(oralExamOther.value) {
      textFinal = textFinal + `--- ${writtenExamOther.value}\n`
    }
  }

  if(generalAgreements.value) {
    textFinal = textFinal + `--- ${writtenExamOther.value}\n`
  }

  navigator.clipboard.writeText(textFinal).then(() => {
    alert("In Zwischenablage kopiert!");
  }).catch(err => {
    console.error('Fehler beim Kopieren in die Zwischenablage: ', err);
    alert("Fehler beim Kopieren in die Zwischenablage.");
  });

  window.location.reload()
}

</script>


<template>
  <div class="container">
    <div class="row">
      <h1 class="text-center" id="title">Erfassungstool Nachteilsausgleiche</h1>
    </div>
    <div class="row">
      <form @submit.prevent="onSubmit" id="mainForm">
        <div class="row" id="decreeDateSection">
          <div class="col">
            <label for="decreeDatePicker" class="form-label">Datum letzte Verfügung</label>
            <input type="date" class="form-control" id="decreeDatePicker" v-model="decreeDate">
          </div>
        </div>
        <div class="row" id="examTypes">
          <div class="col">
            <div class="row">
              <h3>Schriftliche Prüfungen</h3>
            </div>
            <div class="row">
            <label for="writtenExamArrangements" class="form-check-label">
              <input type="checkbox" id="writtenExamArrangements" class="form-check-input" v-model="writtenExamArrangements">
              Hat schriftl. Anpassungen?
            </label>
            </div>
            <div class="row">
              <label for="writtenExamExtraTime">Zusatzzeit (in %)</label>
              <input type="number" class="form-control" id="writtenExamExtraTime" min="0" max="200" v-model="writtenExamExtraTime" :disabled=!writtenExamArrangements>
            </div>
            <div class="row">
              <label for="writtenExamSeparateRoomMulti" class="form-check-label">
                <input type="checkbox" id="writtenExamSeparateRoomMulti" class="form-check-input" v-model="writtenExamSeparateRoomMulti" :disabled=!writtenExamArrangements>
                Separater Raum (max. 10 Studis)
              </label>
            </div>
            <div class="row">
              <label for="writtenExamSeparateRoomSingle" class="form-check-label">
                <input type="checkbox" id="writtenExamSeparateRoomSingle" class="form-check-input" v-model="writtenExamSeparateRoomSingle" :disabled=!writtenExamArrangements>
                Separater Raum (alleine)
              </label>
            </div>
            <div class="row">
              <label for="writtenExamSeatChoice" class="form-check-label">
                <input type="checkbox" id="writtenExamSeatChoice" class="form-check-input" v-model="writtenExamSeatChoice" :disabled=!writtenExamArrangements>
                Freie Sitzplatzwahl
              </label>
            </div>
            <div class="row">
              <label for="writtenExamHearingProtection" class="form-check-label">
                <input type="checkbox" id="writtenExamHearingProtection" class="form-check-input" v-model="writtenExamHearingProtection" :disabled=!writtenExamArrangements>
                Hörschutz (nicht komm.-fähig)
              </label>
            </div>
            <div class="row">
              <label for="writtenExamNoGrammar" class="form-check-label">
                <input type="checkbox" id="writtenExamNoGrammar" class="form-check-input" v-model="writtenExamNoGrammar" :disabled=!writtenExamArrangements>
                Nichtbeachtung Rechtschreibe-/Grammatikfehler
              </label>
            </div>
            <div class="row">
              <label for="writtenExamOther">Weitere Vereinbarungen</label>
              <textarea class="form-control" id="writtenExamOther" v-model="writtenExamOther" :disabled=!writtenExamArrangements></textarea>
            </div>
          </div>
          <div class="col">
            <div class="row">
              <h3>Mündliche Prüfungen</h3>
            </div>
            <div class="row">
              <label for="oralExamArrangements" class="form-check-label">
                <input type="checkbox" id="oralExamArrangements" class="form-check-input" v-model="oralExamArrangements">
                Hat mündl. Anpassungen?
              </label>
            </div>
            <div class="row">
              <label for="oralExamExtraTime">Zusatzzeit (in %)</label>
              <input type="number" class="form-control" id="oralExamExtraTime" min="0" max="200" v-model="oralExamExtraTime" :disabled=!oralExamArrangements>
            </div>
            <div class="row">
              <label for="oralExamExtraPreparationTime">Zusatzzeit Vorbereitung (in %)</label>
            </div>
            <div class="row">
              <input type="number" class="form-control" id="oralExamExtraPreparationTime" min="0" max="200" v-model="oralExamExtraPreparationTime" :disabled=!oralExamArrangements>
            </div>
            <div class="row">  
              <label for="oralExamOther">Weitere Vereinbarungen</label>
              <textarea class="form-control" id="oralExamOther" v-model="oralExamOther" :disabled=!oralExamArrangements></textarea>
            </div>
          </div>
        </div>
        <div class="row" id="generalAgreementsSection">
          <div class="col">
            <h2>Generelle Vereinbarungen</h2>
            <textarea class="form-control" id="generalAgreements" rows="4"
              placeholder="Hier können generelle Vereinbarungen eingetragen werden..." v-model="generalAgreements"></textarea>
            <button @click="copyToClipboard" class="btn btn-primary" id="saveButton">In Zwischenablage kopieren</button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
  input[type=textarea] {
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    box-sizing: border-box;
  }
</style>