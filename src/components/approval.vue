<template>
  <div class="background">
    <v-container>
      <br />
      <h4 class="h4 text-center font-weight-black">
        CREATE APPROVAL
      </h4>
      <br /><br />

      <!-- original -->

      <v-card class="pa-7" outlined shaped elevation="3">
        <v-row>
        <h5 class="cardtitle font-weight-black">DRONE Server Connection</h5>
        <br><br>
          <v-col>
            <p class="font-weight-bold">HOST :</p>
            <v-text-field
              class="mx-0 mt-0"
              dense
              hide-details
              shaped
              filled
              ref="host"
              v-model="host"
              :rules="host_rule"
              placeholder="gcs.iotocean.org"
              required
              :disabled="MOBIUS_CONNECTION_CONNECTED"
            ></v-text-field>
          </v-col>
          <v-col>
            <p class="font-weight-bold">APPROVAL :</p>
            <v-text-field
              class="mx-0 mt-0"
              dense
              shaped
              hide-details
              filled
              ref="approval"
              v-model="approval"
              :rules="approval_rule"
              placeholder="MUV"
              required
              :disabled="MOBIUS_CONNECTION_CONNECTED"
            ></v-text-field>
          </v-col>
        </v-row>


      <v-row>
        <v-col></v-col>
        <v-col cols="3">
          <v-btn
            class="mt-2"
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
        <v-col cols="3">
          <v-btn
            class="mt-2"
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
        <v-col></v-col>
      </v-row>
      </v-card>
      
      <br /><br />

      <v-card class="pa-7" outlined shaped elevation="3">
        <v-row>
        <h5 class="cardtitle font-weight-black">DRONE ID</h5>
        <br>
        <br>

          <v-col>
            <v-text-field
              class="mr-2 mb-n2 pb-3"
              dense
              filled
              shaped
              ref="drone_ID"
              v-model="drone_id"
              :rules="drone_id_rule"
              label="* Drone ID"
              hint="Unique ID of Drone"
              persistent-hint
              required
            ></v-text-field>
          </v-col>

          <v-col cols="3">
            <v-btn
              class="ma-3"
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
      </v-card>

      <br /><br />

      <v-card class="pa-7" outlined shaped elevation="3">
<v-row>
        <h5 class="cardtitle font-weight-black">DRONE Specification</h5>
        <br><br>
        <v-col>
           <label class="font-weight-bold mt-n2">HOST :</label>
          <v-text-field
            class="mr-2 mb-n2 pb-3"
            dense
            filled
            shaped
            ref="drone_host"
            v-model="drone_host"
            :rules="drone_host_rule"
            placeholder="125.132.13.63"
            label="* GCS Host"
            hint="e.g.) gcs.iotocean.org"
            persistent-hint
            required
          ></v-text-field>
        </v-col>

        <v-col>
          <label class="font-weight-bold mt-n2"> DRONE NAME :</label>
          <v-text-field
            class="mr-2 mb-n2 pb-3"
            dense
            filled
            shaped
            ref="drone_Name"
            v-model="drone_name"
            :rules="drone_name_rule"
            placeholder="KETI_UAV"
            label="* Drone Name (e.g. KETI_UAV_1)"
            required
          ></v-text-field>
        </v-col>
</v-row> 

<v-row>
        <v-col>
          <label class="font-weight-bold mt-n2">GCS :</label>
          <v-text-field
            class="mr-2 mb-n2 pb-3"
            dense
            filled
            shaped
            ref="gcs_name"
            v-model="gcs_name"
            :rules="gcs_name_rule"
            placeholder="KETI_MUV"
            label="* GCS Name (e.g. KETI_GCS)"
            required
          ></v-text-field>
        </v-col>

        <v-col>
          <label class="font-weight-bold mt-n2">TYPE :</label>
          <v-text-field
            class="mr-2 mb-n2"
            dense
            filled
            shaped
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

<v-row>
        <v-col>
          <label class="font-weight-bold mt-n2">SYSTEM ID :</label>
          <v-text-field
            class="mr-2 mb-n2"
            dense
            filled
            shaped
            ref="system_id"
            v-model="system_id"
            :rules="system_id_rule"
            label="* System ID (e.g. 255)"
            required
          ></v-text-field>
        </v-col>

       

        <v-col>
           <label class="font-weight-bold mt-n2">MAVLink VERSION :</label>
          <v-text-field
            class="mr-2 mb-n2"
            dense
            filled
            shaped
            ref="mav_ver"
            v-model="mav_ver"
            :rules="mav_ver_rule"
            label="* Mav_ver (e.g. v1)"
            required
          ></v-text-field>
        </v-col>
