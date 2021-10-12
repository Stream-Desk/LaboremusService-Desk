<template>
  <v-card outlined>
    <br />
    <h5 class="header">All Tickets</h5>
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
      </v-row>
    </v-card-actions>

    <div id="grid-template">
      <div class="table-header-wrapper">
        <v-data-table :search="search" :headers="headers" :items="tickets">
          <template v-slot:[`item.status`]="{ item }">
            <v-chip small flat :color="getColor(item.status)" dark>
              {{ item.status }}
            </v-chip>
          </template>

          <template v-slot:[`item.name`]="{ item }">
            {{ item.name }}
          </template>

          <template v-slot:[`item.actions`]="{ item }">
            <v-icon small @click="viewTicket(item.id)" color="blue"
              >mdi-details</v-icon
            >
            <v-icon small @click="deleteTicket(item.id)">mdi-delete</v-icon>
          </template>
        </v-data-table>
      </div>
    </div>
  </v-card>
</template>

<script>
import AllTicketDataService from "../service/AllTicketDataServices";

export default {
  name: "Table",

  data() {
    return {
      search: "",
      tickets: [],
      headers: [
        {
          text: "Ticket Id",
          align: "start",
          filterable: false,
          value: "ticketNumber",
        },
        { text: "Priority", value: "priority" },
        { text: "Category", value: "category" },
        { text: "Summary", value: "summary" },
        { text: "Date", value: "submitDate" },
        { text: "Requester", value: "name" },
        { text: "Status", value: "status", filterable: true },
        { text: "Actions", value: "actions", sortable: false },
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

    refreshList() {
      this.retrieveTickets();
    },

    viewTicket(id) {
      this.$router.push({ name: "Raised", params: { id: id } });
    },

    getColor(status) {
      if (status > open) return "green";
      else if (status > closed) return "yellow";
      else return "amber";
    },

    deleteTicket(id) {
      AllTicketDataService.delete(id)
        .then(() => {
          this.dialog = false;
        })
        .catch((e) => {
          console.log(e);
        });
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
