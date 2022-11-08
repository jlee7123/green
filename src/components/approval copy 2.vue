<template>
  <v-container>

        <h2>Create Approval</h2>


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

<br><br>













    <v-row no-gutters>
      <v-col align="start">

        <label class="font-weight-bold mt-n2">Drone ID:</label>
        <v-row no-gutters>
          <v-col cols="10">
            <v-text-field
              class="mr-2 mb-n2"
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
            <br />

<v-row>

            <label class="font-weight-bold mt-n2">Drone ID:</label>
  <v-col  cols="8">
<!--
          <b-form-input
            class="mr-2 mb-n2"
            dense
            outlined
            ref="drone_ID"
            v-model="drone_id"
            :rules="drone_id_rule"

            placeholder="* Drone ID"
            hint="Unique ID of Drone"
            persistent-hint
            required
          >
          </b-form-input>
          -->
</v-col>
<v-col>
            <v-btn
              class="mr-2 mb-n1 px-2"
              tile
              @click="IDCheck"
              :disabled="drone_id === null || drone_id === ''"
              elevation="5"
              color="primary"
            >
              CREATE
            </v-btn>
</v-col>

</v-row>
          </v-col>



          
        </v-row>
        <v-row class="pt-4"></v-row>
        <h4>ID Description</h4>

        <label class="font-weight-bold mt-n2">host:</label>
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
              label="* GCS Host (e.g. gcs.iotocean.org)"
              required
            ></v-text-field>
          </v-col>
        </v-row>

        <label class="font-weight-bold mt-n2">drone:</label>
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


        <label class="font-weight-bold mt-n2">gcs:</label>
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

        <label class="font-weight-bold mt-n2">type:</label>
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

        <label class="font-weight-bold mt-n2">system id:</label>
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

        <label class="font-weight-bold mt-n2">mav_ver:</label>
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

        <label class="font-weight-bold mt-n2">update:</label>
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

        <label class="font-weight-bold mt-n2">mission name:</label>
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

      <v-col align="start" class="ml-2">
        <h2>Retrieve Approval</h2>
        <label class="font-weight-bold">Drone ID:</label>
        <v-row no-gutters>
          <v-col cols="10">
            <v-select
              v-model="approval_selected"
              :items="approval_list"
              label="Drone ID List*"
              hint="Drone ID List"
              required
              hide-details
            ></v-select>
          </v-col>
          <v-col cols="2">
            <v-btn
              class="mr-2 mb-n2"
              tile
              @click="RetrieveApproval"
              :disabled="approval_selected === ''"
              elevation="5"
              color="primary"
            >
              SELECT
            </v-btn>
          </v-col>
        </v-row>
        <v-card class="mt-3">
          <label v-if="retrieve_approval_flag" class="font-weight-bold mx-2"
            >Drone</label
          >
          <v-data-table
            v-if="retrieve_approval_flag"
            :headers="header"
            :items="response"
            hide-default-header
            hide-default-footer
            class="elevation-1"
            light
          ></v-data-table>
        </v-card>
        <v-card v-if="retrieve_mission_flag" class="mt-3">
          <label class="font-weight-bold mx-2">Mission</label>
          <v-data-table
            v-for="idx in res_length"
            :key="idx"
            :headers="mission_header"
            :items="mission_response[idx]"
            hide-default-header
            hide-default-footer
            class="elevation-1 mt-2"
            light
          ></v-data-table>
        </v-card>
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

    <!-- 기존 코드 시작 -->

    <h2 class="text-center">Approval Management</h2>
    <br /><br />

    <b-col>
      <label for="input-live">Platform Address:</label>
      <b-form-input
        id="Platform"
        v-model="data_obj.Platform_addr"
        :state="data_obj.Platform_addr.length >= 9"
        placeholder="Enter Platform Address"
        trim
      ></b-form-input>
    </b-col>

    <b-row>
      <b-col>
        <v-btn
          class="ma-1"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          v-on:click="ae_form"
        >
          AE
        </v-btn>
      </b-col>
      <b-col>
        <v-btn
          class="ma-1"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          v-on:click="cnt_form"
        >
          Container
        </v-btn>
      </b-col>
      <b-col>
        <v-btn
          class="ma-1"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          v-on:click="cin_form"
        >
          Content Instance</v-btn
        >
      </b-col>
      <b-col>
        <v-btn
          class="ma-1"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          v-on:click="sub_form"
        >
          Subscription</v-btn
        >
      </b-col>
    </b-row>
    <br />

    <!-- AE -->
    <v-card
      v-if="selected === 'ae'"
      class="pa-10"
      outlined
      shaped
      elevation="3"
    >
      <h3>oneM2M Request Primitive Parameters - AE</h3>
      <v-row>
        <v-col>
          <v-card class="pa-1" shaped elevation="0">
            <h3>&nbsp;</h3>
            <b-col>
              <label for="input-live">To (to): String</label>
              <b-form-input
                id="resid"
                v-model="data_obj.Res_Id"
                :state="data_obj.Res_Id.length >= 3"
                placeholder="Enter Resource ID (TO)"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">From (fr): String</label>
              <b-form-input
                id="xm2morigin"
                v-model="data_obj.X_M2M_Origin"
                :state="data_obj.X_M2M_Origin.length >= 1"
                placeholder="Enter X-M2M Origin"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">Request ID (rqi): String </label>
              <b-form-input
                id="xm2mri"
                v-model="data_obj.X_M2M_RI"
                :state="data_obj.X_M2M_RI.length >= 4"
                placeholder="Enter X-M2M RI"
                trim
              ></b-form-input>
              <br />
            </b-col>
          </v-card>
        </v-col>

        <v-col>
          <h5>Primitive Content (pc):</h5>
          <v-card class="pa-3" outlined shaped elevation="1">
            <b-col>
              <label for="input-live">resourceName (rn): String</label>
              <b-form-input
                id="data_obj.rn"
                v-model="data_obj.rn"
                :state="data_obj.rn.length >= 0"
                ref="data_obj.rn"
                @change="change"
                placeholder="resourcename"
                trim
              ></b-form-input>

              <br />

              <label for="input-live">labels (lbl): Array of String</label>
              <b-form-input
                id="xm2mlbl"
                v-model="data_obj.lbl"
                :state="data_obj.lbl.length >= 0"
                placeholder="[''key1'', ''key2'']"
                trim
              ></b-form-input>
              <br />

              <label for="input-live">*app-ID (api): String</label>
              <b-form-input
                id="xm2mapi"
                v-model="data_obj.api"
                :state="data_obj.api.length >= 0"
                ref="xm2mapi"
                @change="change"
                placeholder="appid"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">*requestReachability (rr): Boolean</label>
              <b-form-input
                id="xm2mrr"
                v-model="data_obj.rr"
                :state="res_name.length >= 0"
                ref="xm2mrr"
                @change="change"
                placeholder="true"
                trim
              ></b-form-input>
              <br />
            </b-col>
          </v-card>
        </v-col>
      </v-row>
    </v-card>

    <!-- v-card CNT -->
    <v-card
      v-if="selected === 'cnt'"
      class="pa-10"
      outlined
      shaped
      elevation="3"
    >
      <h3>oneM2M Request Primitive Parameters - container</h3>
      <v-row>
        <v-col>
          <v-card class="pa-1" shaped elevation="0">
            <h3>&nbsp;</h3>
            <b-col>
              <label for="input-live">To (to): String</label>
              <b-form-input
                id="resid"
                v-model="data_obj.Res_Id"
                :state="data_obj.Res_Id.length >= 3"
                placeholder="Enter Resource ID (TO)"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">From (fr): String</label>
              <b-form-input
                id="xm2morigin"
                v-model="data_obj.X_M2M_Origin"
                :state="data_obj.X_M2M_Origin.length >= 1"
                placeholder="Enter X-M2M Origin"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">Request ID (rqi): String </label>
              <b-form-input
                id="xm2mri"
                v-model="data_obj.X_M2M_RI"
                :state="data_obj.X_M2M_RI.length >= 4"
                placeholder="Enter X-M2M RI"
                trim
              ></b-form-input>
              <br />

              <br />
            </b-col>
          </v-card>
        </v-col>

        <v-col>
          <h5>Primitive Content (pc):</h5>
          <v-card class="pa-3" outlined shaped elevation="1">
            <b-col>
              <label for="input-live">resourceName (rn): String</label>
              <b-form-input
                id="data_obj.rn"
                v-model="data_obj.rn"
                :state="data_obj.rn.length >= 0"
                ref="data_obj.rn"
                @change="change"
                placeholder="resourcename"
                trim
              ></b-form-input>

              <br />

              <label for="input-live">labels (lbl): Array of String</label>
              <b-form-input
                id="xm2mlbl"
                v-model="data_obj.lbl"
                :state="data_obj.lbl.length >= 0"
                placeholder="[''key1'', ''key2'']"
                trim
              ></b-form-input>
              <br />

              <label for="input-live">maxNrOfInstances (mni): Integer</label>
              <b-form-input
                id="xm2mmni"
                v-model="data_obj.mni"
                :state="data_obj.mni.length >= 0"
                placeholder="54321"
                trim
              ></b-form-input>
              <br />

              <label for="input-live">maxByteSize (mbs): Integer</label>
              <b-form-input
                id="xm2mmbs"
                v-model="data_obj.mbs"
                :state="data_obj.mbs.length >= 0"
                placeholder="54332"
                trim
              ></b-form-input>
              <br />
            </b-col>
          </v-card>
        </v-col>
      </v-row>
    </v-card>

    <!-- v-card CIN -->
    <v-card
      v-if="selected === 'cin'"
      class="pa-10"
      outlined
      shaped
      elevation="3"
    >
      <h3>oneM2M Request Primitive Parameters - content instance</h3>
      <v-row>
        <v-col>
          <v-card class="pa-1" shaped elevation="0">
            <h3>&nbsp;</h3>
            <b-col>
              <label for="input-live">To (to): String</label>
              <b-form-input
                id="resid"
                v-model="data_obj.Res_Id"
                :state="data_obj.Res_Id.length >= 3"
                placeholder="Enter Resource ID (TO)"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">From (fr): String</label>
              <b-form-input
                id="xm2morigin"
                v-model="data_obj.X_M2M_Origin"
                :state="data_obj.X_M2M_Origin.length >= 1"
                placeholder="Enter X-M2M Origin"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">Request ID (rqi): String </label>
              <b-form-input
                id="xm2mri"
                v-model="data_obj.X_M2M_RI"
                :state="data_obj.X_M2M_RI.length >= 4"
                placeholder="Enter X-M2M RI"
                trim
              ></b-form-input>
              <br />

              <br />
            </b-col>
          </v-card>
        </v-col>

        <v-col>
          <h5>Primitive Content (pc):</h5>
          <v-card class="pa-3" outlined shaped elevation="1">
            <b-col>
              <label for="input-live">resourceName (rn): String</label>
              <b-form-input
                id="data_obj.rn"
                v-model="data_obj.rn"
                :state="data_obj.rn.length >= 0"
                ref="data_obj.rn"
                @change="change"
                placeholder="resourcename"
                trim
              ></b-form-input>

              <br />

              <label for="input-live">labels (lbl): Array of String</label>
              <b-form-input
                id="xm2mlbl"
                v-model="data_obj.lbl"
                :state="data_obj.lbl.length >= 0"
                placeholder="[''key1'', ''key2'']"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">content (con): Any</label>
              <b-form-input
                id="cin_con"
                v-model="data_obj.con"
                :state="data_obj.con.length >= 0"
                ref="cin_con"
                @change="change"
                placeholder="Enter content (con)"
                trim
              ></b-form-input>
              <br />
            </b-col>
          </v-card>
        </v-col>
      </v-row>
    </v-card>

    <!-- SUB -->
    <v-card
      v-if="selected === 'sub'"
      class="pa-10"
      outlined
      shaped
      elevation="3"
    >
      <h3>oneM2M Request Primitive Parameters - subscription</h3>
      <v-row>
        <v-col>
          <v-card class="pa-1" shaped elevation="0">
            <h3>&nbsp;</h3>
            <b-col>
              <label for="input-live">To (to): String</label>
              <b-form-input
                id="resid"
                v-model="data_obj.Res_Id"
                :state="data_obj.Res_Id.length >= 3"
                placeholder="Enter Resource ID (TO)"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">From (fr): String</label>
              <b-form-input
                id="xm2morigin"
                v-model="data_obj.X_M2M_Origin"
                :state="data_obj.X_M2M_Origin.length >= 1"
                placeholder="Enter X-M2M Origin"
                trim
              ></b-form-input>
              <br />
              <label for="input-live">Request ID (rqi): String </label>
              <b-form-input
                id="xm2mri"
                v-model="data_obj.X_M2M_RI"
                :state="data_obj.X_M2M_RI.length >= 4"
                placeholder="Enter X-M2M RI"
                trim
              ></b-form-input>
              <br />

              <br />
            </b-col>
          </v-card>
        </v-col>

        <v-col>
          <h5>Primitive Content (pc):</h5>
          <v-card class="pa-3" outlined shaped elevation="1">
            <b-col>
              <label for="input-live">resourceName (rn): String</label>
              <b-form-input
                id="data_obj.rn"
                v-model="data_obj.rn"
                :state="data_obj.rn.length >= 0"
                ref="data_obj.rn"
                @change="change"
                placeholder="resourcename"
                trim
              ></b-form-input>

              <br />

              <label for="input-live">labels (lbl): Array of String</label>
              <b-form-input
                id="xm2mlbl"
                v-model="data_obj.lbl"
                :state="data_obj.lbl.length >= 0"
                placeholder="[''key1'', ''key2'']"
                trim
              ></b-form-input>
              <br />

              <label for="input-live">*notificationURI (nu): String</label>
              <b-form-input
                id="sub_nu"
                v-model="data_obj.nu"
                :state="data_obj.nu.length >= 0"
                placeholder="[''mqtt://127.0.0.1:8883'']"
                trim
              ></b-form-input>

              <br />

              <label for="input-live"
                >eventNotificationCriteria (enc): Array of Integer</label
              >
              <b-form-input
                id="sub_enc"
                v-model="data_obj.enc"
                :state="data_obj.enc.length >= 0"
                placeholder="[1,2]"
                trim
              ></b-form-input>
              <br />
            </b-col>
          </v-card>
        </v-col>
      </v-row>
    </v-card>
    <br />

    <!--AE 선택시 HTTP Request 버튼 -->
    <v-row v-if="selected === 'ae'">
      <v-col>
        <v-btn
          class="ma-0"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          @click="ae_print"
        >
          CREATE HTTP REQUEST
        </v-btn>
      </v-col>
      <v-col>
        <v-btn
          class="ma-0 indigo--text"
          color="#EDE7F6"
          block
          raised
          rounded
          elevation="2"
          @click="post_request"
        >
          SEND Request
        </v-btn>
      </v-col>
    </v-row>

    <!--CNT 선택시 HTTP Request 버튼 -->
    <v-row v-if="selected === 'cnt'">
      <v-col>
        <v-btn
          class="ma-0"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          @click="cnt_print"
        >
          CREATE HTTP REQUEST
        </v-btn>
      </v-col>
      <v-col>
        <v-btn
          class="ma-0 indigo--text"
          color="#EDE7F6"
          block
          raised
          rounded
          elevation="2"
          @click="post_request"
        >
          SEND Request
        </v-btn>
      </v-col>
    </v-row>

    <!--CIN 선택시 HTTP Request 버튼 -->
    <v-row v-if="selected === 'cin'">
      <v-col>
        <v-btn
          class="ma-0"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          @click="cin_print"
        >
          CREATE HTTP REQUEST</v-btn
        >
      </v-col>
      <v-col>
        <v-btn
          class="ma-0 indigo--text"
          color="#EDE7F6"
          block
          raised
          rounded
          elevation="2"
          @click="post_request"
        >
          SEND Request
        </v-btn>
      </v-col>
    </v-row>

    <!--sub 선택시 HTTP Request 버튼 -->
    <v-row v-if="selected === 'sub'">
      <v-col>
        <v-btn
          class="ma-0"
          color="deep-purple"
          block
          outlined
          raised
          rounded
          elevation="2"
          @click="sub_print"
        >
          CREATE HTTP REQUEST</v-btn
        >
      </v-col>
      <v-col>
        <v-btn
          class="ma-0 indigo--text"
          color="#EDE7F6"
          block
          raised
          rounded
          elevation="2"
          @click="post_request"
        >
          SEND Request
        </v-btn>
      </v-col>
    </v-row>

    <br />
    <br /><br />

    <v-card class="pa-10" outlined shaped elevation="2">
      <v-row>
        <b-col sm="6" md="5" offset-md="2" lg="6" offset-lg="0">
          <h5>HTTP Request</h5>
          <b-card shadow="always">
            <br />
            {{ data_obj.Create_text }}
            {{ "http://" + data_obj.Platform_addr + "/" + data_obj.Res_Id }}
            <br />
            <br />

            <div>
              <b>Header</b>
              <b-table
                ref="reqtable"
                style="font-size: 0.9rem"
                small
                stacked
                :items="req_items"
                :fields="req_fields"
              >
              </b-table>
              <h5>&nbsp;</h5>
              <b>Body</b>

              <b-form-textarea
                style="font-size: 0.8rem"
                id="textreq"
                v-model="request_text"
                rows="10"
                max-rows="20"
              ></b-form-textarea>
            </div>
          </b-card>
          <br />
        </b-col>
        <br />

        <b-col sm="6" md="5" offset-md="2" lg="6" offset-lg="0">
          <h5>HTTP Response</h5>
          <b-card shadow="always">
            <br />
            {{ this.res_status }}
            <br />
            <br />
            <div>
              <b>Header</b>
              <b-table
                ref="restable"
                style="font-size: 0.9rem"
                small
                stacked
                :items="res_items"
                :fields="res_fields"
              >
              </b-table>
              <b>Body</b>
              <b-form-textarea
                style="font-size: 0.8rem"
                id="textres"
                v-model="response_text"
                rows="10"
                max-rows="20"
              ></b-form-textarea>
            </div>
          </b-card>
        </b-col>

        <br />
      </v-row>
    </v-card>
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
            MOBIUS_DISCONNECTION_TEXT: 'Disconnect',
            MOBIUS_CONNECTION_TEXT: 'Connect',

            MOBIUS_CONNECTION_CONNECTED: localStorage.getItem('mobius_connected') ? (localStorage.getItem('mobius_connected') === 'true') : false,
            MOBIUS_CONNECTION_DISABLED: false,

            host: localStorage.getItem('mobius_host') ? (localStorage.getItem('mobius_host')) : (this.$store.state.VUE_APP_MOBIUS_HOST),
            host_rule: [
                v => !!v || '호스트 주소는 필수 입력사항입니다.',
             //   v => /^[.0-9]*$/.test(v) || '호스트 주소는 숫자만 입력 가능합니다.'
            ],
            approval: localStorage.getItem('mobius_approval') ? (localStorage.getItem('mobius_approval')) : (this.$store.state.VUE_APP_MOBIUS_APPROVAL),
            approval_rule: [
                v => !!v || 'Approval 이름은 필수 입력사항입니다.',
                v => !/[~!@#$%^&*()+|<>?:{}]/.test(v) || 'Approval 이름에는 특수문자를 사용할 수 없습니다.'
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

      selected: "ae",
      res_name: "",
      data_obj: {
        Create_text: "POST",
        Platform_addr: "127.0.0.1:7579",
        Res_Id: "Mobius",
        X_M2M_RI: "12345",
        X_M2M_Origin: "SOrigin",
        Accept: "application/json",

        rn: "",
        lbl: "",
        mni: "",
        mbs: "",
        ty: "",
        op: "",
        api: "",
        rr: "",
        con: "",
        nu: "",
        enc: "",
      },
      req_fields: [
        { key: "X-M2M-RI", class: "text-center" },
        { key: "X-M2M-Origin", class: "text-center" },
        { key: "Content-Type", class: "text-center" },
        { key: "Accept", class: "text-center" },
      ],
      req_items: [
        { "X-M2M-RI": "", "X-M2M-Origin": "", "Content-Type": "", Accept: "" },
      ],
      res_fields: [
        { key: "X-M2M-RI", class: "text-center" },
        { key: "X-M2M-RSC", class: "text-center" },
        { key: "X-M2M-RVI", class: "text-center" },
        { key: "Content-Length", class: "text-center" },
        { key: "Content-Type", class: "text-center" },
      ],
      res_items: [
        {
          "X-M2M-RI": "",
          "X-M2M-RSC": "",
          "X-M2M-RVI": "",
          "Content-Length": "",
          "Content-Type": "",
        },
      ],
      headers_text: "",
      request_text: "",
      response_text: "",
      res_mess: "",
      res_errmess: "",
      res_status: "",
    };
  },

  // 기존 data 시작

  /*
  data() {
    return {
            };
  },
  */
  methods: {

//app.vue start
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

    //기존 methods 내용 시작

    ae_form() {
      this.selected = "ae";
    },
    cnt_form() {
      this.selected = "cnt";
    },
    cin_form() {
      this.selected = "cin";
    },
    sub_form() {
      this.selected = "sub";
    },

    request_header_change(obj) {
      console.log(obj);
      this.req_items[0]["X-M2M-RI"] = obj["X-M2M-RI"];
      this.req_items[0]["X-M2M-Origin"] = obj["X-M2M-Origin"];
      this.req_items[0]["Content-Type"] = obj["Content-Type"];
      this.req_items[0]["Accept"] = obj["Accept"];
      this.$refs.reqtable.refresh();
    },
    response_header_change(obj) {
      console.log(obj);
      this.res_items[0]["X-M2M-RI"] = obj["X-M2M-RI"];
      this.res_items[0]["X-M2M-RSC"] = obj["X-M2M-RSC"];
      this.res_items[0]["X-M2M-RVI"] = obj["X-M2M-RVI"];
      this.res_items[0]["Content-Length"] = obj["Content-Length"];
      this.res_items[0]["Content-Type"] = obj["Content-Type"];
      this.$refs.restable.refresh();
    },
    ae_print() {
      let ae_obj = {};
      ae_obj["m2m:ae"] = {};

      if (this.data_obj.rn != "") {
        ae_obj["m2m:ae"].rn = this.data_obj.rn;
      }

      /*
      if(this.data_obj.rn===""){
       
      }else{
        ae_obj["m2m:ae"].rn = this.data_obj.rn;
      };
      */

      if (this.data_obj.lbl != "") {
        ae_obj["m2m:ae"].lbl = JSON.parse(this.data_obj.lbl);
      }

      if (this.data_obj.api == "" || this.data_obj.rr == "") {
        alert("Enter app-ID(api) or requestReachability(rr)");
      } else {
        ae_obj["m2m:ae"].api = this.data_obj.api;
        ae_obj["m2m:ae"].rr = this.data_obj.rr;
      }

      this.data_obj["Content-Type"] = "applicaton/json;ty=2";
      this.data_obj["body"] = ae_obj;

      let headers = {};
      headers["X-M2M-RI"] = this.data_obj.X_M2M_RI;
      headers["X-M2M-Origin"] = this.data_obj.X_M2M_Origin;
      headers["Accept"] = this.data_obj.Accept;
      headers["Content-Type"] = this.data_obj["Content-Type"];

      this.req_display_obj = ae_obj;
      this.request_header_change(headers);
      return (this.request_text = JSON.stringify(ae_obj, undefined, 2));
    },

    cnt_print() {
      let cnt_obj = {};
      cnt_obj["m2m:cnt"] = {};

      if (this.data_obj.rn != "") {
        cnt_obj["m2m:cnt"].rn = this.data_obj.rn;
      }

      if (this.data_obj.lbl != "") {
        cnt_obj["m2m:cnt"].lbl = JSON.parse(this.data_obj.lbl);
      }

      if (this.data_obj.mni != "") {
        cnt_obj["m2m:cnt"].mni = parseInt(this.data_obj.mni);
      }

      if (this.data_obj.mbs != "") {
        cnt_obj["m2m:cnt"].mbs = parseInt(this.data_obj.mbs);
      }

      this.data_obj["Content-Type"] = "applicaton/json;ty=3";
      this.data_obj["body"] = cnt_obj;

      let headers = {};
      headers["X-M2M-RI"] = this.data_obj.X_M2M_RI;
      headers["X-M2M-Origin"] = this.data_obj.X_M2M_Origin;
      headers["Accept"] = this.data_obj.Accept;
      headers["Content-Type"] = this.data_obj["Content-Type"];

      this.req_display_obj = cnt_obj;
      this.request_header_change(headers);
      return (this.request_text = JSON.stringify(
        this.req_display_obj,
        undefined,
        2
      ));
    },

    cin_print() {
      let cin_obj = {};
      cin_obj["m2m:cin"] = {};

      if (this.data_obj.rn != "") {
        cin_obj["m2m:cin"].rn = this.data_obj.rn;
      }
      if (this.data_obj.lbl != "") {
        cin_obj["m2m:cin"].lbl = JSON.parse(this.data_obj.lbl);
      }
      //cin_obj["m2m:cin"].lbl = JSON.parse(this.data_obj.lbl);
      if (this.data_obj.con != "") {
        cin_obj["m2m:cin"].con = this.data_obj.con;
      }

      //cin_obj["m2m:cin"].con = this.data_obj.con;

      this.data_obj["Content-Type"] = "applicaton/json;ty=4";
      this.data_obj["body"] = cin_obj;

      let headers = {};
      headers["X-M2M-RI"] = this.data_obj.X_M2M_RI;
      headers["X-M2M-Origin"] = this.data_obj.X_M2M_Origin;
      headers["Accept"] = this.data_obj.Accept;
      headers["Content-Type"] = this.data_obj["Content-Type"];

      this.req_display_obj = cin_obj;
      this.request_header_change(headers);

      return (this.request_text = JSON.stringify(
        this.req_display_obj,
        undefined,
        2
      ));
    },

    sub_print() {
      let sub_obj = {};
      sub_obj["m2m:sub"] = {};
      if (this.data_obj.enc != "") {
        sub_obj["m2m:sub"].enc = {};
        sub_obj["m2m:sub"].enc.net = JSON.parse(this.data_obj.enc);
      }

      //sub_obj["m2m:sub"].exc = 0;

      if (this.data_obj.rn != "") {
        sub_obj["m2m:sub"].rn = this.data_obj.rn;
      }
      if (this.data_obj.lbl != "") {
        sub_obj["m2m:sub"].lbl = JSON.parse(this.data_obj.lbl);
      }

      //sub_obj["m2m:sub"].nu = ["http://127.0.0.1:8369"];

      if (this.data_obj.nu == "") {
        alert("Enter notificationURI (nu)");
      } else {
        sub_obj["m2m:sub"].nu = JSON.parse(this.data_obj.nu);
      }

      //sub_obj["m2m:sub"].nu = JSON.parse(this.data_obj.nu);
      this.data_obj["Content-Type"] = "applicaton/json;ty=23";
      this.data_obj["body"] = sub_obj;

      let headers = {};
      headers["X-M2M-RI"] = this.data_obj.X_M2M_RI;
      headers["X-M2M-Origin"] = this.data_obj.X_M2M_Origin;
      headers["Accept"] = this.data_obj.Accept;
      headers["Content-Type"] = this.data_obj["Content-Type"];

      this.req_display_obj = sub_obj;
      this.request_header_change(headers);

      return (this.request_text = JSON.stringify(
        this.req_display_obj,
        undefined,
        2
      ));
    },

    post_request() {
      let url =
        "http://" + this.data_obj.Platform_addr + "/" + this.data_obj.Res_Id;
      const headers = {};
      headers["X-M2M-RI"] = this.data_obj.X_M2M_RI;
      headers["X-M2M-Origin"] = this.data_obj.X_M2M_Origin;
      headers["Content-Type"] = this.data_obj["Content-Type"];
      headers["Accept"] = this.data_obj.Accept;

      let body = this.data_obj.body;
      axios
        .post(url, body, { headers })
        .then((response) => {
          this.res_mess = response.data;
          this.res_status = response.status;
          let headers = {};
          headers["X-M2M-RI"] = response.headers["x-m2m-ri"];
          headers["X-M2M-RSC"] = response.headers["x-m2m-rsc"];
          headers["X-M2M-RVI"] = response.headers["x-m2m-rvi"];
          headers["Content-Length"] = response.headers["content-length"];
          headers["Content-Type"] = response.headers["content-type"];
          this.response_header_change(headers);

          return (this.response_text = JSON.stringify(
            this.res_mess,
            undefined,
            2
          ));
        })
        .catch((error) => {
          this.res_errmess = error.response.data;
          if (error.response.status === 409) {
            this.res_status = error.response.status;
          } else if (error.response.status === 404) {
            this.res_status = error.response.status;
          }
          let headers = {};
          headers["X-M2M-RI"] = error.response.headers["x-m2m-ri"];
          headers["X-M2M-RSC"] = error.response.headers["x-m2m-rsc"];
          headers["X-M2M-RVI"] = error.response.headers["x-m2m-rvi"];
          headers["Content-Length"] = error.response.headers["content-length"];
          headers["Content-Type"] = error.response.headers["content-type"];
          this.response_header_change(headers);

          return (this.response_text = this.res_errmess);
        });
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
    }


};
</script>
