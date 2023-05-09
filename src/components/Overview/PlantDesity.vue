<template>
    <div>
        <div class="title">
            <h1>Green Density</h1>
        </div>
        <div style="display: flex; justify-content: space-between; padding: 30px 15px 5px">
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
            <v-card class="box">
                <div>
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <span class="plant_health">
                            PlantHealth
                        </span>
                        <b style="color: rgb(137, 63, 242);">sqkm</b>
                    </div>
                    <div v-for="(item, index) in plantHealth" :key="index" :style="{color: item.color[0], display: 'flex', justifyContent: 'space-between', alignItems: 'center', margin: '5px 5px'}">
                        <div style="width: 7vw; font-size: 13px; font-weight: bold;">{{ item.label }}</div>
                        <div :style="{height: '10px', width: '30%', backgroundColor: item.color[0]}"></div>
                        <div style="width: 40px; font-size: 13px;">{{ item.data[1].toFixed(2) }}</div>
                    </div>
                </div>
            </v-card>
            <v-card class="box">
                <div>
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <span class="plant_health">
                            PlantHealth
                        </span>
                        <b style="color: rgb(137, 63, 242);">sqkm</b>
                    </div>
                    <div v-for="(item, index) in plantHealth" :key="index" :style="{color: item.color[0], display: 'flex', justifyContent: 'space-between', alignItems: 'center', margin: '5px 5px'}">
                        <div style="width: 7vw; font-size: 13px; font-weight: bold;">{{ item.label }}</div>
                        <div :style="{height: '10px', width: '30%', backgroundColor: item.color[0]}"></div>
                        <div style="width: 40px; font-size: 13px;">{{ item.data[0].toFixed(2) }}</div>
                    </div>
                </div>
            </v-card>
        </div>
        <div>
            <v-card class="box">
                <div>
                    <div>
                        <span class="change_plant">
                            % Change in Plant Health
                        </span>
                    </div>
                    <div v-for="(item, index) in changePlant" :key="index" :style="{color: item.color, display: 'flex', justifyContent: 'space-between', alignItems: 'center', margin: '5px 5px'}">
                        <div style="width: 7vw; font-size: 13px; font-weight: bold;">{{ item.label }}</div>
                        <div :style="{height: '10px', width: '50%', backgroundColor: item.color}"></div>
                        <div style="width: 80px; font-size: 13px;">
                            {{ item.change_percent.toFixed(2) }}
                        <i v-if=" item.change_percent > 0" aria-hidden="true" class="v-icon notranslate mdi mdi-menu-up theme--light green--text"
                            style="font-size: 20px; "></i>
                        <i v-else aria-hidden="true" class="v-icon notranslate mdi mdi-menu-down theme--light red--text"
                            style="font-size: 20px; "></i>
                        </div>
                    </div>
                </div>
            </v-card>
        </div>
    </div>
</template>
<script>
export default {
    props: ['plantHealth', 'changePlant'],
    data: () => {
        return {
            dateTime: []
        };
    },
    mounted() {
        this.getDate()
    },
    methods: {
        async getDate() {
            const response = await fetch("https://greencover.eofactory.ai/api/v1/imageries/months?source=sentinel&aoi_id=218");
            const jsonData = await response.json();
            //   console.log(jsonData);
            this.dateTime = jsonData.data
        }
    }
}
</script>
<style>
.change_plant{
    color: rgb(0, 0, 0);
    font-size: 1.7vw;
    font-weight: bold;
    font-style: italic;
}
.plant_health {
    color: rgb(137, 63, 242);
    font-size: 1.7vw;
    font-weight: 80;
    font-style: italic;
}

.title {
    height: 8vh;
    padding: 40px 20px
}

.color {
    margin-left: 5px;
    color: rgb(137, 63, 242)
}

.flex {
    display: flex;
}

/* .box {
    position: relative;
    flex: 1 1 0%;
    box-shadow: rgba(128, 124, 133, 0.35) 1px 2px 2px, rgba(137, 63, 242, 0.33) 0px 0px 4px inset;
    border-radius: 10px;
    background-color: rgb(255, 255, 255);
    margin: 20px;
    padding: 10px;
    height: 200px 
}*/
</style>