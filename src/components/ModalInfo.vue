<template>
<div class="new-modal">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">New message</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true" @click="close">&times;</span>
        </button>
      </div>
      <div class="modal-body">

        <form @submit="submitForm" class="form">
        <div class="">
            <div class="form-group">
                <label for="message">Информация:</label>
                <textarea name="message" id="message" class="form-control" v-model="Message"></textarea>
            </div>
            <button type="submit" class="btn btn-success">
                Сохранить
            </button>          
        </div>
        </form>

        <ul>
          <li v-for="(Msg, i) in originalMessages" :key="i">
            <p>{{Msg}}</p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'ModalInfo',
  props: {
    originalMessages: { type: Array, dafault: []}
  },
  data() {
    return {
        Message: ""
    }
  },
  methods: {
      submitForm(form){
        form.preventDefault();

        this.$emit("new-message", this.Message);
      },
      close(){
          this.$emit("close");
      }
  }
}
</script>

<style scoped>
.new-modal{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0.9;
    background-color: darkslategrey;
    z-index: 9999;
    overflow: auto;
}
</style>