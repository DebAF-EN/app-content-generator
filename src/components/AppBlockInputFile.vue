<template>
  <label class="input-file">
    <span class="input-file-text"></span>
    <input type="file" name="file" accept="image/jpeg, image/png" @change="insertFileName">
    <span class="input-file-btn">Выберите файл</span>
  </label>
</template>

<script>

export default {
  inject: ['emitFileUpload'],
  methods: {
    insertFileName (event) {
      const file = event.target.files[0]
      if (file) {
        event.target.closest('.input-file').querySelector('.input-file-text').innerHTML = file.name
      }
      this.emitFileUpload(file)
    }
  }
}
</script>

<style>
.input-file {
  position: relative;
  display: inline-block;
}
.input-file-text {
  padding: 0 10px;
  line-height: 40px;
  text-align: left;
  height: 40px;
  display: block;
  float: left;
  box-sizing: border-box;
  width: 200px;
  border-radius: 6px 0 0 6px;
  border: 1px solid #ddd;
  overflow: hidden;
}
.input-file-btn {
  position: relative;
  display: inline-block;
  cursor: pointer;
  outline: none;
  text-decoration: none;
  font-size: 14px;
  vertical-align: middle;
  color: rgb(255 255 255);
  text-align: center;
  border-radius: 0 4px 4px 0;
  background-color: #419152;
  line-height: 22px;
  height: 40px;
  padding: 10px 20px;
  box-sizing: border-box;
  border: none;
  margin: 0;
  transition: background-color 0.2s;
}
.input-file input[type=file] {
  position: absolute;
  z-index: -1;
  opacity: 0;
  display: block;
  width: 0;
  height: 0;
}

/* Focus */
.input-file input[type=file]:focus + .input-file-btn {
  box-shadow: 0 0 0 0.2rem rgba(0,123,255,.25);
}

/* Hover/Active */
.input-file:hover .input-file-btn {
  background-color: #59be6e;
}
.input-file:active .input-file-btn {
  background-color: #2E703A;
}

/* Disabled */
.input-file input[type=file]:disabled + .input-file-btn {
  background-color: #eee;
}
</style>
