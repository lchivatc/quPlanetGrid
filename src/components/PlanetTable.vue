<template>
  <table>
    <thead>
      <tr>
        <td>Name</td>
        <td>Diameter</td>
        <td>Rotation Period</td>
        <td>Orbital Period</td>
        <td>Gravity</td>
        <td>Population</td>
        <td>Climate</td>
        <td>Terrain</td>
        <td>Surface Water</td>
        <td>Residents</td>
        <td>Films</td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="planet in planetsArray" :key="planet.id">
        <td>{{ planet.name }}</td>
        <td>{{ planet.diameter }}</td>
        <td>{{ planet.rotation_period }}</td>
        <td>{{ planet.orbital_period }}</td>
        <td>{{ planet.gravity }}</td>
        <td>{{ planet.population }}</td>
        <td>{{ planet.climate }}</td>
        <td>{{ planet.terrain }}</td>
        <td>{{ planet.surface_water }}</td>
        <td>{{ planet.residents.length }}</td>
        <td>{{ planet.films.length }}</td>
      </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  name: "planetTable",
  components: {},
  props: {
    planetsArray: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>
<style scoped lang="scss">
table {
  thead {
    font-weight: bold;
    td {
      padding-right: 0.5rem;
    }
  }
}
</style>
