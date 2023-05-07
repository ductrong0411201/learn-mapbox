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
        <div><v-card-text>Compare Month</v-card-text></div>
        <div><v-card-text>VS Month</v-card-text></div>
      </div>
    </div>
    <v-divider></v-divider>
    <div>
      <!-- <v-card>
        <div class="flex">
          <v-card style="margin: 8px 16px">
            <v-tabs v-model="tab" bg-color="transparent" color="basil" grow>
              <v-tab v-for="item in items" :key="item" :value="item">
                {{ item }}
              </v-tab>
            </v-tabs>

            <v-window v-model="tab">
              <v-window-item v-for="item in items" :key="item" :value="item">
                <v-card color="basil" flat>
                  <v-card-text>{{ item }}</v-card-text>
                </v-card>
              </v-window-item>
            </v-window>
          </v-card>
        </div>
      </v-card> -->
    </div>
  </v-card>
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
