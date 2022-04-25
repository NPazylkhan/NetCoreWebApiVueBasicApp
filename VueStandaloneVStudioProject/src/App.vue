<template>
    <div id="app">
        <div class="demo">
            <button v-for="tab in tabs"
                    :key="tab"
                    :class="['tab-button', { active: currentTab === tab }]"
                    @click="currentTab = tab">
                {{ tab }}
            </button>
            <component :is="currentTab" msg="Core using Vue" class="tab"></component>
        </div>
    </div>
</template>

<style>
    table {
        border: 2px solid #42b983;
        border-radius: 3px;
        background-color: #fff;
    }

    th {
        background-color: #f0f0f0;
        color: #5e5e5e;
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

    .demo {
        font-family: sans-serif;
        border: 1px solid #eee;
        border-radius: 2px;
        padding: 20px 30px;
        margin-top: 1em;
        margin-bottom: 40px;
        user-select: none;
        overflow-x: auto;
    }

    .tab-button {
        padding: 6px 10px;
        border-top-left-radius: 3px;
        border-top-right-radius: 3px;
        border: 1px solid #ccc;
        cursor: pointer;
        background: #f0f0f0;
        margin-bottom: -1px;
        margin-right: -1px;
    }

        .tab-button:hover {
            background: #e0e0e0;
        }

        .tab-button.active {
            background: #e0e0e0;
        }

    .tab {
        border: 1px solid #ccc;
        padding: 10px;
    }

    .button {
        background-color: #4CAF50; /* Green */
        border: none;
        color: white;
        padding: 8px 16px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        margin: 4px 2px;
        transition-duration: 0.4s;
        cursor: pointer;
        border-radius: 0.75em;
    }

    .buttonHover {
        background-color: white;
        color: black;
        border: 2px solid #555555;
    }

        .buttonHover:hover {
            background-color: #555555;
            color: white;
        }

    .fa{
        font-size:large;
    }
</style>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/js/bootstrap.bundle.min.js" integrity="sha384-gtEjrD/SeCtmISkJkNUaaKMoLD0//ElJ19smozuHV6z3Iehds+3Ulb9Bn9Plx0x4" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.21.1/axios.min.js"></script>
<script>
    import Home from './components/Home.vue';
    import Department from './components/Department.vue';
    import Employee from './components/Employee.vue';

    export default {
        name: 'app',
        components: {
            Home,
            Department,
            Employee
        },
        data() {
            return {
                currentTab: 'Home',
                tabs: ['Home', 'Department', 'Employee'],
            }
        }
    };
</script>

