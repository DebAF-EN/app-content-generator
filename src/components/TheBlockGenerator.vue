<template>
  <div class="card block-generator-body">
    <div class="form-control">
      <label for="blockType">Тип блока</label>
      <select id="blockType" v-model="blockType" @change="$emit('changeBlockType', blockType)">
        <option value="h1">Заголовок</option>
        <option value="h2">Подзаголовок</option>
        <option value="img">Аватар</option>
        <option value="p">Текст</option>
        <option value="ul">Список</option>
      </select>
    </div>

    <div class="form-control">
      <label for="textValue">Значение</label>
      <textarea
        id="textValue"
        cols="30"
        rows="3"
        v-model="textValue"
        :placeholder="placeholder"
      ></textarea>
    </div>
    <app-block-input-file v-if="blockType==='img'"></app-block-input-file>
    <button
      class="btn"
      type="button"
      :disabled="textValue.length < 4 && hasDownloadImage === null"
      :class="{'primary': textValue.length >= 4 || hasDownloadImage !== null}"
      @click="addBlock"
    >Добавить</button>
  </div>
</template>

<script>
import AppBlockInputFile from '@/components/AppBlockInputFile'

export default {
  data () {
    return {
      blockType: 'h1',
      textValue: ''
    }
  },
  methods: {
    addBlock () {
      this.$emit('addBlock', this.blockType, this.textValue)
      this.textValue = ''
      this.blockType = 'h1'
    }
  },
  props: ['placeholder', 'hasDownloadImage'],
  emits: ['addBlock', 'changeBlockType'],
  components: { AppBlockInputFile }
}
</script>

<style scoped>

</style>
