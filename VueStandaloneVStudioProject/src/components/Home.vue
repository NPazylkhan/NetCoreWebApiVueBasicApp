<template>
    <div class="tab">
        <div class="home">
            <h3>{{ msg }}</h3>
        </div>
        <button @click="count++" v-if="show">Counter: {{ count }}</button>
        <h5>
            Weather forecast refreshed {{ count }} times!
            <button style="float:right;" class="button buttonHover" @click="refreshData('weatherforecast');counter()">Refresh</button>
        </h5>
        <table class="table table-hover table-stripped table-bordered">
            <tr>
                <th>Date</th>
                <th>Summary</th>
                <th>Temperature F</th>
                <th>Temperature C</th>
            </tr>
            <tr v-for="item in weatherforecast" :key="item.Date">
                <td>{{ formatDate(item.Date) }}</td>
                <td>{{ item.Summary }}</td>
                <td>{{ item.TemperatureF }}</td>
                <td>{{ item.TemperatureC }}</td>
            </tr>
        </table>
    </div>
</template>

<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.21.1/axios.min.js"></script>
<script>
    export default {
        name: 'Home',
        props: {
            msg: String
        },
        data() {
            return {
                count: 0,
                weatherforecast: '',
                show: false,
                apiUrl: 'http://localhost:13183/',
            }
        },
        methods: {
            counter() {
                this.count++
            },
            refreshData(urlText) {
                axios.get(this.apiUrl + urlText)
                    .then((response) => {
                        this.weatherforecast = response.data;
                    });
            },
            formatDate(date) {
                const options = { year: 'numeric', month: 'numeric', day: 'numeric' }
                return new Date(date).toLocaleDateString('ru', options)
            },
        },
        mounted: function () {
            this.refreshData('weatherforecast');
            this.counter();
        }
    };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .home {
        display: flex;
        justify-content: center;
    }
</style>

