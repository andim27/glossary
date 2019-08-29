<template>

  <card :title="$t('Glossary')">
    <div class="row">
      <div class="col-2"></div>
      <div class="col">
          <!--Search input-->
          <div class="row">
            <div class="input-group mb-3">
              <input type="text" class="form-control" placeholder="Search" v-model="search_word" @keydown.enter="search_word">
              <div class="input-group-append">
                <button class="btn btn-primary" type="submit" @click="searchWord">Go</button>
              </div>
            </div>
          </div>
         <!--Litera's list-->
          <div class="row">
              <ul class="list-group list-group-horizontal" v-for="(item,index) in firstLetters">
                  <li class="list-group-item border-none alfa-item" @click="letterClick(item)">{{item}}</li>
                  <li v-if="index == firstLetters.length - 1">
                      <i class="fas fa-backspace " @click="search_word =''"></i>
                  </li>
              </ul>
          </div>
          <!--Word list-->
          <div class="row"  v-show="isSearched == false">
              <div class="col-12">
                  <div class="alert alert-info">
                      <div v-if="dictionary != ''">
                          <strong>Local dictionary exist!</strong> Please search in ...
                      </div>
                      <div v-else >
                          <strong>Local dictionary is empty!</strong> Please add some new words
                          <router-link :to="{ name:'translate' }" class="navbar-brand">
                              here
                          </router-link>
                      </div>


                  </div>
              </div>
          </div>
          <div class="row" v-show="isSearched">
              <div class="col-12" v-show="searched_words.length >0">
                <div class="card-group" >
                  <div class="card bg-primary" >
                      <div class="card-body text-center">
                          <div class="list-group" v-for="word in searched_words">
                              <a href="#" class="list-group-item list-group-item-action" @keyup.enter="translateWord(word)" @click="translateWord(word)">{{word}}</a>
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
                                          <a href="#" class="list-group-item list-group-item-action">
                                             <span v-if="translated_word !='' " >{{translated_word}}</span>
                                             <span v-else> ??? </span>
                                          </a>

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
    cur_lang_to:'sp',
    dictionary:{},
    search_word:'',
    searched_words:[],
    translated_word:'',
    //  "abcdefghijklmnopqrstuvwxyz"
    firstLetters: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
    isSearched:false
  }),

  mounted () {
    this.getDictionary()
    self = this;
    this.$root.$on('changeCurLang',function (lang) {
      self.cur_lang_to = lang;
    });
  },

  methods: {
    getDictionary () {
      if (localStorage.getItem('dictionary'))  {
        this.dictionary = JSON.parse(localStorage.getItem('dictionary'));
      }
    },

    letterClick (letter) {
      this.search_word += letter.toLowerCase();
    },
    searchWord () {
      //--get local dictionary
      //--search
      this.searched_words =[];
      this.translated_word = '';
      console.log('Start search....')
      this.isSearched = true;
      for (let dic_item in this.dictionary) {
           let item = this.dictionary[dic_item];
           console.log('Search in:',item.lang_code_to);
           for (let word_item in item.translations) {
             let   trans_item = item.translations[word_item];
             console.log('check word:',trans_item.word_from);
                //if (this.search_word == trans_item.word_from) {
                if ( trans_item.word_from.search(this.search_word) >=0) {
                  this.searched_words.push(trans_item.word_from);
                  this.isSearched = true;
                }
           }
      }
    },

    translateWord (word_from) {
      console.log('Try to find in dictionary...');
      this.translated_word = '';
      for (let dic_item in this.dictionary) {
        let item = this.dictionary[dic_item];
        if (item.lang_code_to != this.cur_lang_to) {
            continue;
        }
        console.log('Search in:',item.lang_code_to);
        for (let word_item in item.translations) {
          let   trans_item = item.translations[word_item];
          console.log('check word:'+trans_item.word_from+'  word_from:'+word_from);
          if (word_from == trans_item.word_from) {
              this.translated_word = trans_item.word_to;
              console.log('Found!');
          }
        }
      }
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
