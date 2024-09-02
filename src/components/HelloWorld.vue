<script>
export default {
  data() {
    return {
      userName: "",
      selectedWork: "Wähle die Arbeit aus",
      whichMaintanance: "Welche Wartung",
      displayWartungPopUp: "none",
      inputName: false,
      inputWork: false,
      inputMaintanance: false,
      allInputsFilled: false,
      quantity: "",
      quantityOfOrders: "",
      quantityOfVinyls: "",
      inputQuantity: false,
      inputQuantityOrders: false,
      inputQuantityVinyls: false,
      theSeconds: "00",
      theMinutes: "00",
      theHours: "00",
      playActive: false,
      haltActive: true,
      stopActive: true,
      sendActive: true,
      intervallRunning: null,
      wasItStopped: false,
      validQuantities: false,
    };
  },
  computed: {
    showInputStueckzahl() {
      if (
        this.selectedWork === "Schallplatten waschen" ||
        this.selectedWork === "Eingabe Discogs" ||
        this.selectedWork === "Wartung Maschinen"
      ) {
        return true;
      }
    },
  },
  methods: {
    checkRequirements() {
      if (this.userName === "") {
        this.inputName = true;
        this.allInputsFilled = false;
        this.sendActive = true;
      } else {
        if (this.selectedWork === "Wähle die Arbeit aus") {
          this.inputWork = true;
          this.allInputsFilled = false;
        } else if (this.selectedWork === "Wartung Maschinen") {
          if (this.whichMaintanance === "Welche Wartung") {
            this.inputMaintanance = true;
            this.allInputsFilled = false;
          } else {
            this.allInputsFilled = true;
          }
        } else {
          this.allInputsFilled = true;
        }
      }
    },
    changeButtonStatus(whichButton) {
      if (this.allInputsFilled === true) {
        if (whichButton === "playButton") {
          this.playActive = true;
          this.haltActive = false;
          this.stopActive = false;
        } else if (whichButton === "haltButton") {
          this.playActive = false;
          this.haltActive = true;
          this.stopActive = false;
        } else if (whichButton === "stopButton") {
          this.playActive = false;
          this.haltActive = true;
          this.stopActive = true;
        }
      }
    },
    increaseTheTime() {
      if (this.theSeconds === "59") {
        this.theSeconds = "00";
        if (this.theMinutes === "59") {
          this.theMinutes = "00";
          let changedHours = Number(this.theHours);
          changedHours++;
          this.theHours = "" + changedHours;
          if (this.theHours.length === 1) {
            this.theHours = "0" + this.theHours;
          }
        } else {
          let changedMinutes = Number(this.theMinutes);
          changedMinutes++;
          this.theMinutes = "" + changedMinutes;
          if (this.theMinutes.length === 1) {
            this.theMinutes = "0" + this.theMinutes;
          }
        }
      } else {
        let changedSeconds = Number(this.theSeconds);
        changedSeconds++;
        this.theSeconds = "" + changedSeconds;
        if (this.theSeconds.length === 1) {
          this.theSeconds = "0" + this.theSeconds;
        }
      }
    },
    startFunction() {
      this.wasItStopped = false;
      if (this.allInputsFilled === true) {
        this.intervallRunning = setInterval(this.increaseTheTime, 1000);
      }
    },
    haltStopFunction(whatWasClicked) {
      clearInterval(this.intervallRunning);
      if (whatWasClicked === "halt") {
        this.wasItStopped = false;
      } else {
        this.wasItStopped = true;
      }
    },
    validateAchievedNumbers() {
      const regex = /^\d+$/;
      if (this.selectedWork !== "Bestellungen bearbeiten") {
        if (this.quantity === "") {
          this.sendActive = true;
          this.validQuantities = false;
          this.inputQuantity = true;
        } else if (
          regex.test(this.quantity) === false &&
          this.quantity !== ""
        ) {
          alert("Bitte gebe nur Zahlen/Ziffern für die Stückzahl ein!!!");
          this.inputQuantity = true;
        } else if (
          regex.test(this.quantity) === true &&
          this.quantity !== "" &&
          this.allInputsFilled === true
        ) {
          this.validQuantities = true;
        }
      } else {
        if (this.quantityOfOrders === "") {
          this.sendActive = true;
          this.validQuantities = false;
          this.inputQuantityOrders = true;
        } else if (
          regex.test(this.quantityOfOrders) === false &&
          this.quantityOfOrders !== ""
        ) {
          alert("Bitte gebe nur Zahlen/Ziffern für die Stückzahl ein!!!");
          this.inputQuantityOrders = true;
          this.quantityOfOrders = "";
        }
        if (this.quantityOfVinyls === "") {
          this.sendActive = true;
          this.validQuantities = false;
          this.inputQuantityVinyls = true;
        } else if (
          regex.test(this.quantityOfVinyls) === false &&
          this.quantityOfVinyls !== ""
        ) {
          alert("Bitte gebe nur Zahlen/Ziffern für die Stückzahl ein!!!");
          this.inputQuantityVinyls = true;
          this.quantityOfVinyls = "";
        }
        if (
          regex.test(this.quantityOfOrders) === true &&
          this.quantityOfOrders !== "" &&
          regex.test(this.quantityOfVinyls) === true &&
          this.quantityOfVinyls !== "" &&
          this.allInputsFilled === true
        ) {
          this.validQuantities = true;
        }
      }
    },
    activateSendButton() {
      if (this.validQuantities === true && this.allInputsFilled === true) {
        this.sendActive = false;
      } else {
        alert("Bitte überprüfe nochmal alle Eingaben!");
      }
    },
  },
  props: {
    msg: String,
  },
};
</script>

