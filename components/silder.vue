<template>
  <v-carousel cycle height="500" :show-arrows="false" hide-delimiters>
    <v-carousel-item v-for="(img, i) in imgs" :key="i">
      <v-sheet height="100%">
        <v-img :src="img" class="fill-height">
          <v-overlay absolute z-index="-1" opacity=".8"></v-overlay>

          <v-container class="fill-height">
            <v-row align="center">
              <v-col md="6" style="background-color='red'">
                <v-subheader class="red--text text-uppercase pa-0"
                  >now stemaing</v-subheader
                >
                <h1
                  class="
                    white--text
                    text-capitalize
                    font-weight-light
                    display-2
                  "
                >
                  <span class="font-weight-bold">now</span>Stemaing
                </h1>
                <v-btn color="red" class="mt-4 white--text" min-width="150"
                  >view movies</v-btn
                >
              </v-col>
            </v-row>
          </v-container>
        </v-img>
      </v-sheet>
    </v-carousel-item>
  </v-carousel>
</template>

<script>
export default {
  name: "slider-app",
  data() {
    return {
      imgs: [
        "https://galaxylollywood.com/wp-content/uploads/2019/03/cinima.jpg",
        "https://c8.alamy.com/compfr/eaatcb/bobine-de-film-avec-magic-light-eaatcb.jpg",
      ],
    };
  },
};
</script>

