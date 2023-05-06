<template>
  <div class="container">
    <div>
      <nav>
        <div class="header">
          <v-card class="card">
            <v-card-action>
              <v-btn text>OVERVIEW</v-btn>
              <v-btn text>MAP VIEW</v-btn>
            </v-card-action>
          </v-card>
        </div>
      </nav>
    </div>
    <div class="main">
      <OverviewContainer></OverviewContainer>
    </div>
  </div>
</template>

<script>
import OverviewContainer from "@/components/Overview/OverviewContainer.vue";
export default {
  components: { OverviewContainer },
};
</script>

<style scoped>
.main {
  margin: 0 20px;
}
.container {
  background-color: rgb(244, 236, 253);
}
</style>
