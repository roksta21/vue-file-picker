<template>
  <div class="vue-file-picker">
    <a href="#" @click.prevent="clear" class="clear_input" v-if="image != ''">clear</a>
    <div class="image-select" @click="selectImage">
      <template v-if="image == ''">
      <p>Click here to select a file.</p>
      </template>
      <template v-else>
        <img id="vue_single_file_input_preview" src="" alt="" :style="`display: ${multiple ? 'none': '' };`">
        <div id="vue_multiple_file_input_preview"></div>
      </template>
    </div>
    <input type="file" style="display: none;" id="vue-input-type-file" ref="vue_file_picker_image" @input="handle" :multiple="multiple">
  </div>
</template>

<script>
export default {
  data() {
    return {
      image: this.value
    }
  },

  props: {
    value: {
      type:  Object | Array | String,
      default() { return {} }
    },
    multiple: {
      type: Boolean,
      default: false
    }
  },

  methods: {
    clearCurrent() {
      let current = document.getElementById("vue_multiple_file_input_preview");

      if (current) {
        current.innerHTML = '';
      }
    },

    clear() {
      this.$emit('input', '');
      this.image = '';
    },

    handle() {
      this.clearCurrent();

      let images = this.$refs.vue_file_picker_image.files;

      if (this.multiple) {
        this.image = images;
        this.previewMultipleImages();
        this.$emit('input', Object.values(images));
        this.$emit('change')
      } else {
        this.image = images[0];
        this.previewSingleImage();
        this.$emit('input', images[0]);
        this.$emit('change')
      }
    },

    previewMultipleImages() {
      Object.values(this.image).forEach(image => {
        var reader = new FileReader();

        reader.onload = function (e) {
          let img = document.createElement('img');
          img.src = e.target.result;
          img.alt = 'image';
          img.style = 'height: 100px; padding: 10px;'

          document.getElementById("vue_multiple_file_input_preview").append(img);
        };

        reader.readAsDataURL(image);
      });
    },

    previewSingleImage() {
      var reader = new FileReader();

      reader.onload = function (e) {
        document.getElementById("vue_single_file_input_preview").src = e.target.result;
      };

      reader.readAsDataURL(this.image);
    },

    selectImage() {
      document.getElementById('vue-input-type-file').click();
    }
  }
}
</script>

<style scoped>
.vue-file-picker .image-select {
  height: 100px;
  border: solid 1px #ddd;
  cursor: pointer;
}
.vue-file-picker .image-select p {
  text-align: center;
  line-height: 84px;
}
#vue_single_file_input_preview {
  height: 100px;
  padding: 10px;
}
.vue-file-picker .clear_input {
  float: right;
}
</style>