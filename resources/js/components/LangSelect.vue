<template>
    <div class="row">
        <div class="col-8">
            <!--<locale-dropdown />-->
            <select class="form-control" id="eng-to" @change="onChangeCurLang($event)" v-model="cur_lang">
                <option
                        v-for="(item,index) in langList" :value=item.lang_code  :selected="index == 0" >
                        {{item.lang_code}}
                </option>
            </select>
        </div>
        <div class="col-2">
            <i class='far fa-hand-point-right' style='font-size:28px;color:blue' ></i>
        </div>
    </div>
</template>

<script>
export default {
    name: 'LangSelect',

    props: {
      title: { lang_selected: String, default: null }
    },

    data: () => ({
        cur_lang:'sp',
        langList: [
          // {lang_code:'eng',lang_title:'English'},
          {lang_code:'sp',lang_title:'Spanish'},
          {lang_code:'fr',lang_title:'French'},
          {lang_code:'rus',lang_title:'Russian'},
        ]
    }),

    methods: {
      getLangList () {
        return this.langList;
      },
      getCurLang () {
        return this.cur_lang;
      },
      onChangeCurLang (event) {
        console.log('Change cur_lang ',this.cur_lang);
        this.$root.$emit('changeCurLang',this.cur_lang);
      }
    }

}
</script>

<style scoped>

</style>
