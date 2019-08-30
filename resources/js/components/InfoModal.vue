<template id="info-modal">
    <!-- Modal -->
    <div class="modal fade" id="infoModal" tabindex="-1" role="dialog" aria-labelledby="infoModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="infoModalLabel">Info message:</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    {{info_message}}
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Save changes</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'InfoModal',

    data: () => ({
      info_message:'Info message'
    }),

    methods: {
      showModal(info_message) {
        //let element = this.$refs.modal.$el
        let element = document.getElementById('infoModal');
        $(element).modal('show')
      }
    }
  }
</script>

<style scoped>

</style>
