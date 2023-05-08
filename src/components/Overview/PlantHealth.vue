<template>
    <div>
        <div class="title">
            <h1>Green Cover</h1>
        </div>
        <div style="display: flex; justify-content: space-between; padding: 40px 15px 5px">
            <h4 class="flex">
                Data for the month of
                <div class="color"> 
                    Aug/2022 
                    <!-- {{typeof dateTime[0] }} -->
                </div>
            </h4>
            <h4 class="flex">
                Data for the month of 
                <div class="color">
                    Aug/2022 
                    <!-- {{dateTime[1] }} -->
                </div>
            </h4>
        </div>
        <div style="display: flex; justify-content: space-between">
            <v-card class="box">hi</v-card>
            <v-card class="box">hi</v-card>
        </div>
        <div>
            <v-card class="box">hi</v-card>
        </div>
    </div>
</template>
<script>
export default {
    data: () => {
    return {
      dateTime: []
    };
  },
  mounted(){
    this.getDate()
  },
  methods: {
    async getDate() {
      const response = await fetch("https://greencover.eofactory.ai/api/v1/imageries/months?source=sentinel&aoi_id=218");
      const jsonData = await response.json();
    //   console.log(jsonData);
      this.dateTime=jsonData.data
    }
  }
}
</script>
<style>
.title {
    height: 8vh;
    padding: 40px 20px 
}
.color{
    margin-left: 5px;
    color: rgb(137, 63, 242)
}
.flex{
    display: flex;
}
.box{
    position: relative;
    flex: 1 1 0%;
    box-shadow: rgba(128, 124, 133, 0.35) 1px 2px 2px, rgba(137, 63, 242, 0.33) 0px 0px 4px inset;
    border-radius: 10px;
    background-color: rgb(255, 255, 255);
    margin: 20px;
    padding: 10px;
    height: 200px
}
</style>