</v-row>

<v-row>
        <v-col>
          <label class="font-weight-bold mt-n2">UPDATE :</label>
          <v-text-field
            class="mr-2 mb-n2"
            dense
            filled
            shaped
            ref="update"
            v-model="update"
            :rules="update_rule"
            label="* Update (e.g. disable)"
            required
          ></v-text-field>
        </v-col>

       

        <v-col>
          <label class="font-weight-bold mt-n2">BOARD VERSION :</label>
          <v-text-field
            class="mr-2 mb-n2"
            dense
            filled
            shaped
            ref="board_ver"
            v-model="board_ver"
            :rules="board_ver_rule"
            label="* board version (e.g. Crow-A or Crow-B or Crow-C)"
            required
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <label class="font-weight-bold mt-n2">LTE TYPE :</label>
          <v-text-field
            class="mr-2 mb-n2"
            dense
            filled
            shaped
            ref="lte_type"
            v-model="lte_type"
            :rules="lte_type_rule"
            label="* LTE type (e.g. AMT or QTEL)"
            required
          ></v-text-field>
        </v-col>
        <v-col>
          </v-col>
        </v-row>
      </v-card>
      <br /><br />
      
      <!-- mission START -->

      <v-card class="pa-7" outlined shaped elevation="3">
        <v-row>
        <h5 class="cardtitle font-weight-black">Mission Information</h5>
        <br><br>
          <v-col>
            <p class="font-weight-bold">MISSION NAME :</p>

            <v-select
          :items="missions"
          ref="mission_name"
          v-model="mission_name"
          :rules="mission_name_rule"
          filled
          shaped
          label="Select mission"
          required
        ></v-select>
       
       <!--
          <v-text-field
          class="mr-2 mb-n2"
          dense
          filled
          shaped
          ref="mission_name"
          v-model="mission_name"
          :rules="mission_name_rule"
          label="* mission name (e.g. ??)"
          required
        ></v-text-field>
        -->

      </v-col>
 

      <label v-if="mission_name !== null" class="font-weight-bold mt-n2"
        >mission container:</label
      >
      <v-row v-if="mission_name !== null" no-gutters>
        <v-col>
          <v-text-field
            class="mr-2 mb-n2"
            dense
            filled
            shaped
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
            filled
            shaped
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
            filled
            shaped
            ref="mission_git"
            v-model="mission_git"
            :rules="mission_git_rule"
            label="* mission git URL"
            required
            persistent-hint
          ></v-text-field>
        </v-col>
      </v-row>



      <v-row>
        <v-col></v-col>
        <v-col cols="3">
          <v-btn
            class="mt-2"
            @click="AddMission"
            elevation="1"
            color="#64B5F6"
            :disabled="mission_name === null"
            block
            outlined
            raised
            rounded
          >
          Add Mission
          </v-btn>
        </v-col>
        <v-col cols="3">
          <v-btn
            class="mt-2"
            @click="DeleteMission"
            elevation="1"
            color="#64B5F6"
            :disabled="!add_mission_flag"
            block
            outlined
            raised
            rounded
            v-on:click="ae_form"
          >
           Delete Mission
          </v-btn>
        </v-col>
        <v-col></v-col>
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
                    mission_selected.indexOf(item.mission_name) > -1
                      ? 'grey'
                      : ''
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
          

        </v-col>

        
      </v-row>
      
        </v-row>

        
        </v-card>

        <br><br>
        <v-row>
          <v-col>
          <v-btn
            class="mr-2 mb-n2 font-weight-bold"
            @click="SendApproval"
            elevation="1"
            color="#64B5F6"
            :disabled="drone_id === null || drone_id === ''"
            block
            outlined
            raised
            rounded
          >
            SEND
          </v-btn>
          </v-col>

        </v-row>
          <br><br><br>
    </v-container>
  </div>
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
        (v) => !!v || "????????? ????????? ?????? ?????????????????????.",
        //   v => /^[.0-9]*$/.test(v) || '????????? ????????? ????????? ?????? ???????????????.'
      ],
      approval: localStorage.getItem("mobius_approval")
        ? localStorage.getItem("mobius_approval")
        : this.$store.state.VUE_APP_MOBIUS_APPROVAL,
      approval_rule: [
        (v) => !!v || "Approval ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "Approval ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      approval_list: [],

      //app.vue end

      drone_id: null,
      drone_id_rule: [
        (v) => !!v || "?????? ???????????? ?????? ?????????????????????.",
        (v) =>
          /^[a-zA-Z0-9]*$/.test(v) ||
          "?????? ???????????? ??????+????????? ?????? ???????????????.",
      ],
      drone_host: null,
      drone_host_rule: [
        (v) => !!v || "????????? ????????? ?????? ?????????????????????.",
        (v) => /^[.0-9]*$/.test(v) || "????????? ????????? ????????? ?????? ???????????????.",
      ],
      drone_name: null,
      drone_name_rule: [
        (v) => !!v || "?????? ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "?????? ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      gcs_name: null,
      gcs_name_rule: [
        (v) => !!v || "GCS ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "GCS ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      drone_type: null,
      drone_type_rule: [
        (v) => !!v || "FC ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "FC ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      system_id: null,
      system_id_rule: [
        (v) => !!v || "?????? ????????? ???????????? ?????? ?????????????????????.",
        (v) =>
          /^[0-9]*$/.test(v) || "?????? ????????? ???????????? ????????? ?????? ???????????????.",
        (v) => !(v > 254) || "?????? ????????? ???????????? 255??? ?????? ??? ????????????.",
        (v) =>
          !(v <= 0) || "?????? ????????? ???????????? 1 ????????? ?????? ?????? ???????????????.",
      ],
      mav_ver: null,
      mav_ver_rule: [
        (v) => !!v || "MAVLink ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "MAVLink ????????? ??????????????? ????????? ??? ????????????.",
      ],
      update: null,
      update_rule: [
        (v) => !!v || "???????????? ???????????? ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "???????????? ???????????? ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      board_ver: null,
      board_ver_rule: [
        (v) => !!v || "??????????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "?????? ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      lte_type: null,
      lte_type_rule: [
        (v) => !!v || "LTE ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "LTE ???????????? ??????????????? ????????? ??? ????????????.",
      ],

      mission_name: null,
      mission_name_rule: [
        (v) => !!v || "?????? ????????? ?????? ?????????????????????.",
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "?????? ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      mission_container: null,
      mission_container_rule: [
        // v => !!v || '?????? ????????? ????????? ?????? ?????????????????????.',
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "?????? ????????? ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      mission_subcontainer: null,
      mission_subcontainer_rule: [
        // v => !!v || '?????? ?????? ????????? ?????? ?????????????????????.',
        (v) =>
          !/[~!@#$%^&*()+|<>?:{}]/.test(v) ||
          "?????? ?????? ???????????? ??????????????? ????????? ??? ????????????.",
      ],
      mission_git: null,
      mission_git_rule: [
        (v) => !!v || "?????? URL ????????? ?????? ?????????????????????.",
        // v => !/[~!@#$%^&*()+|<>?:{}]/.test(v) || '?????? URL ???????????? ??????????????? ????????? ??? ????????????.',
        // v => !(v.split('/').length !== 5) || '?????? URL??? ?????????????????????.'
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

      missions: [
        'msw_lte',
        'msw_sparrow_gun',
        'msw_sparrow_air',
        'msw_timesync',
        'msw_webrtc_crow',
        'msw_webrtc_kea',
        'msw_lx_cam',
        ],

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
          // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
                // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
                // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
                    // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
                        // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
          // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
                // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
        board_ver: self.board_ver,
        lte_type: self.lte_type,
        mission: self.mission,
      };

      if (Object.getOwnPropertyNames(self.mission).length <= 1) {
        delete con["mission"];
      }

      axios({
        validateStatus: function (status) {
          // ?????? ????????? 500 ????????? ?????? ??????. ?????????(500?????? ??????)??? ??????.
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
            {
              value1: "board_ver",
              value2: res_con.board_ver,
            },
            {
              value1: "lte_type",
              value2: res_con.lte_type,
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

<style>
.background {
  height: 100vh;
  /*background-image: url("../assets/blueyellow.jpg");*/
  background-color: white;
}
.h2 {
  text-align: left;
  font-weight: black;
}
.cardtitle {
  font-weight: black;
  color: #31323A;
  letter-spacing: 1mm;
  align-content: right;
}
.h4 {
  font-weight: black;
  color: #31323A;
  letter-spacing: 2mm;
}
.h5 {
  font-weight: black;
  color: #31323A;
  letter-spacing: 2mm;
}




</style>
