<template>
  <div>
    <label for="genotype">Select ADCY9 genotype:</label>
    <select id="genotype" v-model="genotype" v-on:change="change">
      <option value="" disabled selected>Select genotype</option>
      <option value="AA">AA</option>
      <option value="GA">GA</option>
      <option value="GG">GG</option>
    </select>

    <div v-bind:class="'result ' + (error ? 'error' : '')">{{ result }}</div>
  </div>
</template>

<script>

  import axios from 'axios';

export default {
  name: 'SelectGenotype',
  data () {
    return {
      genotype: null,
      result: null,
      error: false
    }
  },
  methods: {
    change() {
      this.result = "Please wait...";
      axios.post("https://activator.kgrid.org/99999/fk4n99hh99/v0.0.1/rs1967309interpret", {
        'twoBases': this.genotype
      }).then((resp) => {
        this.result = resp.data.result;
        // todo: better check. current knowledge object only returns a string
        if(resp.data.result.includes("Prescribe as needed")) {
          this.error = false;
        } else {
          this.error = true;
        }
      }).catch((error) => {
        this.result = "Error fetching result: " + error;
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .result {
    font-weight: bold;
  }

  .error {
    color: red;
  }
</style>
