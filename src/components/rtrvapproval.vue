<template>
  <v-container>
    <h2 class="text-left font-weight-black" color="#0D47A1">Retrieve Approval</h2>
    <br /><br /><br />

    <v-row>
      <v-col>
        <label class="font-weight-bold mt-n2">HOST :</label>
      </v-col>
      <v-col>
        <label class="font-weight-bold mt-n2">APPROVAL :</label>
      </v-col>
      <v-col> </v-col>
      <v-col> </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-text-field
          class="mx-0 mt-1"
          dense
          hide-details
          outlined
          ref="host"
          v-model="host"
          :rules="host_rule"
          placeholder="gcs.iotocean.org"
          required
          :disabled="MOBIUS_CONNECTION_CONNECTED"
        ></v-text-field>
      </v-col>

      <v-col>
        <v-text-field
          class="mx-0 mt-1"
          dense
          hide-details
          outlined
          ref="approval"
          v-model="approval"
          :rules="approval_rule"
          placeholder="MUV"
          required
          :disabled="MOBIUS_CONNECTION_CONNECTED"
        ></v-text-field>
      </v-col>

      <v-col>
        <v-btn
          class="ma-1"
          @click="GcsAppBarCreated"
          elevation="1"
          color="#64B5F6"
          :disabled="MOBIUS_CONNECTION_CONNECTED"
          block
          outlined
          raised
          rounded
          v-on:click="ae_form"
        >
          {{ MOBIUS_CONNECTION_TEXT }}
        </v-btn>
      </v-col>

      <v-col>
        <v-btn
          class="ma-1"
          @click="GcsAppBarReseted"
          elevation="1"
          color="#64B5F6"
          :disabled="MOBIUS_CONNECTION_DISABLED"
          block
          outlined
          raised
          rounded
          v-on:click="ae_form"
        >
          {{ MOBIUS_DISCONNECTION_TEXT }}
        </v-btn>
      </v-col>
    </v-row>

    <br /><br />


      <v-row>

        <label class="font-weight-bold mt-n2">DRONE ID :</label>

          <v-col>
            <v-text-field
              class="mx-0 mt-1"
              dense
              outlined
              ref="drone_ID"
              v-model="drone_id"
              :rules="drone_id_rule"
              label="* Drone ID"
              hint="Unique ID of Drone"
              persistent-hint
              required
            ></v-text-field>
          </v-col>

           
              <v-col>
                <v-btn
                  class="ma-1"
                  @click="IDCheck"
                  elevation="1"
                  color="#64B5F6"
                  :disabled="drone_id === null || drone_id === ''"
                  block
                  outlined
                  raised
                  rounded
                >
                  CREATE
                </v-btn>
              </v-col>
            </v-row>
            

        <br /><br />

