<template>
  <div id="calculator_container">
    <h2 id="title" >Schaltungsrechner</h2>

    <div id="resistors" v-for="(resistance, index) in resistances" :key="index">
      <label id="resistor" :for="'resistance_' + index">Widerstand {{ index + 1 }}:</label>
      <input
        :id="'resistance_' + index"
        v-model="resistance.value"
        type="number"
        placeholder="Widerstandswert"
        @input="handleInput(index)"
      />
      
      <button id="deleteResistanceButton" @click="deleteResistor(index)">Delete</button>
    </div>

    <div>
      <button id="addResistanceButton" @click="addResistance">Widerstand hinzufügen</button>
    </div>

    <div id="circuit_type">
      <label for="connectionType">Schaltungstyp:</label>
      <select id="circuit_selector" v-model="connectionType">
        <option value="series">Serienschaltung</option>
        <option value="parallel">Parallelschaltung</option>
      </select>
    </div>

    <div>
      <button id="calculate_button" @click="calculate">Berechnen</button>
      <p id="calculation_display" v-if="showOhmSymbol">Ergebnis: {{ result }} Ω</p>
      <p id="calculation_display" v-else>Ergebnis: {{ result }}</p>
    </div>
  </div>
</template>

<style scoped>

#title {
  margin: 30px;
  margin-left: 10px;
  margin-bottom: 20px;
  font-size: 3em;
}

#calculator_container {
  max-width: 400px;
  font-family: Arial, sans-serif;
}

#resistors {
  margin: 5px;
}

#resistor {
  margin: 10px;
}

#deleteResistanceButton {
  margin-left: 10px;
  cursor: pointer;
  background-color: #ff6666;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
}

#addResistanceButton {
  margin-left: 10px;
  padding: 5px;
}

#circuit_type {
  margin: 10px;
  margin-top: 25px;
}

#circuit_selector {
  margin-left: 5px;
}

#calculate_button {
  margin-left: 10px;
  margin-top: 10px;
  padding: 10px;
  cursor: pointer;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
}

#calculate_button:hover {
  background-color: #45a049;
  color: white;
}

#calculation_display {
  padding: 10px;
  font-size: 1.5em
}

</style>

<script>
export default {
  data() {
    return {
      resistances: [{ value: 0 }],
      connectionType: 'series',
      result: null,
      showOhmSymbol: false,
    };
  },
  methods: {
    addResistance() {
      this.resistances.push({ value: 0 });
    },
    deleteResistor(index) {
      this.resistances.splice(index, 1);
    },
    calculate() {
      if (this.connectionType === 'series') {
        this.result = this.resistances.reduce(
          (total, resistance) => total + resistance.value,
          0
        );
      } else {
        this.result =
          1 /
          this.resistances.reduce(
            (inverseTotal, resistance) =>
              inverseTotal + 1 / resistance.value,
            0
          );
      }
      
      this.showOhmSymbol = true;
    },
    handleInput(index) {
      if (this.resistances[index].value < 0) {
        this.resistances[index].value = 0;
      }
    },
  },
};
</script>
