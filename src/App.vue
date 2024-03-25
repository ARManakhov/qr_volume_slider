<template>
  <v-app>
    <v-main>
      <v-row>
        <v-col cols=2></v-col>
        <v-col cols="6">
          <v-slider
            v-model="slider"
            prepend-icon="mdi-volume-high"
            :max="100"
            :min="0"
            class="align-center"
            hide-details
            readonly>
            <template v-slot:append>
              <v-text-field
                v-model="slider"
                density="compact"
                style="width: 70px"
                type="number"
                hide-details
                single-line
                readonly></v-text-field>
            </template>
          </v-slider>
        </v-col>
        <v-col cols=2></v-col>
        <v-col cols=2></v-col>
      </v-row>
      <v-row>
        <v-col cols=2></v-col>
        <v-col cols="3">
          <v-img v-bind:src='prev' ></v-img>
          <v-card-title class="text-h6">
            decrease volume
          </v-card-title>
        </v-col>
        <v-col cols="3">
          <v-img v-bind:src='next'></v-img>
          <v-card-title class="text-h6">
            increase volume
          </v-card-title>
        </v-col>
        <v-col cols=2></v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import {defineComponent} from "vue";
import * as QRCode from 'qrcode'

export default defineComponent({
  data() {
    return {
      slider: undefined as number,
      next: undefined as string,
      prev: undefined as string,
    }
  },
  created() {
    if (window.location.hash) {
      this.slider = Number(window.location.hash.substring(1))
    } else {
      this.slider = 50
    }
    const nextLocation = new URL(window.location);
    nextLocation.hash = String(this.slider + 1)
    QRCode.toDataURL(nextLocation.toString())
      .then((u: string) => this.next = u)

    const prevLocation = new URL(window.location);
    prevLocation.hash = String((this.slider - 1))
    QRCode.toDataURL(prevLocation.toString())
      .then((u: string) => this.prev = u)
  }
})
</script>


