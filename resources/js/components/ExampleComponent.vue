<template>
    <div class="container justify-content-center mt-5 ">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="mb-2 col-md-2 row">
                    <select v-model="limit" @change="changeLimit()" class="form-control" id="exampleFormControlSelect1">
                        <option>10</option>
                        <option>25</option>
                        <option>50</option>
                        <option>100</option>
                    </select>
                </div>
                <table class="table shadow-sm">
                    <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">Customer Name</th>
                        <th scope="col">E mail</th>
                        <th scope="col">Birth Date</th>
                        <th scope="col">State</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(item,index) in datas" :key="index">
                        <th scope="row">{{ index + (currentPage === 1 ? currentPage : ((currentPage-1) * limit ) + 1) }}</th>
                        <td>{{ item.full_name }}</td>
                        <td>{{ item.email }}</td>
                        <td>{{ normalizeDate(item.birth_date) }}</td>
                        <td><span v-if="item.is_deleted === 0" class="badge badge-pill badge-success">Active</span><span v-else class="badge badge-pill badge-danger">Pasive</span></td>
                    </tr>
                    </tbody>
                </table>

                <nav aria-label="Page navigation example shadow-lg">
                    <ul class="pagination">
                        <li class="page-item" :class="currentPage === 1 ? 'disabled' : ''"><a @click="getCustomer(currentPage - 1)" class="page-link" href="#">Previous</a></li>
                        <li v-for="(page, index) in totalPage" :key="index" class="page-item" :class="page === currentPage ? 'active' : ''">
                            <a class="page-link " href="#" @click="getCustomer(page)">{{ page }}</a>
                        </li>
                        <li class="page-item" :class="currentPage === totalPage ? 'disabled' : ''"><a @click="getCustomer(currentPage + 1)" class="page-link" href="#">Next</a></li>
                    </ul>
                </nav>

            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
    export default {
        data(){
            return{
                datas:[],
                totalPage: Number,
                currentPage: Number,
                limit: 10,
            }
        },
        mounted() {
            this.getCustomer();
        },
        methods:{
            getCustomer(page = null){
                let url = null;
                if (page){
                    if(page === this.currentPage) return false;
                    url = 'http://localhost:3000/customers?page='+page+'&limit='+this.limit;
                }
                axios
                    .get(url || 'http://localhost:3000/customers')
                    .then(response => {
                        this.datas          = response.data.data;
                        this.totalPage      = parseInt(response.data.meta.totalPage);
                        this.currentPage    = parseInt(response.data.meta.page);
                    });
            },
            normalizeDate(date){
                return moment(String(date)).locale("tr").format("Do MMM YYYY");
            },
            changeLimit(){
                console.log('sefer');
                this.getCustomer(1);
            }
        }
    }
</script>
<style>

</style>
