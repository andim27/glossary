<template>

  <card :title="Glossary">
    <div class="row">
      <div class="col-2"></div>
      <div class="col">
          <!--Search input-->
          <div class="row">
            <div class="input-group mb-3">
              <input type="text" class="form-control" placeholder="Search" v-model="search_word">
              <div class="input-group-append">
                <button class="btn btn-primary" type="submit" @click="searchWord">Go</button>
              </div>
            </div>
          </div>
         <!--Litera's list-->
          <div class="row">
              <ul class="list-group list-group-horizontal" v-for="item in firstLetters">
                  <li class="list-group-item border-none alfa-item" @click="letterClick(item)">{{item}}</li>
              </ul>
          </div>
          <!--Word list-->
          <div class="row"  v-show="isSearched == false">
              <div class="col-12">
                  <div class="alert alert-info">
                      <strong>Local dictionary is empty!</strong> Please add some new words
                      <router-link :to="{ name:'translate' }" class="navbar-brand">
                      here
                      </router-link>
                  </div>
              </div>
          </div>
          <div class="row" v-show="isSearched">
              <div class="col-12">
                <div class="card-group">
                  <div class="card bg-primary">
                      <div class="card-body text-center">
                          <div class="list-group">
                              <a href="#" class="list-group-item list-group-item-action">First word</a>
                              <a href="#" class="list-group-item list-group-item-action">Second word</a>
                              <a href="#" class="list-group-item list-group-item-action">Third word</a>
                              <a href="#" class="list-group-item list-group-item-action">Fourth word</a>
                              <a href="#" class="list-group-item list-group-item-action">Fifth word</a>
                          </div>
                      </div>
                  </div>
                  <div class="card">
                      <div class="card-body text-center">
                          <p class="card-text">
                              <div class="row h-100">
                                  <div class="col-sm-12 my-auto">
                                      <div class="card card-block w-100 border-none" >
                                          <div class="row">
                                              <div class="col">
                                                  <lang-select></lang-select>
                                              </div>

                                          </div>

                                      </div>
                                  </div>
                              </div>
                          </p>
                      </div>
                  </div>
                  <div class="card ">
                      <div class="card-body text-center">
                          <p class="card-text">
                          <div class="row h-100">
                              <div class="col-sm-12 my-auto">
                                  <div class="card card-block w-100">
                                      <div class="list-group">
                                          <a href="#" class="list-group-item list-group-item-action">Translated word 1</a>
                                          <a href="#" class="list-group-item list-group-item-action">Translated word 2</a>
                                          <a href="#" class="list-group-item list-group-item-action">Translated word 3</a>
                                      </div>
                                  </div>
                              </div>
                          </div>
                          </p>
                      </div>
                  </div>

              </div>
              </div>
          </div>
      </div>
      <div class="col-2"></div>
    </div>
  </card>
</template>

<script>
import LocaleDropdown from '../components/LocaleDropdown'
import LangSelect from '../components/LangSelect'
export default {
  components: {
    LocaleDropdown,LangSelect
  },
  middleware: 'auth',
  data: () => ({
    search_word:'',
    //  "abcdefghijklmnopqrstuvwxyz"
    firstLetters: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
    isSearched:false
  }),

  methods: {
    letterClick (letter) {
      this.search_word += letter;
    },
    searchWord () {
      //--get local dictionary
      //--search
      this.isSearched = true;
    }

  },

  metaInfo () {
    return { title: this.$t('home') }
  }
}
</script>

<style>
.border-none {
    border:none;
}
.alfa-item:hover  {
    color: blue;
    font-weight:bold;
    text-decoration: underline;
    cursor: pointer;
}
</style>
