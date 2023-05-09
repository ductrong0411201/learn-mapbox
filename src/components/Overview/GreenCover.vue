<template>
    <div>
        <div class="title">
            <h1>Green Cover</h1>
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
                <div class="data">
                    <div class="greencover_area">
                        {{ this.result.green_cover_area.toFixed(2) }}
                        <sup style="font-weight: 400; font-size: 1.5vw; ">sqkm</sup>
                    </div>
                    <div class="nonGreen">
                        <div class="nonGreenArea">- Non Green Area: {{ this.result.non_green_area.toFixed(2) }} sqkm</div>
                        <div class="waterArea">- Water Area: {{ this.result.water_area.toFixed(2) }} sqkm</div>
                    </div>
                </div>
            </v-card>
            <v-card class="box">
                <div class="data">
                    <div class="greencover_area">
                        {{ this.data.green_cover_area.toFixed(2) }}
                        <sup style="font-weight: 400; font-size: 1.5vw; ">sqkm</sup>
                    </div>
                    <div class="nonGreen">
                        <div class="nonGreenArea">- Non Green Area: {{ this.data.non_green_area.toFixed(2) }} sqkm</div>
                        <div class="waterArea">- Water Area: {{ this.data.water_area.toFixed(2) }} sqkm</div>
                    </div>
                </div>
            </v-card>
        </div>
        <div>
            <v-card class="box">
                <div style="display: flex; align-items: center; height: 100%;">
                    <div class="change">
                        <span style="font-size: 4vw; font-weight: 700; color: rgb(0, 0, 0);">
                            {{ this.result.area_percent_change ? this.result.area_percent_change.toFixed(2) : 0 }}%
                        </span>
                        <i v-if=" this.result.area_percent_change > 0" aria-hidden="true" class="v-icon notranslate mdi mdi-menu-up theme--light green--text"
                            style="font-size: 70px; margin-top: -3vh;"></i>
                        <i v-else aria-hidden="true" class="v-icon notranslate mdi mdi-menu-down theme--light red--text"
                            style="font-size: 70px; margin-bottom: -3vh;"></i>
                        <div>
                            <span
                                style="line-height: 3vh; font-size: 1.1vw; text-transform: uppercase; color: rgb(0, 0, 0);">
                                Change in Green Cover Area
                            </span>
                            <div style="height: 5vh;">
                                <span style="font-size: 18px; font-weight: bold; color: red;" class="pl-2">
                                    {{ this.result.area_change.toFixed(0) }} sqkm
                                    <span style="font-weight: normal; color: black;">
                                        in Green Cover area compared to
                                        <span style="color: rgb(137, 63, 242); font-weight: bold;">
                                            Aug
                                        </span>
                                        records.
                                    </span>
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </v-card>
        </div>
    </div>
</template>
<script>
export default {
    props: ['data', 'result'],
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
.change {
    height: 10vh;
    font-weight: 600;
    /* font-size: 3vw; */
    color: rgb(0, 0, 0);
    display: flex;
}

.nonGreenArea {
    font-size: 0.9vw;
    font-weight: bold;
    color: rgb(156, 122, 0);
}

.waterArea {
    font-size: 0.9vw;
    font-weight: bolder;
    color: rgb(31, 123, 176);
}

.nonGreen {
    /* height: 80px; */
    position: absolute;
    bottom: -10px;
    left: 0px;
    background-color: white;
}

.greencover_area {
    /* min-width: 200px; */
    height: 8vh;
    font-weight: 600;
    font-size: 2.7vw;
    color: rgb(137, 63, 242);
    position: absolute;
    top: -30px;
    left: 0px;
    background-color: white;
    padding-right: 10px;
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

.box {
    position: relative;
    flex: 1 1 0%;
    box-shadow: rgba(128, 124, 133, 0.35) 1px 2px 2px, rgba(137, 63, 242, 0.33) 0px 0px 4px inset;
    border-radius: 10px;
    background-color: rgb(255, 255, 255);
    margin: 20px;
    padding: 20px;
    height: 20vh 
}

.data {
    margin: 30px 10px 10px 10px;
    height: 12vh;
    border-top: 1px solid rgb(137, 63, 242);
    border-right: 1px solid rgb(137, 63, 242);
    border-bottom: 1px solid rgb(137, 63, 242);
    position: relative;
}
</style>