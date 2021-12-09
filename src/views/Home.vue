<template>
  <div class="home">
    <div class="wrapper">
      <div class="uploader">
        <Uploader
          @submit-file="submitFile"
        />
      </div>
      <div class="edit-form" v-if="uploaded">
        <input
          v-model="fullname"
          type="text"
          name="fullname"
          placeholder="Введите ФИО"
        >
        <input type="button" value="Сохранить" @click="edit">
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import Uploader from "@/components/Uploader.vue";

export default {
  name: "Home",
  data() {
    return {
      uploaded: false,
      filename: null,
      fullname: null,
    }
  },
  components: {
    Uploader,
  },
  methods: {
    async submitFile(file) {
      let formData = new FormData();

      formData.append("file", file);

      formData.forEach(item => {
        console.log(item.size);
      });

      try {
        let response = await axios.post("http://localhost/api/upload",
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data'
            },
          }
        )
        if (response.data.status === 'success') {
          console.log(response.data);
          this.uploaded = true;
          this.filename = response.data.filename;
        } else {
          this.uploaded = false;
        }
      } catch (err) {
        console.error(err);
        this.uploaded = false;
      }
    },
    async edit() {
      try {
        let response = await axios.post("http://localhost/api/edit", {
          fullName: this.fullname,
          fileName: this.filename,
          },
          {
            responseType: 'arraybuffer'
          }
        )
        if (response.status === 200) {
          let blob = new Blob([response.data], { type:"application/pdf" })
          let link = document.createElement('a')
          link.href = window.URL.createObjectURL(blob)
          link.download = this.filename.split('.').slice(0, -1).join('.') + '.pdf'
          link.click()
        }
      } catch (err) {
        console.error(err);
        this.uploaded = false;
      }
    }
  },
};
</script>

<style lang="stylus" scoped>
.home
  display flex
  justify-content center

.wrapper
  display flex
  flex-flow column
  align-items flex-start

.edit-form
  margin-bottom 10px

  & > input
    margin-right 10px

</style>