<v-row>
<v-col>
        <v-row class="pt-4"></v-row>
        <h4 class="font-weight-black"> ID Description </h4>
        <br />

        <label class="font-weight-bold mt-n2">HOST :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="drone_host"
              v-model="drone_host"
              :rules="drone_host_rule"
              placeholder="125.132.13.63"
              label="* GCS Host"
              hint="예시) gcs.iotocean.org"
              persistent-hint
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label class="font-weight-bold mt-n2"> DRONE :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="drone_Name"
              v-model="drone_name"
              :rules="drone_name_rule"
              placeholder="KETI_UAV"
              label="* Drone Name (e.g. KETI_UAV_1)"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label class="font-weight-bold mt-n2">GCS :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="gcs_name"
              v-model="gcs_name"
              :rules="gcs_name_rule"
              placeholder="KETI_MUV"
              label="* GCS Name (e.g. KETI_GCS)"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label class="font-weight-bold mt-n2">TYPE :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="drone_type"
              v-model="drone_type"
              :rules="drone_type_rule"
              :items="['ardupilot', 'px4', 'dji', 'etc']"
              placeholder="pixhawk"
              label="* FC Type (e.g. ardupilot or px4)"
              hint="Type of FC"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label class="font-weight-bold mt-n2">SYSTEM ID :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="system_id"
              v-model="system_id"
              :rules="system_id_rule"
              label="* System ID (e.g. 255)"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label class="font-weight-bold mt-n2">MAV_VER :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="mav_ver"
              v-model="mav_ver"
              :rules="mav_ver_rule"
              label="* Mav_ver (e.g. v1)"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label class="font-weight-bold mt-n2">UPDATE :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="update"
              v-model="update"
              :rules="update_rule"
              label="* Update (e.g. disable)"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <!-- mission START -->

        <label class="font-weight-bold mt-n2">MISSION NAME :</label>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="mission_name"
              v-model="mission_name"
              :rules="mission_name_rule"
              label="* mission name (e.g. ??)"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label v-if="mission_name !== null" class="font-weight-bold mt-n2"
          >mission container:</label
        >
        <v-row v-if="mission_name !== null" no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="mission_container"
              v-model="mission_container"
              :rules="mission_container_rule"
              label="* mission container"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label v-if="mission_name !== null" class="font-weight-bold mt-n2"
          >mission sub_container:</label
        >
        <v-row v-if="mission_name !== null" no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="mission_subcontainer"
              v-model="mission_subcontainer"
              :rules="mission_subcontainer_rule"
              label="* mission sub_container"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label v-if="mission_name !== null" class="font-weight-bold mt-n2"
          >mission URL:</label
        >
        <v-row v-if="mission_name !== null" no-gutters>
          <v-col>
            <v-text-field
              class="mr-2 mb-n2"
              dense
              outlined
              ref="mission_git"
              v-model="mission_git"
              :rules="mission_git_rule"
              label="* mission git URL"
              required
              persistent-hint
            ></v-text-field>
          </v-col>
        </v-row>

        <v-col>
          <b-btn
            class="ml-4 mb-n2"
            tile
            @click="AddMission"
            :disabled="mission_name === null"
            elevation="5"
            color="primary"
          >
            Add Mission
          </b-btn>
          <b-btn
            class="mr-6 mb-n2"
            tile
            @click="DeleteMission"
            :disabled="!add_mission_flag"
            elevation="5"
            color="primary"
          >
            Delte Mission
          </b-btn>
        </v-col>
      </v-col>
    </v-row>


    <v-row class="mt-2">
      <v-col no-gutters>
        <v-col>
          <hr color="dimgrey" />
          <label v-if="add_mission_flag" class="font-weight-bold"
            >Mission List</label
          >
        </v-col>
        <v-col class="mt-n4">
          <v-data-table
            v-if="add_mission_flag"
            v-model="mission_selected"
            :headers="mission_col"
            :items="mission_data"
            item-key="mission_name"
            class="elevation-1"
            hide-default-footer
          >
            <template v-slot:item="{ item }">
              <tr
                :class="
                  mission_selected.indexOf(item.mission_name) > -1 ? 'grey' : ''
                "
                @click="rowClicked(item)"
              >
                <td>{{ item.mission_name }}</td>
                <td>{{ item.mission_container }}</td>
                <td>{{ item.mission_subcontainer }}</td>
                <td>{{ item.mission_url }}</td>
              </tr>
            </template>
          </v-data-table>
        </v-col>
        <v-col>
          <v-btn
            class="mr-2 mb-n2 font-weight-bold"
            tile
            @click="SendApproval"
            :disabled="drone_id === null || drone_id === ''"
            elevation="5"
            color="success"
          >
            Send
          </v-btn>
        </v-col>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
import EventBus from "@/EventBus";

