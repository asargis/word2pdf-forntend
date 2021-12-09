<template>
  <div class="uploader_wrapper">
    <div class="uploader_form">
      <input
        type="file"
        name="file"
        id="file"
        ref="file"
        @change="handleFileUpload"
        required
      >
      <input
        type="button"
        id="upload_file_btn"
        value="Загрузить файл"
        @click="submitFile"
      >
    </div>
  </div>
</template>

<script>

export default {
  name: "Uploader",
  data() {
    return {
      file: null,
    }
  },
  methods: {
    handleFileUpload() {
      if (this.$refs.file) {
        this.file = this.$refs.file.files[0];
      }
    },
    submitFile() {
      this.$emit('submit-file', this.file);
    }
  },
}
</script>

<style scoped lang="stylus">
.uploader_wrapper
  display flex
  justify-content center

.uploader_form
  display flex
  flex-direction column

  & > *
    margin-bottom 10px

</style>