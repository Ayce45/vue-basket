<template>
  <tr>
    <td>{{ category }}</td>
    <td>{{ libelle }}</td>
    <td>{{ quantity }}</td>
    <td>{{ price.toFixed(2) }} €</td>
  </tr>
</template>

<script>
export default {
  name: "basket-item",
  props: ["id", "category", "libelle", "quantity", "price"],
};
</script>