export default {
  data: function () {
    return {
      //app.vue start

      open: false,
      MOBIUS_DISCONNECTION_TEXT: "Disconnect",
      MOBIUS_CONNECTION_TEXT: "Connect",

      MOBIUS_CONNECTION_CONNECTED: localStorage.getItem("mobius_connected")
        ? localStorage.getItem("mobius_connected") === "true"
        : false,
      MOBIUS_CONNECTION_DISABLED: false,

      host: localStorage.getItem("mobius_host")
        ? localStorage.getItem("mobius_host")
        : this.$store.state.VUE_APP_MOBIUS_HOST,
      host_rule: [
        (v) => !!v || "호스트 주소는 필수 입력사항입니다.",
        //   v => /^[.0-9]*$/.test(v) || '호스트 주소는 숫자만 입력 가능합니다.'
      ],
      approval: localStorage.getItem("mobius_approval")
        ? localStorage.getItem("mobius_approval")
        : this.$store.state.VUE_APP_MOBIUS_APPROVAL,
      approval_rule: [
        (v) => !!v || "Approval 이름은 필수 입력사항입니다.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "Approval 이름에는 특수문자를 사용할 수 없습니다.",
      ],
      approval_list: [],

      //app.vue end

      drone_id: null,
      drone_id_rule: [
        (v) => !!v || "드론 아이디는 필수 입력사항입니다.",
        (v) =>
          /^[a-zA-Z0-9]*$/.test(v) ||
          "드론 아이디는 영문+숫자만 입력 가능합니다.",
      ],
      drone_host: null,
      drone_host_rule: [
        (v) => !!v || "호스트 주소는 필수 입력사항입니다.",
        (v) => /^[.0-9]*$/.test(v) || "호스트 주소는 숫자만 입력 가능합니다.",
      ],
      drone_name: null,
      drone_name_rule: [
        (v) => !!v || "드론 이름은 필수 입력사항입니다.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "드론 이름에는 특수문자를 사용할 수 없습니다.",
      ],
      gcs_name: null,
      gcs_name_rule: [
        (v) => !!v || "GCS 이름은 필수 입력사항입니다.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "GCS 이름에는 특수문자를 사용할 수 없습니다.",
      ],
      drone_type: null,
      drone_type_rule: [
        (v) => !!v || "FC 타입은 필수 입력사항입니다.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "FC 타입에는 특수문자를 사용할 수 없습니다.",
      ],
      system_id: null,
      system_id_rule: [
        (v) => !!v || "드론 시스템 아이디는 필수 입력사항입니다.",
        (v) =>
          /^[0-9]*$/.test(v) || "드론 시스템 아이디는 숫자만 입력 가능합니다.",
        (v) => !(v > 254) || "드론 시스템 아이디는 255를 넘을 수 없습니다.",
        (v) =>
          !(v <= 0) || "드론 시스템 아이디는 1 이상의 수만 입력 가능합니다.",
      ],
      mav_ver: null,
      mav_ver_rule: [
        (v) => !!v || "MAVLink 버전은 필수 입력사항입니다.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "MAVLink 버전은 특수문자를 사용할 수 없습니다.",
      ],
      update: null,
      update_rule: [
        (v) => !!v || "소스코드 업데이트 여부는 필수 입력사항입니다.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "소스코드 업데이트 여부에는 특수문자를 사용할 수 없습니다.",
      ],

      mission_name: null,
      mission_name_rule: [
        (v) => !!v || "임무 이름은 필수 입력사항입니다.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "임무 이름에는 특수문자를 사용할 수 없습니다.",
      ],
      mission_container: null,
      mission_container_rule: [
        // v => !!v || '임무 데이터 이름은 필수 입력사항입니다.',
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "임무 데이터 이름에는 특수문자를 사용할 수 없습니다.",
      ],
      mission_subcontainer: null,
      mission_subcontainer_rule: [
        // v => !!v || '임무 제어 이름은 필수 입력사항입니다.',
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "임무 제어 이름에는 특수문자를 사용할 수 없습니다.",
      ],
      mission_git: null,
      mission_git_rule: [
        (v) => !!v || "임무 URL 이름은 필수 입력사항입니다.",
        // v => !/[~!@#$%^&*()+|<>?:{}]/.test(v) || '임무 URL 이름에는 특수문자를 사용할 수 없습니다.',
        // v => !(v.split('/').length !== 5) || '임무 URL이 잘못되었습니다.'
      ],
      mission: {},

      approval_selected: "",

      create_btn: false,

      header: [
        {
          text: "Column1",
          align: "end",
          sortable: false,
          value: "value1",
        },
        {
          text: "Column2",
          align: "start",
          sortable: false,
          value: "value2",
        },
      ],
      response: [],

      mission_header: [
        {
          text: "Column1",
          align: "end",
          sortable: false,
          value: "value1",
        },
        {
          text: "Column2",
          align: "start",
          sortable: false,
          value: "value2",
        },
      ],

      mission_response: [[]],

      retrieve_approval_flag: false,
      retrieve_mission_flag: false,

      res_length: 0,

      add_mission_flag: false,
      mission_col: [
        {
          text: "mission_name",
          align: "center",
          sortable: false,
          value: "mission_name",
        },
        {
          text: "mission_container",
          align: "center",
          sortable: false,
          value: "mission_container",
        },
        {
          text: "mission_subcontainer",
          align: "center",
          sortable: false,
          value: "mission_subcontainer",
        },
        {
          text: "mission_url",
          align: "center",
          sortable: false,
          value: "mission_url",
        },
      ],
      mission_data: [],
      mission_selected: [],
    };
  },

  methods: {
    //app.vue start
    GcsAppBarCreated() {
      this.$store.state.VUE_APP_MOBIUS_HOST = this.host;
      this.$store.state.VUE_APP_MOBIUS_APPROVAL = this.approval;

      localStorage.setItem("mobius_host", this.host);
      localStorage.setItem("mobius_approval", this.approval);

      let self = this;

      axios({
        validateStatus: function (status) {
          // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
          return status < 500;
        },
        method: "get",
        url:
          "http://" +
          this.$store.state.VUE_APP_MOBIUS_HOST +
          ":7579/Mobius/" +
          this.$store.state.VUE_APP_MOBIUS_APPROVAL,
        headers: {
          "X-M2M-RI": String(parseInt(Math.random() * 10000)),
          "X-M2M-Origin": "SVue",
          "Content-Type": "application/json",
        },
      })
        .then(function (res) {
          console.log(
            "ApprovalAppBarCreated",
            "http://" +
              self.$store.state.VUE_APP_MOBIUS_HOST +
              ":7579/Mobius/" +
              self.$store.state.VUE_APP_MOBIUS_APPROVAL,
            res
          );

          if (res.status === 200) {
            self.MOBIUS_CONNECTION_CONNECTED = true;
            self.$store.state.MOBIUS_CONNECTION_CONNECTED = true;

            localStorage.setItem(
              "mobius_connected",
              self.MOBIUS_CONNECTION_CONNECTED
            );

            axios({
              validateStatus: function (status) {
                // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                return status < 500;
              },
              method: "get",
              url:
                "http://" +
                self.$store.state.VUE_APP_MOBIUS_HOST +
                ":7579/Mobius/" +
                self.$store.state.VUE_APP_MOBIUS_APPROVAL +
                "/approval",
              params: {
                fu: "1",
                lvl: "1",
                ty: "3",
              },
              headers: {
                Accept: "application/json",
                "X-M2M-RI": "12345",
                "X-M2M-Origin": "SOrigin",
              },
            })
              .then(function (response) {
                let list = response.data["m2m:uril"];
                for (let idx in list) {
                  let id = list[idx].split("/");
                  self.approval_list.push(id[id.length - 1]);
                }
                self.$store.state.APPROVAL_LIST = self.approval_list;
                EventBus.$emit(
                  "do-updated-approval-list",
                  self.$store.state.APPROVAL_LIST
                );
              })
              .catch(function (error) {
                console.log(error);
              });
          } else if (res.status === 404) {
            axios({
              validateStatus: function (status) {
                // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                return status < 500;
              },
              method: "post",
              url:
                "http://" +
                self.$store.state.VUE_APP_MOBIUS_HOST +
                ":7579/Mobius",
              headers: {
                "X-M2M-RI": String(parseInt(Math.random() * 10000)),
                "X-M2M-Origin": "S" + self.$store.state.VUE_APP_MOBIUS_APPROVAL,
                "Content-Type": "application/json;ty=2",
              },
              data: {
                "m2m:ae": {
                  rn: self.$store.state.VUE_APP_MOBIUS_APPROVAL,
                  api: "0.2.481.1.1111",
                  lbl: [self.$store.state.VUE_APP_MOBIUS_APPROVAL],
                  rr: true,
                  poa: ["http://localhost:8080"],
                },
              },
            })
              .then(function () {
                axios({
                  validateStatus: function (status) {
                    // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                    return status < 500;
                  },
                  method: "post",
                  url:
                    "http://" +
                    self.$store.state.VUE_APP_MOBIUS_HOST +
                    ":7579/Mobius/" +
                    self.$store.state.VUE_APP_MOBIUS_APPROVAL,
                  headers: {
                    "X-M2M-RI": String(parseInt(Math.random() * 10000)),
                    "X-M2M-Origin": "SVue",
                    "Content-Type": "application/json;ty=3",
                  },
                  data: {
                    "m2m:cnt": {
                      rn: "approval",
                      lbl: ["approval"],
                    },
                  },
                })
                  .then(function () {
                    self.MOBIUS_CONNECTION_CONNECTED = true;
                    self.$store.state.MOBIUS_CONNECTION_CONNECTED = true;

                    localStorage.setItem(
                      "mobius_connected",
                      self.MOBIUS_CONNECTION_CONNECTED
                    );
                    axios({
                      validateStatus: function (status) {
                        // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                        return status < 500;
                      },
                      method: "get",
                      url:
                        "http://" +
                        self.$store.state.VUE_APP_MOBIUS_HOST +
                        ":7579/Mobius/" +
                        self.$store.state.VUE_APP_MOBIUS_APPROVAL +
                        "/approval",
                      params: {
                        fu: "1",
                        lvl: "1",
                        ty: "3",
                      },
                      headers: {
                        Accept: "application/json",
                        "X-M2M-RI": "12345",
                        "X-M2M-Origin": "SOrigin",
                      },
                    })
                      .then(function (response) {
                        let list = response.data["m2m:uril"];
                        for (let idx in list) {
                          let id = list[idx].split("/");
                          self.approval_list.push(id[id.length - 1]);
                        }
                        self.$store.state.APPROVAL_LIST = self.approval_list;
                        EventBus.$emit(
                          "do-updated-approval-list",
                          self.$store.state.APPROVAL_LIST
                        );
                      })
                      .catch(function (error) {
                        console.log(error);
                      });
                  })
                  .catch(function (err) {
                    console.log(err.message);
                  });
              })
              .catch(function (err) {
                console.log(err.message);
              });
          }
        })
        .catch(function (err) {
          console.log(err.message);
        });
    },

    GcsAppBarReseted() {
      this.MOBIUS_CONNECTION_CONNECTED = false;
      this.$store.state.MOBIUS_CONNECTION_CONNECTED = false;
      this.approval_list = [];

      localStorage.setItem(
        "mobius_connected",
        this.MOBIUS_CONNECTION_CONNECTED
      );
    },
    //app.vue end

    IDCheck() {
      let self = this;

      axios({
        validateStatus: function (status) {
          // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
          return status < 500;
        },
        method: "get",
        url:
          "http://" +
          this.$store.state.VUE_APP_MOBIUS_HOST +
          ":7579/Mobius/" +
          this.$store.state.VUE_APP_MOBIUS_APPROVAL +
          "/approval/" +
          this.drone_id,
        headers: {
          "X-M2M-RI": String(parseInt(Math.random() * 10000)),
          "X-M2M-Origin": "SVue",
          "Content-Type": "application/json",
        },
      })
        .then(function (res) {
          console.log(
            "Retrieve drone ID -",
            "http://" +
              self.$store.state.VUE_APP_MOBIUS_HOST +
              ":7579/Mobius/" +
              self.$store.state.VUE_APP_MOBIUS_APPROVAL +
              "/approval/" +
              self.drone_id,
            res
          );

          if (res.status === 200) {
            console.log("resource is already exist");
            self.create_btn = true;
          } else if (res.status === 404) {
            axios({
              validateStatus: function (status) {
                // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
                return status < 500;
              },
              method: "post",
              url:
                "http://" +
                self.$store.state.VUE_APP_MOBIUS_HOST +
                ":7579/Mobius/" +
                self.$store.state.VUE_APP_MOBIUS_APPROVAL +
                "/approval",
              headers: {
                "X-M2M-RI": String(parseInt(Math.random() * 10000)),
                "X-M2M-Origin": "SVue",
                "Content-Type": "application/json;ty=3",
              },
              data: {
                "m2m:cnt": {
                  rn: self.drone_id,
                  lbl: [self.drone_id],
                },
              },
            })
              .then(function () {
                self.$store.state.APPROVAL_LIST.push(self.drone_id);
                console.log("Enter approval description");
                self.create_btn = true;
              })
              .catch(function (err) {
                console.log(err.message);
              });
          }
        })
        .catch(function (err) {
          console.log(err.message);
        });
    },
    SendApproval() {
      let self = this;

      let con = {
        host: self.drone_host,
        drone: self.drone_name,
        gcs: self.gcs_name,
        type: self.drone_type,
        system_id: self.system_id,
        mav_ver: self.mav_ver,
        update: self.update,
        mission: self.mission,
      };

      if (Object.getOwnPropertyNames(self.mission).length <= 1) {
        delete con["mission"];
      }

      axios({
        validateStatus: function (status) {
          // 상태 코드가 500 이상일 경우 거부. 나머지(500보다 작은)는 허용.
          return status < 500;
        },
        method: "post",
        url:
          "http://" +
          self.$store.state.VUE_APP_MOBIUS_HOST +
          ":7579/Mobius/" +
          self.$store.state.VUE_APP_MOBIUS_APPROVAL +
          "/approval/" +
          self.drone_id,
        headers: {
          "X-M2M-RI": String(parseInt(Math.random() * 10000)),
          "X-M2M-Origin": "SVue",
          "Content-Type": "application/json;ty=4",
        },
        data: {
          "m2m:cin": {
            con: con,
          },
        },
      })
        .then(function (res) {
          console.log(res.data["m2m:cin"].con);
        })
        .catch(function (err) {
          console.log(err.message);
        });
    },
    RetrieveApproval() {
      let self = this;
      self.mission_response = [[]];
      axios({
        method: "get",
        url:
          "http://" +
          self.$store.state.VUE_APP_MOBIUS_HOST +
          ":7579/Mobius/" +
          self.$store.state.VUE_APP_MOBIUS_APPROVAL +
          "/approval/" +
          self.approval_selected +
          "/la",
        headers: {
          "X-M2M-RI": String(parseInt(Math.random() * 10000)),
          "X-M2M-Origin": "SVue",
          "Content-Type": "application/json;ty=4",
        },
      })
        .then(function (res) {
          let res_con = res.data["m2m:cin"].con;
          self.response = [
            {
              value1: "host",
              value2: res_con.host,
            },
            {
              value1: "gcs",
              value2: res_con.gcs,
            },
            {
              value1: "drone",
              value2: res_con.drone,
            },
            {
              value1: "type",
              value2: res_con.type,
            },
            {
              value1: "system_id",
              value2: res_con.system_id,
            },
            {
              value1: "mav_ver",
              value2: res_con.mav_ver,
            },
            {
              value1: "update",
              value2: res_con.update,
            },
          ];
          self.retrieve_approval_flag = true;
          if (Object.hasOwnProperty.call(res_con, "mission")) {
            self.res_length = Object.getOwnPropertyNames(
              res_con.mission
            ).length;
            for (let idx = 0; idx < self.res_length; idx++) {
              let res = [
                {
                  value1: "mission name",
                  value2: Object.getOwnPropertyNames(res_con.mission)[idx],
                },
                {
                  value1: "container",
                  value2:
                    res_con.mission[
                      Object.getOwnPropertyNames(res_con.mission)[idx]
                    ].container,
                },
                {
                  value1: "sub_container",
                  value2:
                    res_con.mission[
                      Object.getOwnPropertyNames(res_con.mission)[idx]
                    ].sub_container,
                },
                {
                  value1: "git",
                  value2:
                    res_con.mission[
                      Object.getOwnPropertyNames(res_con.mission)[idx]
                    ].git,
                },
              ];
              self.mission_response.push(res);
            }
            self.retrieve_mission_flag = true;
          } else {
            self.retrieve_mission_flag = false;
          }
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    DeleteMission() {
      for (let select = this.mission_selected.length; select > 0; select--) {
        for (let idx = this.mission_data.length; idx > 0; idx--) {
          if (
            this.mission_data[idx - 1].mission_name ===
            this.mission_selected[select - 1]
          ) {
            delete this.mission[this.mission_selected[select - 1]];
            this.mission_data.splice(idx - 1, 1);
            this.mission_selected.splice(select - 1, 1);
          }
        }
      }
    },
    AddMission() {
      let self = this;

      let mission = {
        mission_name: self.mission_name,
        mission_container: self.mission_container,
        mission_subcontainer: self.mission_subcontainer,
        mission_url: self.mission_git,
      };
      self.mission_data.push(mission);
      self.add_mission_flag = true;

      self.mission[self.mission_name] = {
        container: [self.mission_container],
        sub_container: [self.mission_subcontainer],
        git: self.mission_git,
      };
      if (
        self.mission[self.mission_name].container[0] === "" ||
        self.mission[self.mission_name].container[0] === null
      ) {
        self.mission[self.mission_name].container = [];
      }
      if (
        self.mission[self.mission_name].sub_container[0] === "" ||
        self.mission[self.mission_name].sub_container[0] === null
      ) {
        self.mission[self.mission_name].sub_container = [];
      }
      if (
        self.mission[self.mission_name].git[0] === "" ||
        self.mission[self.mission_name].git[0] === null
      ) {
        self.mission[self.mission_name].git = [];
      }
    },
    rowClicked(row) {
      if (this.mission_selected.includes(row.mission_name)) {
        this.mission_selected = this.mission_selected.filter(
          (selectedKeyID) => selectedKeyID !== row.mission_name
        );
        // delete this.mission[row.mission_name]
      } else {
        this.mission_selected.push(row.mission_name);
      }
    },
  },

  created() {
    EventBus.$on("do-updated-approval-list", () => {
      this.approval_list = this.$store.state.APPROVAL_LIST;
      console.log(this.approval_list);

      this.$forceUpdate();
    });
  },
  beforeDestroy() {
    this.create_btn = false;
    this.retrieve_approval_flag = false;
    this.retrieve_mission_flag = false;
    this.add_mission_flag = false;
    this.GcsAppBarReseted();
  },

  mounted() {
    if (this.MOBIUS_CONNECTION_CONNECTED) {
      this.GcsAppBarCreated();
    }
  },
};
</script>
