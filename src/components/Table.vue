<template>
<v-container>
<v-card outlined>
    <br />
    <h5 class="header">All Ticktes</h5>
    <v-card-actions>
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search here"
            hide-details
            class="search"
            name="query"
          ></v-text-field>
        </v-col>    
      
        <v-col cols="12" sm="6">
          <v-select
            class="filter"
            :items="status"
            label="Status"
            solo
          ></v-select>
        </v-col>
      </v-row>
    </v-card-actions>

    <div id="grid-template">
      <div class="table-header-wrapper">
        <v-data-table :search="search" :headers="headers" :items="tickets">
          <template v-slot:[`item.status`]="{item}" >
             <v-chip  small flat @click="viewTicket(item.id)" :color="getColor(item.status)"
        dark>
            {{ item.status }}
          </v-chip>
          </template>

          <template  v-slot:[`item.actions`]="{ item }">
             <v-icon small @click="deleteTickets(item)">mdi-delete</v-icon>
          </template>
        </v-data-table>
      </div>
    </div>
  </v-card>
</v-container>
</template>

<script>
import AllTicketDataService from "../service/AllTicketDataServices";

export default {
  data() {
    return {
      search: "",
      tickets: [],
      slots: [
        "TicketId",
        "Priority",
        "Category",
        "Date",
        "Summary",
        "Actions",
      ],
      status: ["Open", "Closed", "Resolved", "Pending"],
      headers: [
        {
          text: "TicketId",
          align: "start",
          filterable: false,
          value: "name",
        },
        { text: "Priority", value: "priority" },
        { text: "Category", value: "category" },
        { text: "Summary", value: "summary" },
        { text: "Date", value: "date" },
        { text: "Status", value: "status", filterable: true },
        { text: "Actions", value: "action", sortable: false },
      ],
    };
  },
  created() {
    this.getAllTickets();
  },
  mounted() {
    this.getAllTickets();
  },
  methods: {
    getAllTickets() {
      var tickets = tickets;
      setInterval(() => {
        AllTicketDataService.getAll()
          .then((response) => {
            this.tickets = response.data;

            console.log(response.data);
          })
          .catch((e) => {
            console.log(e);
          });
      }, 10000);
    },

    refreshList(){
      this.retrieveTickets();
    },

    viewTicket(id) {
      this.$router.push({name: "Raised", params: {id: id}});
    },

    getColor (status) {
        if (status > open) return 'green'
        else if (status > closed) return 'yellow'
        else return 'red'
      },
  },
};
</script>

<style scoped>
.header {
  margin-left: 20px;
}
.search {
  /* margin-left: 99%; */
  box-sizing: border-box;
  width: 70%;
}
.filter {
  width: 30%;
  box-sizing: border-box;
  border-radius: 5px;
  left: 67%;
}
.v-card {
  border: 1px solid grey;
}
</style>
