<template>
    <div class="row">
        <div class="col-8">
            <!--<locale-dropdown />-->
            <select class="form-control" id="eng-to" >
                <option v-for="item in langList">{{item.lang_code}}</option>
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
        langList: [
          // {lang_code:'eng',lang_title:'English'},
          {lang_code:'sp',lang_title:'Spanish'},
          {lang_code:'fr',lang_title:'French'},
          {lang_code:'rus',lang_title:'Russian'},
        ]
    }),

}
</script>

<style scoped>

</style>
