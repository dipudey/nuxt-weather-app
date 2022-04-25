<template>
    <div class="row">
        <div class="col-md-8 m-auto mt-5">
            <div class="card">
                <div class="card-body bg-secondary tex-white">
                    <div class="d-flex justify-content-between" v-if="weather">
                        <div>
                            <h6 class="text-light">Temperature</h6>
                            <h3 class="text-light my-3">{{ weather.name }}</h3>
                            <p class="text-light">
                                <span class="display-2">{{ temp }} &deg;C</span>
                                <span class="caption ml-4">{{
                                    weather.weather[0].description
                                }}</span>
                            </p>
                        </div>
                        <div>
                            <h6 class="text-light">Wind & Pressure</h6>
                            <h5 class="text-white mt-4">
                                Wind: {{ weather.wind.speed }} m/s ({{
                                    weather.wind.deg
                                }}
                                &deg;)
                            </h5>
                            <h5 class="text-white mt-3">
                                Humidity: {{ weather.main.humidity }} %
                            </h5>
                            <h5 class="text-white mt-3">
                                Pressure: {{ weather.main.pressure }} hPa
                            </h5>
                        </div>
                        <div>
                            <h6 class="text-light">Other</h6>
                            <h5 class="text-white mt-3">
                                Max Temperature:
                                {{ Math.round(weather.main.temp_max - 273) }}
                                &deg; C
                            </h5>
                            <h5 class="text-white mt-3">
                                Min Temperature:
                                {{ Math.round(weather.main.temp_min - 273) }}
                                &deg; C
                            </h5>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-md-8 m-auto mt-5">
            <div class="mb-3">
                <form @submit.prevent="temperature">
                    <input
                        type="text"
                        v-model="city"
                        class="form-control no-radius"
                        id="exampleFormControlInput1"
                        placeholder="Enter City Name"
                    />
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    head: {
        title: "Weather app nuxt",
        weather: "",
    },
    data() {
        return {
            city: "dhaka",
            weather: "",
        };
    },
    computed: {
        temp() {
            // return 0;
            if (this.weather) {
                return Math.round(this.weather.main.temp - 273);
            }
            return 0;
        },
    },
    methods: {
        temperature() {
            this.$axios
                .$get(
                    `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${process.env.API_KEY}`
                )
                .then((res) => (this.weather = res))
                .catch((res) => alert("not found"));
        },
    },
    created() {
        this.temperature();
    },
};
</script>
<style scoped>
.no-radius {
    border-radius: 0px !important;
}
</style>
