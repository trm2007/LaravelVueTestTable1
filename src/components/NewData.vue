<template>
  <div class="">

    <form @submit="submitForm" class="form">
      <div class="form-row">
        <div class="form-group col-2">
            <label for="date">Дата</label>
            <input name="date" id="date" type="date" required pattern="[0-9]{4}-[0-9]{2}-[0-9]{2}" class="form-control" v-model="date">
        </div>
        <div class="form-group col-6">
            <label for="text">Текст</label>
            <input name="text" id="text" type="text" class="form-control" v-model="text">
        </div>
        <div class="form-group col-3">
            <label for="digits">Цифры</label>
            <input name="digits" id="digits" type="number" class="form-control" v-model="digits">
        </div>
        <div class="col-1">
          <button type="submit" class="btn" :class="originalDate ? 'btn-success' : 'btn-primary'">
              {{ originalDate ? 'Сохранить' : 'Добавить' }}
          </button>
        </div>          
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'NewData',
  props: {
    originalDate: { type: String, dafault: ""}, 
    originalText: { type: String, default: ""}, 
    originalDigits: {type: Number, default: 0}
  },
  data() {
    return {
        date: this.originalDate, 
        text: this.originalText, 
        digits: this.originalDigits
    }
  },
  watch: {
      originalDate(date){ this.date = date; },
      originalText(text){ this.text = text; },
      originalDigits(digits){ this.digits = digits; },
  },
  methods: {
      submitForm(form){
        form.preventDefault();

        this.$emit("new-data", {
            "date": this.date, 
            "text": this.text, 
            "digits": this.digits
        });
      }
  }
}
</script>

