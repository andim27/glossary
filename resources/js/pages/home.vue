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
                <button class="btn btn-primary" type="submit">Go</button>
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
          <div class="row">
              <div class="col-12">
                <div class="card-group">
                  <div class="card bg-primary">
                      <div class="card-body text-center">
                          <div class="list-group">
                              <a href="#" class="list-group-item list-group-item-action">First item</a>
                              <a href="#" class="list-group-item list-group-item-action">Second item</a>
                              <a href="#" class="list-group-item list-group-item-action">Third item</a>
                              <a href="#" class="list-group-item list-group-item-action">Four item</a>
                              <a href="#" class="list-group-item list-group-item-action">Five item</a>
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
                                                  <!--<locale-dropdown />-->
                                                  <!--<select class="form-control" id="eng-to">-->
                                                      <!--<option>Eng</option>-->
                                                      <!--<option>Es</option>-->
                                                      <!--<option>Rus</option>-->
                                                  <!--</select>-->
                                                  <lang-select></lang-select>
                                              </div>
                                              <!--<div class="col">-->
                                                  <!--<i class='far fa-hand-point-right' style='font-size:38px;color:blue' ></i>-->
                                              <!--</div>-->
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
    firstLetters: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
  }),

  methods: {
    letterClick (letter) {
      this.search_word += letter;
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
