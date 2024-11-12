<template>
  <div class="container">
    <div class="content-wrapper">
      <the-block-generator :hasDownloadImage="file" :placeholder="getPlaceholder" @addBlock="addBlock" @changeBlockType="setBlockType" ></the-block-generator>
      <div class="card block-resume" >
        <h2 v-if="resumeElements === 0">Добавь свой первый блок!<br> Если захочешь удалить - нажми на него</h2>
      </div>
    </div>
    <app-comments :users="users" @loadComments="loadComments"></app-comments>
  </div>
</template>

<script>
import TheBlockGenerator from './components/TheBlockGenerator.vue'
import AppComments from './components/AppComments.vue'
import axios from 'axios'

export default {
  data () {
    return {
      resumeElements: 0,
      users: [],
      blockType: 'h1',
      file: null
    }
  },
  methods: {
    addBlock (blockType, textValue) {
      const blockResume = document.querySelector('.block-resume')
      const newElement = document.createElement(blockType)
      newElement.classList.add('remove')
      const hr = document.createElement('hr')
      if (blockType === 'img') {
        if (this.file) {
          newElement.src = URL.createObjectURL(this.file)
          this.file = null
        } else {
          newElement.src = textValue
        }
        newElement.alt = 'Некорректная ссылка на изображение'
        newElement.classList.add('avatar')
        newElement.onerror = () => {
          newElement.classList.remove('avatar')
        }
      } else if (blockType === 'ul') {
        const itemsList = textValue.split('\n')
        itemsList.forEach(itemText => {
          const item = document.createElement('li')
          item.innerHTML = itemText
          newElement.appendChild(item)
        })
      } else {
        newElement.innerHTML = textValue
      }
      newElement.addEventListener('click', () => {
        blockResume.removeChild(newElement)
        if (blockType === 'h2') {
          blockResume.removeChild(hr)
        }
        this.resumeElements -= 1
      })
      blockResume.append(newElement)
      if (blockType === 'h2') {
        blockResume.append(hr)
      }
      this.resumeElements += 1
    },
    handleFileUpload (file) {
      this.file = file
    },
    setBlockType (blockType) {
      this.blockType = blockType
    },
    async loadComments () {
      try {
        const { data } = await axios.get('https://portfolio-comments-c7274-default-rtdb.firebaseio.com/mails.json')
        Object.keys(data).forEach(key => {
          this.users.push(data[key])
        })
      } catch (e) {
        console.log(e.message)
      }
    }
  },
  computed: {
    getPlaceholder () {
      if (this.blockType === 'img') {
        return 'Вставьте ссылку на изображение'
      } else if (this.blockType === 'ul') {
        return 'При вводе нажимайте Enter для отделения элемента списка'
      }
      return 'Введите текст'
    }
  },
  components: { TheBlockGenerator, AppComments },
  provide () {
    return {
      emitFileUpload: this.handleFileUpload
    }
  }
}
</script>

<style>
</style>
