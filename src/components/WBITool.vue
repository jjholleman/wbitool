<script setup>
import {ref} from "vue";

const wbiValue = ref("")
const wbiVersion = ref("nieuwste")
const wbiType = ref("WECO")
const alphabet = [...Array(26)].map((_, i) => String.fromCharCode('A'.charCodeAt(0) + i));
alphabet.unshift("nieuwste")
const downloaded = ref([])

const formatWbiValue = () => {
  return ('0000000' + wbiValue.value).slice(-7);
}

const submit = (wbiValue, wbiVersion, wbiType) => {
  const realWbiValue = formatWbiValue(wbiValue.value)
  const url = `https://btd-planner.prorail.nl?taak=WBI-${realWbiValue}-${wbiVersion}&doc=${wbiType}`
  downloaded.value.push({
    "wbiValue": realWbiValue,
    "wbiVersion": wbiVersion === "nieuwste" && "*" || wbiVersion,
    "wbiType": wbiType,
    "url": url
  })
  window.open(url, '_blank')
}
</script>

<template>
  <v-container>
    <v-form @submit.prevent="submit(wbiValue, wbiVersion, wbiType)">
      <v-row>
        <v-col>
          <v-text-field
            label="WBI Nummer"
            v-model="wbiValue"
            hide-details
            variant="outlined"
            placeholder="84579"
          ></v-text-field>
        </v-col>
        <v-col>
          <v-select
            label="Versie"
            v-model="wbiVersion"
            :items=alphabet
            hide-details
            variant="outlined"
          >
          </v-select>
        </v-col>
        <v-col>
          <v-btn-toggle
            v-model="wbiType"
            color="primary"
            hide-details
            variant="outlined"
          >
            <v-btn value="WECO">WECO</v-btn>
            <v-btn value="FOT">FOT</v-btn>
          </v-btn-toggle>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="3">
          <v-btn class="mt-2" type="submit" block variant="text" color="primary">Download</v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<style scoped>

</style>
