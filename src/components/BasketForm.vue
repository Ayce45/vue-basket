<template>
  <form v-on:submit.prevent="onSubmit" class="row">
    <div class="col-md-6 mb-3 mt-3">
      <label for="category" class="form-label">Catégorie</label>
      <select
        required
        v-model="category"
        type="text"
        class="form-control"
        id="category"
      >
        <option value="" disabled selected>Choisir une catégorie</option>
        <option
          v-for="category in categories"
          v-bind:key="category"
          v-bind:value="category"
        >
          {{ category }}
        </option>
      </select>
    </div>
    <div class="col-md-6 mb-3 mt-3">
      <label for="category" class="form-label">Produit</label>
      <select
        required
        v-model="libelle"
        type="text"
        class="form-control"
        id="libelle"
      >
        <option value="" disabled selected>Choisir un produit</option>
        <option
          v-for="product in products"
          v-bind:key="product.libelle"
          v-bind:value="product.libelle"
        >
          {{ product.libelle }}
        </option>
      </select>
    </div>
    <div class="col-md-6 mb-3 mt-3">
      <label for="category" class="form-label">Quantité</label>
      <input
        required
        type="number"
        v-model="quantity"
        class="form-control"
        placeholder="Choisir une quantité"
      />
    </div>
    <div class="col-md-3 mt-5">
      <input type="reset" class="btn btn-secondary btn-block" />
    </div>
    <div class="col-md-3 mt-5">
      <input
        type="submit"
        class="btn btn-primary btn-block"
        id="submit"
        value="Ajouter"
      />
    </div>
  </form>
</template>

<script>
export default {
  name: "basket-form",
  props: ["data"],
  data: function () {
    return {
      category: "",
      libelle: "",
      quantity: "",
    };
  },
  computed: {
    categories() {
      let categories = [];
      this.data.forEach((element) => {
        categories.push(element.category);
      });
      return categories;
    },
    products() {
      let products = [];
      this.data.forEach((element) => {
        if (this.category == element.category) {
          products = element.products;
        }
      });
      return products;
    },
  },
  methods: {
    onSubmit: function () {
      console.log("onSubmit");
      this.$emit("addbasket", {
        id: null,
        category: this.category,
        libelle: this.libelle,
        quantity: this.quantity,
        priceOnlyOne: this.getPrice(this.libelle),
      });

      this.category = "";
      this.libelle = "";
      this.quantity = "";
    },
    getPrice(product) {
      let price = 0;
      this.products.forEach((element) => {
        if (element.libelle == product) {
          price = element.price;
        }
      });
      return price;
    },
  },
};
</script>