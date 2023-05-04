<template>
  <div id="app">
    <!--Navbar Here -->
    <div>
      <nav>
        <div class="header">
          <h3>GEOCODER</h3>
        </div>
      </nav>
    </div>
    <!--Index Page Here -->
    
    <!-- huongkoihihi -->
    <div>
      <div class="layout">
        SENTINEL
      </div>
      <div class="overview">
        <div class="bar_overview">
          <div>
            Overview of Changes in
          </div>
          <div>

          </div>
        </div>
        <div class="body_overview">
          <div class="left_overview">hi</div>
          <index />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import index from "./components/Index.vue";
export default {
  name: "App",
  components: {
    index,
  },
};
</script>

<style>
.left_overview{
  width: 50%;
  margin: 20px 20px;
  padding:10px;
  /* height: 100%;
  width: 50%; */
  /* border: 1px solid rgba(137, 63, 242, 0.2); */
  background-color: rgb(244, 239, 250);
  border-radius: 11px 0px 0px 11px;
}
.body_overview{
  margin: 10px;
  border-radius: 10px;
  box-shadow: 0 3px 1px -2px rgba(0,0,0,.2),0 2px 2px 0 rgba(0,0,0,.14),0 1px 5px 0 rgba(0,0,0,.12)!important;
  display: flex;
}
.bar_overview{
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
  height: 70px;
  border: 1px solid rgba(137, 63, 242, 0.2);
  box-shadow: rgba(0, 0, 0, 0.22) 0px 0px 4px inset;
  background: linear-gradient(rgb(239, 239, 239) 76.97%, rgba(227, 227, 227, 0.28) 100%);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  color: rgba(0,0,0,.6);
}
.overview{
  margin: 0px 40px;
  border-radius: 10px;
  /* width: 100%; */
  height: 100%;
  border: 1px solid rgba(137, 63, 242, 0.2);
  box-shadow: rgba(0, 0, 0, 0.22) 0px 0px 4px inset;
  background-color: rgb(255, 255, 255);
}
.layout {
  display: flex;
  flex: 1 1 auto;
  flex-wrap: nowrap;
  min-width: 0;
}
#app {
  background-color:rgb(244, 239, 250);
  color: rgb(137, 63, 242);
  font-family: system-ui, -apple-system, Roboto sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
html,
body {
  /* background: #fafcfd; */
  margin: 0;
}

nav {
  /* background: #ffffff; */
  box-shadow: 0px 2px 5px rgba(0, 58, 78, 0.15);
}
.header {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 0px 50px;
}

.header h3 {
  /* color: #1f2a53; */
  font-weight: 600;
}
</style>
