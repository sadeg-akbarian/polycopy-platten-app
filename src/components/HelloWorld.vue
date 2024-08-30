<script>
export default {
  data() {
    return {
      userName: "",
      selectedWork: "Wähle die Arbeit aus",
      whichMaintanance: "Welche Wartung",
      displayWartungPopUp: "none",
      inputUser: false,
      inputWork: false,
      inputMaintanance: false,
      allInputsFilled: false,
      quantity: "",
      quantityOfOrders: "",
      quantityOfVinyls: "",
      theSeconds: "00",
      theMinutes: "00",
      theHours: "00",
      playActive: false,
      haltActive: true,
      stopActive: true,
      sendActive: true,
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
        this.inputUser = true;
        this.allInputsFilled = false;
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
          :class="{ 'input-alert': inputUser }"
          @click="inputUser = false"
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
        />
        <input
          type="text"
          placeholder="Anzahl Bestellungen"
          v-model="quantityOfOrders"
          v-show="selectedWork === 'Bestellungen bearbeiten'"
        />
        <input
          type="text"
          placeholder="Anzahl Platten"
          v-model="quantityOfVinyls"
          v-show="selectedWork === 'Bestellungen bearbeiten'"
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
      <button type="button" :disabled="playActive" @click="checkRequirements()">
        ▶
      </button>
      <button type="button" :disabled="haltActive">||</button>
      <button type="button" :disabled="stopActive">■</button>
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
