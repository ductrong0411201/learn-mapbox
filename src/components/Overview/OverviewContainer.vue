<template>
  <v-card>
    <div class="flex">
      <div class="flex">
        <div><v-card-text>Overview of Changes in </v-card-text></div>
        <div class="select">
          <v-select :items="items"></v-select>
        </div>
      </div>
      <div class="flex">
        <div></div>
        <div></div>
      </div>
    </div>

    <v-divider></v-divider>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      items: [
        "California",
        "Colorado",
        "Florida",
        "Georgia",
        "Texas",
        "Wyoming",
      ],
    };
  },
};
</script>

<style scoped>
.flex {
  display: flex;
}
.select {
  margin-left: 10px;
  width: 300px;
}
</style>
