<template>
    <div id="app">
        <Home msg="ASP.NET Core 6.0 WebAPI App using Vue.js 3!" />
        <button @click="count++" v-if="show">Counter: {{ count }}</button>
        <div class="info">
            <strong>Weather forecast refreshed {{ count }} times!</strong>
            <button style="float:right;" class="btn btn-success" @click="refreshData('weatherforecast');counter()">Refresh</button>
        </div>
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

<style>
    .info{
        margin:10px 15px;
    }

    table {
        border: 2px solid #42b983;
        border-radius: 3px;
        background-color: #fff;
    }

    th {
        background-color: #198754;
        color: rgba(255, 255, 255, 0.66);
        cursor: pointer;
        user-select: none;
    }

    td {
        background-color: #f9f9f9;
    }

    th,
    td {
        min-width: 120px;
        padding: 10px 20px;
    }
        th.active {
            color: #fff;
        }
            th.active .arrow {
                opacity: 1;
            }
</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.21.1/axios.min.js"></script>
<script>
    import Home from './components/Home.vue';

    export default {
        name: 'app',
        components: {
            Home
        },
        data() {
            return {
                count: 0,
                weatherforecast: '',
                show:false
            }
        },
        methods: {
            counter() {
                this.count++
            },
            refreshData(urlText) {
                axios.get("http://localhost:13183/" + urlText)
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
<style>
</style>

