<template>
    <card :title="$t('Translate and add to dictionary')">
        <div class="container">
            <div class="row align-items-center">
                <div class="col-5">
                    <div class="form-inline">
                        <label class="mb-2 mr-sm-2" for="source">Source (eng):</label>
                        <input type="text" class="form-control" id="source"  v-model="word_from" placeholder="Enter word" name="source">
                    </div>
                </div>
                <div class="col-2">
                    <lang-select ref="curlang"></lang-select>
                </div>
                <div class="col-5">
                    <div class="form-inline">

                        <label class="mb-2 mr-sm-2" for="source">Translated:</label>
                        <input type="text" class="form-control mb-2 mr-sm-2" id="translated"  v-model="word_to" placeholder="" name="translated">
                        <button class="btn btn-primary mb-2 mr-sm-2"   @click="addTranslation"> + </button>

                    </div>
                </div>
            </div>
        </div>
        <!--DICTIONARY-->
    <div class="row align-items-center mt-4">

        <div class="col-6 offset-4">
            <div class="card dic-card-width"  v-for="(item,index) in dictionary" v-show="item.translations.length >0" >
                <div class="card-header back-color" >
                    <strong>{{item.lang_title_to}}</strong>
                </div>
                <ul class="list-group list-group-flush">
                    <li class="list-group-item">
                        <div class="row" v-for="translated_item in item.translations" >
                            <div class="col-5">{{translated_item.word_from}}</div>
                            <div class="col-2">
                                <i class='far fa-hand-point-right' style='font-size:18px;color:blue' ></i>

                            </div>
                            <div class="col-5">{{translated_item.word_to}}</div>
                        </div>
                    </li>
                </ul>

            </div>
            <div class="row" v-show="some_added && is_message==false">
                <div class="col-2  mt-2">
                    <button class="btn btn-primary mx-auto mt-1" @click="save">Save</button>
                </div>
                <div class="col-2 offset-3 mt-2">
                    <button class="btn btn-primary mx-auto mt-1" @click="clear">clear</button>
                </div>
            </div>

            <div class="row mt-2" v-show="is_message">
                <div class="alert alert-success ml-3">
                    <strong>Attention!</strong> {{message_text}}.
                </div>
            </div>
            <!--<InfoModal ref="modal"></InfoModal>-->

        </div>

    </div>
    </card>
</template>

<script>

  import LangSelect from '../components/LangSelect'
  //import InfoModal from '../components/InfoModal'
  import ModalComponent from 'vue-bootstrap4-modal'

  export default {
    components: {
      // InfoModal,
      ModalComponent,
      LangSelect
    },
    middleware: 'auth',

    data: () => ({
      cur_lang_from:'eng',
      cur_lang_to:'eng',
      word_from:'',
      word_to:'',
      dic_fields_obj:{lang_code_from:'eng',lang_code_to:'sp', lang_title_to:'Spanish:', translations:[]},
      dictionary:[],
      words_add_limit:5,
      words_cnt:0,
      some_added:false,
      is_message:false,
      message_text:''
    }),

    mounted () {
      this.initDictionary()
      this.initCurLang()
      self = this;
      this.$root.$on('changeCurLang',function (lang) {
        self.cur_lang_to = lang;
        console.log('Set cur_lang_to:',lang);
      });
    },

    methods: {
      initDictionary () {
        if (localStorage.getItem('dictionary')) {
            this.restoreDictionary();
        } else {
          this.dictionary = [ // -- translations:[{word_from:'table',word_to:'стол'}]
            {lang_code_from:'eng',lang_code_to:'sp', lang_title_to:'Spanish:', translations:[]},
            {lang_code_from:'eng',lang_code_to:'fr', lang_title_to:'French:', translations:[]},
            {lang_code_from:'eng',lang_code_to:'rus',lang_title_to:'Russian:', translations:[]}
          ];
        }

        console.log(this.dictionary);
      },

      restoreDictionary() {
        if (localStorage.getItem('dictionary'))  {
          this.dictionary = JSON.parse(localStorage.getItem('dictionary'));
        }
      },
      countDictionaryWords() {
        this.words_cnt = 0;
        for (let dic_item in this.dictionary) {
             let item = this.dictionary[dic_item];
             this.words_cnt = this.words_cnt + item.translations.length;
        }
        return this.words_cnt;
      },
      initCurLang() {
        this.cur_lang_to = this.$refs.curlang.getCurLang();
        console.log('Translate cur lang:'+this.cur_lang_to);
      },

      getCurLangTo() {
        return  this.cur_lang_to;
      },

      addTranslation() {
        if ((this.word_to == '')||(this.word_from == '')) {
          this.showMessage('Please fill in word!');
          return;
        }
        let translation = {
          word_from:this.word_from,
          word_to:this.word_to
        };
        for (let dic_item in this.dictionary) {
          let item = this.dictionary[dic_item];
          let words_length = item.translations.length;
          if (item.lang_code_to == this.cur_lang_to)  {
                if (words_length > this.words_add_limit) {
                  this.showMessage('Words limit is:'+this.words_add_limit);
                  break;
                }
                item.translations.push(translation);
                this.some_added = true;
                this.words_cnt++;
                console.log('added to :'+item.lang_code_to,translation);
          }
        }

      },

      save() {
        localStorage.setItem('dictionary', JSON.stringify(this.dictionary));
        //this.$refs.modal.showModal('Saved!');
        this.is_message = true;
        let mes_total ='Words:'+this.countDictionaryWords()+'';
        this.showMessage('Local dictionary has been saved!'+mes_total);
      },

      clear() {
        localStorage.removeItem('dictionary');
        this.initDictionary();
        this.showMessage('Local dictionary has been deleted!');
      },

      showMessage(mes_text) {
        self = this;
        self.message_text = mes_text;
        self.is_message = true;
        setInterval(function(){self.is_message = false;},5000);
      }

    },

    metaInfo () {
      return { title: this.$t('home') }
    }
  }
</script>
<style>
    .back-color {
        background-color: #b3d7ff;
    }
    .dic-card-width {
        width: 18rem;
    }
</style>
