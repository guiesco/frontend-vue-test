<template>
<div>
    <b-card :title="`${product.name}`" tag="article" :footer="`Last updated ${timeCalc()} mins ago`" footer-tag="footer" class="mb-2">
        <b-container class="bv-row">
            <b-row>
                <b-col cols="4">
                    <p class="card-text">
                        <b>Department</b>
                    </p>
                </b-col>
                <b-col cols="4">
                    <p class="card-text">
                        {{ product.department }}
                    </p>
                </b-col>
            </b-row>
            <b-row>
                <b-col cols="4">
                    <p class="card-text">
                        <b>Price</b>
                    </p>
                </b-col>
                <b-col cols="4">
                    <p class="card-text">
                        {{ product.price | priceFilter }}
                    </p>
                </b-col>
            </b-row>
            <b-row style="margin-top: 35px;">
                <b-col cols="4">
                    <p class="card-text">
                       <b>Status</b>
                    </p>
                </b-col>
                <b-col cols="4">
                    <b-badge variant="success">{{ product.status | statusFilter }}</b-badge>
                </b-col>
            </b-row>
        </b-container>
    </b-card>
</div>
</template>


<script>
import productService from "@/services/product";

export default {
  name: "item",
  data() {
    return {
      product: productService.get(this.$route.params.id)
    };
  },
  methods: {
    timeCalc: function() {
        return Math.floor(Math.abs(new Date() - new Date(this.product.date)) / 1000 / 60)
    }
  },
  filters: {
    priceFilter: value => {
      return "R$ " + value.replace(".", ",");
    },
    statusFilter: value => {
      if (value == "PROD_ACTIVE") {
        return "Active";
      } else {
        return "Inactive";
      }
    }
  }
};
</script>