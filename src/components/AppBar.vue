<template>
    <div>
        <v-app-bar app color="dark" dark dense>
            <v-toolbar-title>
                <v-row no-gutters align="center">
                    <!--                    <img src="../../public/keti-logo.png" width="30" height="30" class="mr-2">-->
                    approval management
                </v-row>
            </v-toolbar-title>

            <v-spacer></v-spacer>

            <v-row no-gutters>
                <v-col cols="3">
                    <v-text-field
                        class="mx-2 mt-1"
                        dense hide-details outlined
                        ref="host"
                        v-model="host" :rules="host_rule"
                        placeholder="125.132.13.63"
                        label="HOST*"
                        required
                        :disabled="MOBIUS_CONNECTION_CONNECTED"
                    ></v-text-field>
                </v-col>
                <v-col cols="3">
                    <v-text-field
                        class="mx-2 mt-1"
                        dense hide-details outlined
                        ref="approval"
                        v-model="approval" :rules="approval_rule"
                        placeholder="MUV"
                        label="APPROVAL*"
                        required
                        :disabled="MOBIUS_CONNECTION_CONNECTED"
                    ></v-text-field>
                </v-col>
                <v-col cols="6">
                    <v-btn
                        class="mx-2 mt-1"
                        tile @click="GcsAppBarCreated"
                        elevation="5"
                        color="primary"
                        :disabled="MOBIUS_CONNECTION_CONNECTED"
                    > {{ MOBIUS_CONNECTION_TEXT }}
                    </v-btn>
                    <v-btn
                        class="mx-2 mt-1"
                        tile @click="GcsAppBarReseted"
                        elevation="2"
                        color="primary"
                        :disabled="!MOBIUS_CONNECTION_CONNECTED"
                    > {{ MOBIUS_DISCONNECTION_TEXT }}
                    </v-btn>
                </v-col>
            </v-row>

            <v-spacer></v-spacer>

        </v-app-bar>
    </div>
</template>

<script>
import axios from 'axios'
import EventBus from "@/EventBus";

