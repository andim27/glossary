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
                    <lang-select></lang-select>
                </div>
                <div class="col-5">
                    <div class="form-inline">

                        <label class="mb-2 mr-sm-2" for="source">Translated:</label>
                        <input type="text" class="form-control mb-2 mr-sm-2" id="translated"  v-model="word_to" placeholder="" name="translated">
                        <button class="btn btn-primary mb-2 mr-sm-2" @click="addTranslation"> + </button>

                    </div>
                </div>
            </div>
        </div>
        <!--DICTIONARY-->
    <div class="row align-items-center mt-4">

        <div class="col-6 offset-4">
            <div class="card dic-card-width" >
                <div class="card-header back-color">
                    <strong>Spanish</strong>
                </div>
                <ul class="list-group list-group-flush">
                    <li class="list-group-item">
                        <div class="row">
                            <div class="col-5">Table</div>
                            <div class="col-2">
                                <i class='far fa-hand-point-right' style='font-size:18px;color:blue' ></i>

                            </div>
                            <div class="col-5">Mesa</div>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="card dic-card-width" >
                <div class="card-header back-color">
                    <strong>French</strong>
                </div>
                <ul class="list-group list-group-flush">
                    <li class="list-group-item">
                        <div class="row">
                            <div class="col-5">Table</div>
                            <div class="col-2">
                                <i class='far fa-hand-point-right' style='font-size:18px;color:blue' ></i>

                            </div>
                            <div class="col-5">le tableau</div>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="card dic-card-width" >
                <div class="card-header back-color">
                    <strong>Russian</strong>
                </div>
                <ul class="list-group list-group-flush">
                    <li class="list-group-item">
                        <div class="row">
                            <div class="col-5">Table</div>
                            <div class="col-2">
                                <i class='far fa-hand-point-right' style='font-size:18px;color:blue' ></i>

                            </div>
                            <div class="col-5">Стол</div>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="row">
                <div class="col offset-2 mt-2">
                    <button class="btn btn-primary mx-auto mt-1">Save</button>
                </div>

            </div>

        </div>

    </div>
    </card>
</template>

<script>

  import LangSelect from '../components/LangSelect'
  export default {
    components: {
      LangSelect
    },
    middleware: 'auth',

    data: () => ({
      cur_lang_from:'eng',
      word_from:'',
      word_to:'',
      dictionary:[] //[{lang_code_from:'eng',lang_code_to:'rus',word_from:'table',word_to:'стол'}]
    }),

    methods: {
      getCurLangTo () {
        return  'fr';
      },
      addTranslation () {

        let translation = {
          lang_code_from:this.cur_lang_from,
          lang_code_to:this.getCurLangTo(),
          word_from:this.word_from,
          word_to:this.word_to
        };
        this.dictionary.push(translation);
        console.log(this.dictionary.length);
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
