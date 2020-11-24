<template>
  <div>
    <table class="table">
      <thead class="thead-dark">
        <th>
          Catégorie
          <span
            role="button"
            class="text-white order"
            @click="ordered('category')"
            v-html="type == 'category' && asc ? '⯅' : '⯆'"
          ></span>
        </th>
        <th>
          Produit
          <span
            role="button"
            class="text-white order"
            @click="ordered('libelle')"
            v-html="type == 'libelle' && asc ? '⯅' : '⯆'"
          ></span>
        </th>
        <th>
          Quantité
          <span
            role="button"
            class="text-white order"
            @click="ordered('quantity')"
            v-html="type == 'quantity' && asc ? '⯅' : '⯆'"
          ></span>
        </th>
        <th>
          Prix
          <span
            role="button"
            class="text-white order"
            @click="ordered('price')"
            v-html="type == 'price' && asc ? '⯅' : '⯆'"
          ></span>
        </th>
      </thead>
      <tbody v-if="dataOrdered.length">
        <basket-item
          v-for="basket in dataOrdered"
          :key="basket.id"
          :id="basket.id"
          :category="basket.category"
          :libelle="basket.libelle"
          :quantity="basket.quantity"
          :price="basket.priceOnlyOne * basket.quantity"
        ></basket-item>
      </tbody>
      <tbody v-if="!dataOrdered.length">
        <tr>
          <td colspan="4">Votre panier est vide</td>
        </tr>
      </tbody>
      <tfoot>
        <tr class="bg-dark">
          <td class="text-white font-weight-bold">Prix Total</td>
          <td></td>
          <td></td>
          <td class="text-white font-weight-bold">
            {{ totalPrice.toFixed(2) }} €
          </td>
        </tr>
      </tfoot>
    </table>
    <div class="row">
      <div class="offset-md-6 row col-6">
        <div class="col-6">
          <a class="btn btn-secondary btn-block" @click="reset">Annuler</a>
        </div>
        <div class="col-6">
          <a class="btn btn-primary btn-block">Valider</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BasketItem from "../components/BasketItem.vue";

export default {
  name: "basket-table",
  props: ["data"],
  components: {
    BasketItem,
  },
  data: function () {
    return {
      type: "id",
      asc: true,
    };
  },
  computed: {
    totalPrice() {
      let totalPrice = 0;
      this.data.forEach((element) => {
        totalPrice += element.quantity * element.priceOnlyOne;
      });
      return totalPrice;
    },
    dataOrdered() {
      let type = this.type;
      if (type == "price") {
        return this.asc
          ? [...this.data].sort((a, b) =>
              a["quantity"] * a["priceOnlyOne"] >
              b["quantity"] * b["priceOnlyOne"]
                ? 1
                : -1
            )
          : [...this.data].sort((a, b) =>
              a["quantity"] * a["priceOnlyOne"] <
              b["quantity"] * b["priceOnlyOne"]
                ? 1
                : -1
            );
      } else {
        return this.asc
          ? [...this.data].sort((a, b) => (a[type] > b[type] ? 1 : -1))
          : [...this.data].sort((a, b) => (a[type] < b[type] ? 1 : -1));
      }
    },
  },
  methods: {
    ordered(type) {
      if (this.type == type && this.asc == true) {
        this.asc = false;
      } else {
        this.type = type;
        this.asc = true;
      }
    },
    reset() {
      this.$emit("resetbasket");
    },
  },
};
</script>