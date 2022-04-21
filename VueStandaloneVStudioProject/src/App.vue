<template>
    <div id="app">
        <Home msg="Hello world!" />
        <button @click="count++">Counter: {{count}}</button><br /><br />
        <button @click="refreshData">Refresh Weather Forecast</button>

        <table class="table table-hover">
            <tr>
                <th>Date</th>
                <th>Summary</th>
                <th>Temperature F</th>
                <th>Temperature C</th>
            </tr>
            <tr v-for="item in weatherforecast" :key="item.Date">
                <td>{{formatDate(item.Date)}}</td>
                <td>{{item.Summary}}</td>
                <td>{{item.TemperatureF}}</td>
                <td>{{item.TemperatureC}}</td>
            </tr>
        </table>
    </div>
</template>

<style>
    table {
        border: 2px solid #42b983;
        border-radius: 3px;
        background-color: #fff;
    }

    th {
        background-color: #42b983;
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
                weatherforecast: ''
            }
        },
        methods: {
            refreshData() {
                axios.get("http://localhost:13183/weatherforecast")
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
            this.refreshData();
        }
    };
</script>
<style>
</style>