export default {
    name: "AppBar",

    data: function () {
        return {
            open: false,
            MOBIUS_DISCONNECTION_TEXT: 'Disconnect',
            MOBIUS_CONNECTION_TEXT: 'Connect',

            MOBIUS_CONNECTION_CONNECTED: localStorage.getItem('mobius_connected') ? (localStorage.getItem('mobius_connected') === 'true') : false,
            MOBIUS_CONNECTION_DISABLED: false,

            host: localStorage.getItem('mobius_host') ? (localStorage.getItem('mobius_host')) : (this.$store.state.VUE_APP_MOBIUS_HOST),
            host_rule: [
                v => !!v || '호스트 주소는 필수 입력사항입니다.',
                v => /^[.0-9]*$/.test(v) || '호스트 주소는 숫자만 입력 가능합니다.'
            ],
            approval: localStorage.getItem('mobius_approval') ? (localStorage.getItem('mobius_approval')) : (this.$store.state.VUE_APP_MOBIUS_APPROVAL),
            approval_rule: [
                v => !!v || 'Approval 이름은 필수 입력사항입니다.',
                v => !/[~!@#$%^&*()+|<>?:{}]/.test(v) || 'Approval 이름에는 특수문자를 사용할 수 없습니다.'
            ],
            approval_list: []
        }
    },

    methods: {
        GcsAppBarCreated() {

            this.$store.state.VUE_APP_MOBIUS_HOST = this.host;
            this.$store.state.VUE_APP_MOBIUS_APPROVAL = this.approval;

            localStorage.setItem('mobius_host', this.host);
            localStorage.setItem('mobius_approval', this.approval);

            let self = this;

            axios({
                validateStatus: function (status) {
                    // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                    return status < 500;
                },
                method: 'get',
                url: 'http://' + this.$store.state.VUE_APP_MOBIUS_HOST + ':7579/Mobius/' + this.$store.state.VUE_APP_MOBIUS_APPROVAL,
                headers: {
                    'X-M2M-RI': String(parseInt(Math.random() * 10000)),
                    'X-M2M-Origin': 'SVue',
                    'Content-Type': 'application/json'
                }
            }).then(
                function (res) {

                    console.log('ApprovalAppBarCreated', 'http://' + self.$store.state.VUE_APP_MOBIUS_HOST + ':7579/Mobius/' + self.$store.state.VUE_APP_MOBIUS_APPROVAL, res);

                    if (res.status === 200) {
                        self.MOBIUS_CONNECTION_CONNECTED = true;
                        self.$store.state.MOBIUS_CONNECTION_CONNECTED = true;

                        localStorage.setItem('mobius_connected', self.MOBIUS_CONNECTION_CONNECTED);

                        axios({
                            validateStatus: function (status) {
                                // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                                return status < 500;
                            },
                            method: 'get',
                            url: 'http://' + self.$store.state.VUE_APP_MOBIUS_HOST + ':7579/Mobius/' + self.$store.state.VUE_APP_MOBIUS_APPROVAL + '/approval',
                            params: {
                                'fu': '1',
                                'lvl': '1',
                                'ty': '3',
                            },
                            headers: {
                                'Accept': 'application/json',
                                'X-M2M-RI': '12345',
                                'X-M2M-Origin': 'SOrigin'
                            }
                        }).then(
                            function (response) {
                                let list = response.data["m2m:uril"];
                                for (let idx in list) {
                                    let id = list[idx].split('/')
                                    self.approval_list.push(id[id.length - 1])
                                }
                                self.$store.state.APPROVAL_LIST = self.approval_list
                                EventBus.$emit('do-updated-approval-list', self.$store.state.APPROVAL_LIST);
                            }
                        ).catch(
                            function (error) {
                                console.log(error);
                            }
                        );
                    } else if (res.status === 404) {
                        axios({
                            validateStatus: function (status) {
                                // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                                return status < 500;
                            },
                            method: 'post',
                            url: 'http://' + self.$store.state.VUE_APP_MOBIUS_HOST + ':7579/Mobius',
                            headers: {
                                'X-M2M-RI': String(parseInt(Math.random() * 10000)),
                                'X-M2M-Origin': 'S' + self.$store.state.VUE_APP_MOBIUS_APPROVAL,
                                'Content-Type': 'application/json;ty=2'
                            },
                            data: {
                                'm2m:ae': {
                                    rn: self.$store.state.VUE_APP_MOBIUS_APPROVAL,
                                    api: '0.2.481.1.1111',
                                    lbl: [self.$store.state.VUE_APP_MOBIUS_APPROVAL],
                                    rr: true,
                                    poa: ["http://localhost:8080"]
                                }
                            }
                        }).then(
                            function () {
                                axios({
                                    validateStatus: function (status) {
                                        // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                                        return status < 500;
                                    },
                                    method: 'post',
                                    url: 'http://' + self.$store.state.VUE_APP_MOBIUS_HOST + ':7579/Mobius/' + self.$store.state.VUE_APP_MOBIUS_APPROVAL,
                                    headers: {
                                        'X-M2M-RI': String(parseInt(Math.random() * 10000)),
                                        'X-M2M-Origin': 'SVue',
                                        'Content-Type': 'application/json;ty=3'
                                    },
                                    data: {
                                        'm2m:cnt': {
                                            rn: 'approval',
                                            lbl: ['approval'],
                                        }
                                    }
                                }).then(
                                    function () {
                                        self.MOBIUS_CONNECTION_CONNECTED = true;
                                        self.$store.state.MOBIUS_CONNECTION_CONNECTED = true;

                                        localStorage.setItem('mobius_connected', self.MOBIUS_CONNECTION_CONNECTED);
                                        axios({
                                            validateStatus: function (status) {
                                                // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                                                return status < 500;
                                            },
                                            method: 'get',
                                            url: 'http://' + self.$store.state.VUE_APP_MOBIUS_HOST + ':7579/Mobius/' + self.$store.state.VUE_APP_MOBIUS_APPROVAL + '/approval',
                                            params: {
                                                'fu': '1',
                                                'lvl': '1',
                                                'ty': '3',
                                            },
                                            headers: {
                                                'Accept': 'application/json',
                                                'X-M2M-RI': '12345',
                                                'X-M2M-Origin': 'SOrigin'
                                            }
                                        }).then(
                                            function (response) {
                                                let list = response.data["m2m:uril"];
                                                for (let idx in list) {
                                                    let id = list[idx].split('/')
                                                    self.approval_list.push(id[id.length - 1])
                                                }
                                                self.$store.state.APPROVAL_LIST = self.approval_list
                                                EventBus.$emit('do-updated-approval-list', self.$store.state.APPROVAL_LIST);
                                            }
                                        ).catch(
                                            function (error) {
                                                console.log(error);
                                            }
                                        );
                                    }
                                ).catch(
                                    function (err) {
                                        console.log(err.message);
                                    }
                                );
                            }
                        ).catch(
                            function (err) {
                                console.log(err.message);
                            }
                        );
                    }
                }
            ).catch(
                function (err) {
                    console.log(err.message);
                }
            );
        },

        GcsAppBarReseted() {
            this.MOBIUS_CONNECTION_CONNECTED = false;
            this.$store.state.MOBIUS_CONNECTION_CONNECTED = false;
            this.approval_list = []

            localStorage.setItem('mobius_connected', this.MOBIUS_CONNECTION_CONNECTED);
        },
    },

    mounted() {
        if (this.MOBIUS_CONNECTION_CONNECTED) {
            this.GcsAppBarCreated();
        }
    },

    beforeDestroy() {
        this.GcsAppBarReseted()
    }
}
</script>

<style scoped>
#create .v-speed-dial {
    position: absolute;
}

#create .v-btn--floating {
    position: relative;
}
</style>
