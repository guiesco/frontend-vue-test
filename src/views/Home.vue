<template>
  <div class="home">
    <b-form>
      <b-input v-model="search" class="mb-2 mr-sm-2 mb-sm-0" size="lg" placeholder="Search..." />
    </b-form>
    <b-table selectable @row-clicked="clickHandler" @row-hovered="rowPointer" :sort-by.sync="sortBy" sort-desc.sync="sortDesc" bordered striped hover :items="filteredItems" :fields="tableFields"></b-table>
    <p>
      Sorting By: <b>{{ sortBy }}</b>,
      Sort Direction: <b>{{ sortDesc ? 'Descending' : 'Ascending' }}</b>
    </p>
  </div>
</template>

<script>
// @ is an alias to /src
import productService from "@/services/product";

import moment from "moment";

export default {
  name: "home",
  components: {},
  data() {
    return {
      products: productService.list(),
      tableFields: [
        {
          key: "name",
          label: "Product",
          sortable: true
        },

        {
          key: "department",
          label: "Department",
          sortable: true
        },

        {
          key: "price",
          label: "Price",
          sortable: true,
          formatter: value => {
            return "R$ " + value.replace(".", ",");
          }
        },

        {
          key: "date",
          label: "Last Modified",
          sortable: true,
          formatter: value => {
            return moment(value).format("MM/DD/YYYY hh:mm");
          }
        },

        {
          key: "status",
          label: "Status",
          sortable: true,
          formatter: value => {
            if (value == "PROD_ACTIVE") {
              return "Active";
            } else {
              return "Inactive";
            }
          }
        }
      ],
      search: "",
      sortBy: "name",
      sortDesc: false
    };
  },
  computed: {
    filteredItems() {
      return this.products.filter(
        product =>
          product.name.toLowerCase().includes(this.search.toLowerCase()) ||
          product.department.toLowerCase().includes(this.search.toLowerCase())
      );
    }
  },
  methods: {
    clickHandler(item) {
      this.$router.push({ name: "item", params: { id: item.id } });
    },
    rowPointer() {
      this.$el.style.cursor = "pointer";
    }
  },
  filters: {}
};
</script>


<style lang="stylus" scoped>
form {
  margin-bottom: 30px;
}
</style>
