<template>
  <div class="container">
    <div class="title">📝 文档智能问答 3.0（记忆+Rerank）</div>
    <div class="chat-box">
      <div v-for="(item, i) in list" :key="i" class="msg">
        <div v-if="item.role === 'user'" class="user-box">
          <span>👤</span>
          <div class="user">{{ item.content }}</div>
        </div>
        <div v-else class="ai-box">
          <span>🤖</span>
          <div class="ai">{{ item.content }}</div>
          <div class="source" v-for="(s, j) in item.sources" :key="j">
            来源：{{ s.file }} 第{{ s.page }}页
          </div>
        </div>
      </div>
    </div>
    <div class="input-area">
      <button @click="selectFile">📁 上传文档</button>
      <input v-model="msg" @keyup.enter="send" placeholder="输入问题..." />
      <button @click="send">🚀 发送</button>
      <input type="file" hidden ref="fileRef" @change="upload" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const msg = ref('')
const list = ref([])
const fileRef = ref(null)

const send = async () => {
  if (!msg.value) return
  const userMsg = { role: 'user', content: msg.value }
  list.value.push(userMsg)

  const history = list.value.map(m => ({
    role: m.role,
    content: m.content
  }))

  try {
    const res = await axios.post('/api/chat', {
      msg: msg.value,
      history: history
    })
    list.value.push({
      role: 'assistant',
      content: res.data.answer,
      sources: res.data.sources
    })
    msg.value = ''
  } catch (e) {
    list.value.push({ role: 'assistant', content: '请求失败' })
  }
}

const selectFile = () => fileRef.value.click()
const upload = async (e) => {
  const file = e.target.files[0]
  if (!file) return
  const fd = new FormData()
  fd.append('file', file)
  await axios.post('/api/upload', fd)
  alert('上传成功')
}
</script>

<style scoped>
.container { max-width: 900px; margin: 20px auto; padding: 20px; }
.title { font-size: 22px; text-align: center; margin-bottom: 20px; font-weight: bold; }
.chat-box { height: 500px; border: 1px solid #eee; border-radius: 10px; padding: 20px; overflow-y: auto; background: #f9f9f9; }
.msg { display: flex; margin-bottom: 16px; }
.user-box { justify-content: end; display: flex; width: 100%; }
.ai-box { justify-content: start; display: flex; width: 100%; }
.user { background: #42b983; color: white; padding: 10px 14px; border-radius: 16px; max-width: 70%; }
.ai { background: white; border: 1px solid #eee; padding: 10px 14px; border-radius: 16px; max-width: 70%; }
.source { font-size: 12px; color: #777; margin-top: 6px; }
.input-area { display: flex; gap: 10px; margin-top: 20px; }
input { flex: 1; padding: 12px; border: 1px solid #ddd; border-radius: 6px; }
button { padding: 12px 16px; background: #42b983; color: white; border: none; border-radius: 6px; cursor: pointer; }
</style>