<template>
  <div class="whole-area">
    <h1>{{ msg }}</h1>
    <div class="input-area">
      <div class="before-start-area">
        <input
          type="text"
          placeholder="Benutzername"
          v-model="userName"
          :class="{ 'input-alert': inputName }"
          @click="inputName = false"
          @keyup="
            if (wasItStopped === true) {
              checkRequirements();
            }
          "
        />
        <select
          v-model="selectedWork"
          :class="{ 'input-alert': inputWork }"
          @click="inputWork = false"
        >
          <option disabled selected>Wähle die Arbeit aus</option>
          <option value="Schallplatten waschen">Schallplatten waschen</option>
          <option value="Eingabe Discogs">Eingabe Discogs</option>
          <option value="Bestellungen bearbeiten">
            Bestellungen bearbeiten
          </option>
          <option value="Wartung Maschinen">Wartung Maschinen</option>
          <option value="Lager arbeiten">Lager arbeiten</option>
          <option value="Messe Vorbereitung">Messe Vorbereitung</option>
        </select>
        <select
          class="wartungSelection"
          v-show="selectedWork === 'Wartung Maschinen'"
          v-model="whichMaintanance"
          :class="{ 'input-alert': inputMaintanance }"
          @click="inputMaintanance = false"
        >
          <option disabled selected>Welche Wartung</option>
          <option value="Wartung 1">Wartung 1</option>
          <option value="Wartung 2">Wartung 2</option>
          <option value="Wartung 3">Wartung 3</option>
        </select>
        <div class="wartung-container">
          <!-- :style="{ display: displayWartungPopUp }" -->
          <h2>Wartung 1</h2>
          <p>jvjdfgbhncv,bjflijdflkglgfkjhlhlgljholgjhjkljgfhn</p>
          <h2>Wartung 2</h2>
          <p>dkjvdfjbdfbvgfjbhlgfjhlnjgflnjlgkmnjgljmnblkgjnbkl</p>
          <h2>Wartung 3</h2>
          <p>dkvdjchnbvklfjbgklifjbgljcgflbfjgclbjcfklbjcljblcjb</p>
        </div>
      </div>
      <div class="after-start-area">
        <input
          type="text"
          placeholder="Stückzahl"
          v-model="quantity"
          v-show="showInputStueckzahl"
          :disabled="!wasItStopped"
          :class="{ 'input-alert': inputQuantity }"
          @click="inputQuantity = false"
          @blur="validateAchievedNumbers()"
        />
        <input
          type="text"
          placeholder="Anzahl Bestellungen"
          v-model="quantityOfOrders"
          v-show="selectedWork === 'Bestellungen bearbeiten'"
          :disabled="!wasItStopped"
          :class="{ 'input-alert': inputQuantityOrders }"
          @click="inputQuantityOrders = false"
          @blur="validateAchievedNumbers()"
        />
        <input
          type="text"
          placeholder="Anzahl Platten"
          v-model="quantityOfVinyls"
          v-show="selectedWork === 'Bestellungen bearbeiten'"
          :disabled="!wasItStopped"
          :class="{ 'input-alert': inputQuantityVinyls }"
          @click="inputQuantityVinyls = false"
          @blur="validateAchievedNumbers()"
        />
        <div class="explanationForSendenButton">
          Wenn alles richtig eingegeben ist, <br />
          klicke bitte auf den grünen Kreis <br />
          um den Senden-Button zu aktivieren!
        </div>
        <div class="green-circle" @click="activateSendButton()"></div>
      </div>
    </div>
    <div class="button-area">
      <button
        type="button"
        :disabled="playActive"
        @click="
          checkRequirements(), changeButtonStatus('playButton'), startFunction()
        "
      >
        ▶
      </button>
      <button
        type="button"
        :disabled="haltActive"
        @click="
          checkRequirements(),
            changeButtonStatus('haltButton'),
            haltStopFunction('halt')
        "
      >
        ||
      </button>
      <button
        type="button"
        :disabled="stopActive"
        @click="
          checkRequirements(),
            changeButtonStatus('stopButton'),
            haltStopFunction('stop')
        "
      >
        ■
      </button>
      <button type="button" :disabled="sendActive">Senden</button>
    </div>
    <div class="time-area">
      <p>
        <span>{{ theHours }}</span
        >:<span>{{ theMinutes }}</span
        >:<span>{{ theSeconds }}</span>
      </p>
    </div>
  </div>
</template>

<style scoped>
.input-area,
.button-area {
  display: flex;
  gap: 3rem;
  font-size: 1.5rem;
  margin-bottom: 2rem;
}

.before-start-area,
.after-start-area {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  position: relative;
}

.after-start-area {
  align-items: center;
}

.wartungSelection:hover + .wartung-container {
  display: block;
}

.wartung-container {
  border: 0.1rem solid black;
  border-radius: 2rem;
  padding: 0.5rem;
  position: absolute;
  left: -24vw;
  bottom: -1rem;
  background-color: rgb(255, 255, 0);
  display: none;
  color: black;
}

.wartung-container p {
  font-size: 1rem;
  margin-top: 0;
}

.input-alert {
  border: red 0.25rem dotted;
}

h2 {
  margin-block: 0;
}

h2 {
  font-size: 1.5rem;
}

button {
  border: 0.15rem solid black;
}

.time-area {
  font-size: 3rem;
}

.explanationForSendenButton {
  margin-top: 1rem;
  font-weight: 800;
  color: red;
  font-size: 1rem;
}

.green-circle {
  width: 3rem;
  height: 3rem;
  background-color: green;
  border-radius: 50%;
  margin-top: 1.5rem;
}
</style>
