<template>
  <div class="MoveableBox scheduler pa-5">
    <v-row>
      <v-col cols="12">
        <v-row>
          <v-col cols="6">
            <v-menu
              ref="dateOpen"
              v-model="dateOpen"
              :close-on-content-click="false"
              :return-value.sync="start"
              offset-y
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  v-model="start"
                  label="Start Date"
                  prepend-icon="mdi-scheduler"
                  dense
                  readonly
                  outlined
                  hide-details
                  v-on="on"
                ></v-text-field>
              </template>

              <v-date-picker v-model="start" no-title>
                <v-spacer />
                <v-btn text color="primary" @click="dateOpen = false">Cancel</v-btn>
                <v-btn text color="primary" @click="$refs.dateOpen.save(start)">OK</v-btn>
              </v-date-picker>
            </v-menu>
          </v-col>
          <v-col cols="6">
            <v-select
              v-model="type"
              :items="typeOptions"
              label="Type"
              class="my-auto"
              hide-details
              outlined
              dense
            ></v-select>
          </v-col>
        </v-row>
        <div class="text-center mb-3 display-1">{{start | moment('YYYY MMMM')}}</div>
        <v-sheet height="500">
          <v-calendar
            ref="scheduler"
            :start="start"
            :events="events"
            @click:date="open"
            @click:event="showEvent"
            :type="type"
          ></v-calendar>
        </v-sheet>
      </v-col>
    </v-row>
    <Dialog v-if="$store.state.scheduler.dialog" />
    <EventDetail v-if="$store.state.scheduler.eventDetail" />
    <!-- <v-footer absolute>{{ $store.state.scheduler }}</v-footer> -->
  </div>
</template>

<script>
import Dialog from "./scheduler/Dialog";
import EventDetail from "./scheduler/EventDetail";

export default {
  components: {
    Dialog,
    EventDetail,
  },
  computed: {
    events() {
      return this.$store.state.scheduler.events;
    },
  },
  created() {
    var today = new Date();
    this.start =
      today.getFullYear() +
      "-" +
      (today.getMonth() + 1) +
      "-" +
      today.getDate();
  },
  data() {
    return {
      dateOpen: false,
      start: "",
      type: "month",
      typeOptions: [
        { text: "Day", value: "day" },
        { text: "Week", value: "week" },
        { text: "Month", value: "month" },
      ],
    };
  },
  methods: {
    open(date) {
      this.$store.commit("OPEN_SCHEDULER_DIALOG", date);
    },
    showEvent(event) {
      this.$store.commit("OPEN_SCHEDULER_EVENT", event);
    },
  },
};
</script>

<style>
.scheduler {
  background-color: white;
  /* border: solid 2px gray;  */
  border-radius: 5px;
  width: 550px;
  box-shadow: .5rem 1rem 2rem rgba(0,0,0,.3)!important;
}
</style>

