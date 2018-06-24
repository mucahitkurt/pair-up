<template>
    <v-container grid-list-md text-xs-center>
        <v-layout v-if="!paired" row wrap>
            <v-flex v-for="(candidate, index) in candidates" :key="index" xs4>
                <v-card color="purple" class="white--text">
                    <v-container fluid grid-list-lg>
                        <v-layout row>
                            <v-flex xs6>
                                <img :src="imgUrl(candidate)" width="120" height="120"/>
                            </v-flex>
                            <v-flex xs6>
                                <v-card-text>
                                    <h1>{{candidate}}</h1>
                                </v-card-text>
                            </v-flex>
                        </v-layout>
                    </v-container>
                </v-card>
            </v-flex>
        </v-layout>
        <v-layout v-if="paired" row wrap>
            <div><h1>Session {{sessionCount}}</h1></div>
            <v-container v-for="(pair, index) in pairs" :key="index">
                <v-layout row wrap>
                    <v-flex xs4>
                        <v-card color="pink" class="white--text">
                            <v-container fluid grid-list-lg>
                                <v-layout row>
                                    <v-flex xs4>
                                        <v-card :img="imgUrl(pair.p1.name)" width="100" height="100">
                                            <v-card-text></v-card-text>
                                        </v-card>
                                    </v-flex>
                                    <v-flex xs4>
                                        <v-card-text><h1>{{pair.p1.name}}</h1></v-card-text>
                                    </v-flex>
                                </v-layout>
                            </v-container>
                        </v-card>
                    </v-flex>
                    <v-flex xs4>
                        <v-card color="green" class="white--text">
                            <v-container fluid grid-list-lg>
                                <v-layout row>
                                    <v-flex xs4>
                                        <v-card :img="imgUrl(pair.p2.name)" width="100" height="100">
                                            <v-card-text></v-card-text>
                                        </v-card>
                                    </v-flex>
                                    <v-flex xs4>
                                        <v-card-text><h1>{{pair.p2.name}}</h1></v-card-text>
                                    </v-flex>
                                </v-layout>
                            </v-container>
                        </v-card>
                    </v-flex>
                </v-layout>
            </v-container>
        </v-layout>
        <v-layout>
            <v-flex xs12>
                <v-btn v-if="!paired" color="orange" style="font-weight: bolder; color: white; font-size: x-large"
                       @click="pairups()">
                    Pair Up
                </v-btn>
                <v-btn v-if="paired" color="red" style="font-weight: bolder; color: white; font-size: x-large"
                       @click="session()">
                    Next Session
                </v-btn>
            </v-flex>
        </v-layout>

        <v-snackbar
                :timeout="5000"
                color="green"
                v-model="snackbar"
        >
            <h3>Done:)</h3>
            <v-btn dark flat @click.native="snackbar = false">Close</v-btn>
        </v-snackbar>
    </v-container>
</template>

<script>
    export default {
        name: "PairUp",
        data: function () {
            return {
                candidates: [
                    'Giray',
                    'Ahmed',
                    'Ekrem',
                    'Beytullah',
                    'Alperen',
                    'Gorkem',
                    'Aras',
                    'Mustafa',
                    'Deniz',
                    'Tugce',
                    'Ibrahim',
                    'Simay',
                    'Ahmet',
                    'Firat',
                ],
                pairs: [],
                paired: false,
                snackbar: false,
                sessionCount:1
            };
        },
        methods: {
            pairups: async function () {
                let retry = this.rnd(5, 10);
                while (retry > 0) {
                    this.pairup();
                    await this.sleep(1000);
                    retry--;
                }
                this.snackbar = true;
            },
            sleep: function (ms) {
                return new Promise(res => setTimeout(res, ms));
            },
            pairup: function () {
                this.pairs = [];
                let matched = [];

                while (matched.length !== this.candidates.length) {
                    let p1 = this.rnd(0, this.candidates.length - 1);
                    while (matched.indexOf(p1) >= 0) {
                        p1 = this.rnd(0, this.candidates.length - 1);
                    }

                    let p2 = this.rnd(0, this.candidates.length - 1);
                    while (p1 === p2 || matched.indexOf(p2) >= 0) {
                        p2 = this.rnd(0, this.candidates.length - 1);
                    }

                    matched.push(p1, p2);
                    this.pairs.push({
                            p1: {
                                name: this.candidates[p1],
                            },
                            p2: {
                                name: this.candidates[p2],
                            }
                        }
                    );
                }
                this.paired = true;
            },

            rnd: function (min, max) {
                return Math.floor(Math.random() * (max - min + 1)) + min;
            },

            imgUrl: function (img) {
                return require(`../assets/${img}.jpg`)
            },

            session: function () {
                let newPairs = [];
                for (let i = 1; i < this.pairs.length; i++) {
                    newPairs.push({p1: this.pairs[i].p1, p2: this.pairs[i - 1].p2});
                }
                newPairs.push({p1: this.pairs[0].p1, p2: this.pairs[this.pairs.length - 1].p2});
                this.pairs = newPairs;
                this.sessionCount++;
            }

        }


    }
</script>

<style scoped>

</style>