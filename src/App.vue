<template>
    <section v-loading="loading">
        <el-table :data="distanceData" style="width: 100%">
            <el-table-column prop="name" label="Name"/>
            <el-table-column prop="distance" label="Distance"/>
        </el-table>

        <section>
            <el-row>
                <el-col :span="6"><el-input placeholder="Enter first word" v-model="words.word1"></el-input></el-col>
                <el-col :span="6"><el-input placeholder="Enter second word" v-model="words.word2"></el-input></el-col>
                <el-col :span="6"><el-input placeholder="Enter third word" v-model="words.word3"></el-input></el-col>
                <el-col :span="6"><el-button type="success" plain v-on:click="compareStrings">Compare</el-button></el-col>
            </el-row>
            <el-alert :title="anagramCheckResult" type="success" v-if="anagramCheckResult"/>
            <el-alert :title="anagramCheckError" type="error" v-if="anagramCheckError"/>
        </section>


    </section>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'app',
        data: () => ({
            distanceData: [],
            words: {
                word1: '',
                word2: '',
                word3: '',
            },
            anagramCheckResult: '',
            anagramCheckError: '',
            loading: false,
        }),
        created() {
            this.loading = true;
            axios.get('http://localhost:3000/')
                .then(response => {
                    this.loading = false;
                    this.distanceData = response.data;
                })
                .catch(() => {
                    this.loading = false;
                })
        },
        methods: {
            compareStrings() {
                this.anagramCheckResult = '';
                this.anagramCheckError = '';
                this.loading = true;
                axios.post('http://localhost:3000/', {words: Object.values(this.words)})
                    .then(response => {
                        this.loading = false;
                        this.anagramCheckResult = response.data;
                    })
                    .catch(error => {
                        this.loading = false;
                        this.anagramCheckError = error.response.data;
                    })
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
