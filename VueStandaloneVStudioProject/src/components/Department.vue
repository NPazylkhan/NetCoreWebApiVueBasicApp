<template>
    <div class="tab">
        <button type="button" style="float: right;" class="button buttonHover" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="addClick()">Add Department</button>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>
                        <div class="d-flex flex-row">
                            <input class="form-control m-2" v-model="DepartmentIdFilter" v-on:keyup="FilterFn()" placeholder="Id">
                            <button type="button" class="btn btn-light" @click="sortResult('DepartmentId',sort=!sort)">
                                <i class="fa fa-sort"></i>
                            </button>
                        </div>
                        Id
                    </th>
                    <th>
                        <div class="d-flex flex-row">
                            <input class="form-control m-2" v-model="DepartmentNameFilter" v-on:keyup="FilterFn()" placeholder="Department">
                            <button type="button" class="btn btn-light" @click="sortResult('DepartmentName',sort=!sort)">
                                <i class="fa fa-sort"></i>
                            </button>
                        </div>
                        Department
                    </th>
                    <th>
                        Options
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="dep in departments" :key="dep.DepartmentId">
                    <td>{{dep.DepartmentId}}</td>
                    <td>{{dep.DepartmentName}}</td>
                    <td>
                        <button type="button" class="btn btn-light mr-1" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="editClick(dep)">
                            <i class="fa fa-edit"></i>
                        </button>
                        <button type="button" @click="deleteClick(dep.DepartmentId)" class="btn btn-light mr-1">
                            <i class="fa fa-times"></i>
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">{{modalTitle}}</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="input-group mb-3">
                            <span class="input-group-text">Department Name</span>
                            <input type="text" class="form-control" v-model="DepartmentName">
                        </div>
                        <button type="button" @click="createClick()" v-if="DepartmentId==0" class="button buttonHover">Create</button>
                        <button type="button" @click="updateClick()" v-if="DepartmentId!=0" class="button buttonHover">Update</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.21.1/axios.min.js"></script>
<script>
    const variables = {
        API_URL: "http://localhost:13183/api/",
        PHOTO_URL: "http://localhost:13183/photos/"
    }
    export default {
        name: 'Department',
        data() {
            return {
                departments: [],
                modalTitle: "",
                DepartmentName: "",
                DepartmentId: 0,
                DepartmentNameFilter: "",
                DepartmentIdFilter: "",
                departmentsWithoutFilter: [],
                sort:true
            }
        },
        methods: {
            refreshData() {
                axios.get(variables.API_URL + "department")
                    .then((response) => {
                        this.departments = response.data;
                        this.departmentsWithoutFilter = response.data;
                    });
            },
            addClick() {
                this.modalTitle = "Add Department";
                this.DepartmentId = 0;
                this.DepartmentName = "";
            },
            editClick(dep) {
                this.modalTitle = "Edit Department";
                this.DepartmentId = dep.DepartmentId;
                this.DepartmentName = dep.DepartmentName;
            },
            createClick() {
                axios.post(variables.API_URL + "department", { DepartmentName: this.DepartmentName })
                    .then((response) => {
                        this.modalShow = false;
                        this.refreshData();
                        alert(response.data);
                    });
            },
            updateClick() {
                axios.put(variables.API_URL + "department", {
                    DepartmentId: this.DepartmentId,
                    DepartmentName: this.DepartmentName
                })
                    .then((response) => {
                        this.refreshData();
                        alert(response.data);
                    });
            },
            deleteClick(id) {
                if (!confirm("Are you sure?")) {
                    return;
                }
                axios.delete(variables.API_URL + "department/" + id)
                    .then((response) => {
                        this.refreshData();
                        alert(response.data);
                    });
            },
            FilterFn() {
                var DepartmentIdFilter = this.DepartmentIdFilter;
                var DepartmentNameFilter = this.DepartmentNameFilter;

                this.departments = this.departmentsWithoutFilter.filter(
                    function (el) {
                        return el.DepartmentId.toString().toLowerCase().includes(
                            DepartmentIdFilter.toString().trim().toLowerCase()
                        ) &&
                            el.DepartmentName.toString().toLowerCase().includes(
                                DepartmentNameFilter.toString().trim().toLowerCase()
                            )
                    });
            },
            sortResult(prop, asc) {
                this.departments = this.departmentsWithoutFilter.sort(function (a, b) {
                    if (asc) {
                        return (a[prop] > b[prop]) ? 1 : ((a[prop] < b[prop]) ? -1 : 0);
                    }
                    else {
                        return (b[prop] > a[prop]) ? 1 : ((b[prop] < a[prop]) ? -1 : 0);
                    }
                })
            }
        },
        mounted: function () {
            this.refreshData();
        }
    };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

