<template>
  <div class="container">
    <div class="resume-generator">
      <app-block-generator :placeholder="getPlaceholder" @addBlock="addBlock" @changeBlockType="setBlockType" ></app-block-generator>
      <app-block-resume :resumeElements="resumeElements"></app-block-resume>
    </div>
    <app-comments :users="users" @loadComments="loadComments"></app-comments>
  </div>
</template>

<script>
import AppBlockGenerator from './components/AppBlockGenerator.vue'
import AppBlockResume from './components/AppBlockResume.vue'
import AppComments from './components/AppComments.vue'
import axios from 'axios'

export default {
  data () {
    return {
      resumeElements: 0,
      users: [],
      blockType: 'h1'
    }
  },
  methods: {
    addBlock (blockType, textValue) {
      const blockResume = document.querySelector('.resume')
      const newElement = document.createElement(blockType)
      newElement.classList.add('remove')
      const hr = document.createElement('hr')
      if (blockType === 'img') {
        newElement.src = textValue
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
      return ''
    }
  },
  components: { AppBlockGenerator, AppBlockResume, AppComments }
}
</script>

<style>
</style>
