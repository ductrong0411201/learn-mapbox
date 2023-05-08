<template>
  <div class="flex">
    <div class="flex">
      <div><v-card-text>Overview of Changes in </v-card-text></div>
      <div class="select">
        <v-select :items="items" variant="outlined"></v-select>
      </div>
    </div>
    <div class="flex">
      <div><v-card-text>Compare Month</v-card-text></div>
      <div><v-card-text>VS Month</v-card-text></div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      items: [
        "California",
        "Colorado",
        "Florida",
        "Georgia",
        "Texas",
        "Wyoming",
      ],
      tab: "California",
      dateTime: []
    };
  },
  mounted(){
    this.getData()
  },
  methods: {
    async getData() {
      const response = await fetch("https://greencover.eofactory.ai/api/v1/imageries/months?source=sentinel&aoi_id=218");
      const jsonData = await response.json();
      console.log(jsonData);
      this.dateTime=jsonData.data
    }
  }
};
</script>

<style scoped>
.flex {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.select {
  margin-left: 10px;
  width: 300px;
}
</style>
