<template>
  <div>
    <silder />
    <films keep-alive />
  </div>
</template>

<script>
import Films from "~/components/films.vue";
import silder from "~/components/silder.vue";
export default {
  components: { silder, Films },
  name: "IndexPage",
};
</script>
