  <template>
    <div class="card menu">
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

      <button
        class="btn"
        type="button"
        :disabled="textValue.length < 4"
        :class="{'primary': textValue.length >= 4}"
        @click="$emit('addBlock', blockType, textValue), textValue=''"
      >Добавить</button>
    </div>
  </template>

<script>
export default {
  data () {
    return {
      blockType: 'h1',
      textValue: ''
    }
  },
  props: ['placeholder'],
  emits: ['addBlock', 'changeBlockType']
}
</script>

<style scoped>

</style>
