  <template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="center" v-show="isLoading">
            <h5>Cargando detalle del personaje...</h5>
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
          <div v-show="!isLoading" class="modal-data center">
            <h4>Detalles de personaje</h4>
            <h5>{{character.name}}</h5>
            <img v-if="character.imageLink" :src="'https://api.got.show' + character.imageLink">
            <p>
              <span v-if="character.culture">{{character.culture}}, </span>
              {{character.male ? "Masculino" : "Femenino"}}
            </p>
            <div class="left-align">
              <ul class="collapsible">
                <li>
                  <div v-if="character.titles.length > 0" class="collapsible-header">
                    <i class="material-icons">title</i>Título(s)
                  </div>
                  <div class="collapsible-body">
                    <ul v-for="title in character.titles">
                      <li>{{ title }}</li>
                    </ul>
                  </div>
                </li>
                <li>
                  <div class="collapsible-header">
                    <i class="material-icons">library_books</i>Libro(s)
                  </div>
                  <div class="collapsible-body">
                    <ul v-for="book in character.books">
                      <li>{{ book }}</li>
                    </ul>
                  </div>
                </li>
                <li v-if="character.house">
                  <div id="house" class="collapsible-header">
                    <i class="material-icons">home</i>
                    {{ character.house }}
                  </div>
                </li>
              </ul>
            </div>
            <button class="btn" @click="() => { goBack(); }">VOLVER</button>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import { getACharacter } from "../services/got.service.js";
export default {
  name: "detail",
  data() {
    return {
      character: { titles: [] },
      isLoading: false
    };
  },
  created() {
    this.isLoading = true;
    getACharacter(this.$route.params.id).then(res => {
      M.AutoInit();
      this.character = res.data;
      this.isLoading = false;
    });
  },
  methods: {
    /**
     * @description go to the other component and request the character detail.
     * @param {string} id. the "_id" of the character that we are going to request.
     * @method goToDetail
     */
    goBack(id) {
      this.$router.push({ path: "/" });
    }
  }
};
</script>

<style>
.modal-mask {
  margin-top: 2em;
  width: 100%;
  height: 100%;
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 500px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
