<template>
    <div class="body container-fluid">

        <div class="title">
            <h3>Área do gestor financeiro</h3>
            <hr />
        </div>

        <div class="row button-search-area">
            <div class="col-md-6 button">
                <button @click="getList()" class="btn btn-primary">Ver lista de inadimplentes</button>
            </div>
            <div class="col-md-6 button">
                <input type="text" v-model="search" placeholder="Pesquisar" />
                <button @click="getListFilter()" class="btn">
                    <img src="../../assets/images/search.png" class="icon" />
                </button>
            </div>
        </div>
        
        <div v-if="inadimplentes.length === 0" class="no-list"></div>

        <div v-if="inadimplentes.length > 0" class="table-area">
            <table class="table">
                <thead>
                    <tr>
                        <th @click="getListOrder('name')" class="name-table" scope="col">Nome</th>
                        <th @click="getListOrder('since')" class="name-table" scope="col">Desde</th>
                        <th @click="getListOrder('value')" class="name-table" scope="col">Valor</th>
                    </tr>
                </thead>
                <tbody v-for="(item, index) in inadimplentes" :key="index">
                    <tr>
                        <td>{{item.name}}</td>
                        <td>{{getDate(item.since)}}</td>
                        <td>{{getValue(item.value)}}</td>
                    </tr>
                </tbody>
            </table>
        </div>

    </div>
</template>

<script>
import { get } from '../../services/api'
// import { get, put, post, del } from '../../services/api'
import moment from 'moment'

export default {
    data() {
        return {
            inadimplentes: [],
            search: ''
        };
    },
    props: {

    },
    created() {
        
    },
    methods: {
        async getList() {
            await get('/inadimplentes')
            .then(res => {
                console.log(res)
                this.inadimplentes = res
            })
        },
        async getListOrder(type) {
            await get(`/inadimplentes?order=${type}`)
            .then(res => {
                console.log(res)
                this.inadimplentes = res
            })
        },
        async getListFilter() {
            await get(`/inadimplentes?filter=${this.search}`)
            .then(res => {
                console.log(res)
                this.inadimplentes = res
            })
        },
        getDate(date) {
            return moment(date).format('DD/MM/YYYY')
        },
        getValue(value) {
            return value.toLocaleString("pt-BR", {
                style: "currency",
                currency: "BRL"
            });
        },
    },
};
</script>

<style scoped>
.title {
    text-align: left;
    margin-top: 120px;
}
.button-search-area {
    margin-top: 70px;
}
.table-area {
    margin-top: 70px;
    margin-bottom: 200px;
}
.name-table {
    cursor: pointer;
}
.no-list {
    height: 100vh;
}
.icon {
    margin-top: -10px;
    width: 20px;
}
</style>