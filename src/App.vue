<template>
    <section v-loading="loading">
        <el-header>Fleetsmart Technical Question</el-header>
        <section class="anagram-wrapper ">
            <h2>Anagram checker</h2>
            <el-divider></el-divider>
            <div class="row-bg">
                <el-row type="flex">
                    <el-col :span="8">
                        <div class="grid-content">
                            <el-input style="width: 90%" placeholder="Enter first word" v-model="words.word1"/>
                        </div>
                    </el-col>
                    <el-col :span="8">
                        <div class="grid-content ">
                            <el-input style="width: 90%" placeholder="Enter second word" v-model="words.word2"/>
                        </div>
                    </el-col>
                    <el-col :span="8">
                        <div class="grid-content ">
                            <el-input style="width: 90%" placeholder="Enter third word" v-model="words.word3"/>
                        </div>
                    </el-col>
                    <el-col :span="8">
                        <div class="grid-content ">
                            <el-button type="success" v-on:click="compareStrings">Compare</el-button>
                        </div>
                    </el-col>

                </el-row>
                <div class="alert-wrapper">
                    <el-alert :title="anagramCheckResult" type="success" v-if="anagramCheckResult" effect="dark"
                              show-icon center/>
                    <el-alert :title="anagramCheckError" type="error" v-if="anagramCheckError" effect="dark" show-icon
                              center/>
                </div>

            </div>
        </section>
        <section class="table-wrapper">
            <h2>Distance checker</h2>
            <el-divider></el-divider>
            <el-table type="flex" class="table-bg" justify="space-around" :data="distanceData" stripe
                      style="width: 100%">
                <el-table-column prop="name" label="Name"/>
                <el-table-column prop="distance" label="Distance, Km"/>
            </el-table>
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
    .el-row {
        margin-bottom: 20px;
    }

    .el-col {
        border-radius: 4px;
    }

    .grid-content {
        padding: 7%;
        border-radius: 5px;
        min-height: 30px;
    }

    .row-bg {
        box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);
        min-height: 80px;
    }

    .anagram-wrapper {
        padding: 25px 25px 5px 25px;
        background-color: #f9fafc;
    }

    .alert-wrapper {
        padding: 1px 25px 15px 25px;
    }

    .table-wrapper {
        padding: 25px;
        background-color: #f9fafc;
    }

    .table-bg {
        padding: 10px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
    }

    .el-header {
        background-color: #24292E;
        color: white;
        text-align: start;
        line-height: 60px;
        font-size: 28px;
    }
</style>
