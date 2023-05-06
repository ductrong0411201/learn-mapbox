<template>
  <v-app>
    <div>
      <nav>
        <div class="header">
          <h3>Geocoder</h3>
        </div>
      </nav>
    </div>
    <v-main class="v-main">
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: "App",
  data: () => ({}),

  components: {},

  methods: {},

  async mounted() {},
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
html,
body {
  background: #fafcfd;
  margin: 0;
}

nav {
  background: #ffffff;
  box-shadow: 0px 2px 5px rgba(0, 58, 78, 0.15);
}
.header {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 0px 50px;
  height: 64px;
  margin-top: 0px;
  transform: translateY(0px);
  left: 0px;
  right: 0px;
  background-color: rgb(244, 239, 250);
  border-color: rgb(244, 239, 250);
  box-shadow: none !important;
}

.header h3 {
  color: #1f2a53;
  font-weight: 800;
}
 
.v-main {
    display: flex;
    flex: 1 0 auto;
    max-width: 100%;
    transition: .2s cubic-bezier(.4,0,.2,1);
}
</style>
