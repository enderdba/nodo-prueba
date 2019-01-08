<template>
  <div>
    <div class="container center" v-show="isLoading">
      <h3>Cargando personajes ...</h3>
      <div class="preloader-wrapper big active">
        <div class="spinner-layer spinner-blue-only">
          <div class="circle-clipper left">
            <div class="circle"></div>
          </div>
          <div class="gap-patch">
            <div class="circle"></div>
          </div>
          <div class="circle-clipper right">
            <div class="circle"></div>
          </div>
        </div>
      </div>
    </div>
    <table v-show="!isLoading">
      <tr>
        <th>Nombre</th>
        <th>Casa</th>
        <th>Detalle</th>
      </tr>
      <tr v-for="character in characters">
        <td>{{ character.name }}</td>
        <td>{{ character.house }}</td>
        <td>
          <button @click="() => { goToDetail(character._id); }">Ver detalle</button>
        </td>
      </tr>
    </table>
  </div>
</template>


<script>
import { listsAllCharacters } from "../services/got.service.js";

export default {
  name: "list-component",
  /**
   * @description the data block represents all the local variable of this component.
   */
  data() {
    return {
      characters: [],
      isLoading: false
    };
  },

  /**
   * @description the create function is the first one to be execute when the component is being created (see vue js lifecycle).
   */
  created() {
    this.isLoading = true;
    listsAllCharacters().then(res => {
      this.characters = res;
      this.isLoading = false;
    });
  },

  /**
   * @description the methods block represents all the local methods of this component.
   */
  methods: {
    /**
     * @description go to the other component and request the character detail.
     * @param {string} id. the "_id" of the character that we are going to request.
     * @method goToDetail
     */
    goToDetail(id) {
      this.$router.push({ name: "detail", params: { id: id } });
    }
  }
};
</script>
<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>