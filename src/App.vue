<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const comments = ref([])
const newCommentText = ref('')
const apiUrl = 'http://localhost:5231/api/testComment'
const userName = ref('Blend 285')

const fetchComments = async () => {
  try {
    const response = await axios.get(apiUrl)
    comments.value = response.data
  } catch (error) {
    console.error('Error fetching comments:', error)
    alert('Get comment Fail!')
  } 
}

const submitComment = async () => {
  if (!newCommentText.value.trim()) return

  try {
    await axios.post(apiUrl, {
      user: userName.value,
      message: newCommentText.value
    })
    newCommentText.value = ''
    await fetchComments()
  } catch (error) {
    console.error('Error posting comment:', error)
    alert('Summit comment Fail!')
  }
}

const formatNow = (date) => {
  return new Date(date).toLocaleString('en-GB', {
    day: '2-digit',
    month: 'long',
    year: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  });
}

onMounted(fetchComments)

</script>

<template>
  <div class="main-container">
    <!-- Header Bar -->
    <header class="top-header">IT 08-1</header>

    <!-- Header Post -->
    <div class="content-body">
      <div class="post-header">
        <div class="avatar blue">{{ userName.charAt(0).toUpperCase() }}</div>
        <div class="user-info">
          <span class="user-name">{{ userName }}</span>
          <span class="post-time">{{ formatNow(new Date()) }}</span>
        </div>
      </div>
    
      <!-- Image -->
      <div class="image-section">
        <img src="/img/dog2.jpg" alt="Dog">
      </div>

       <!-- Comment Box -->
      <div class="input-area">
        <div class="avatar blue">{{ userName.charAt(0).toUpperCase() }}</div>
        <input 
          v-model="newCommentText" 
          @keyup.enter="submitComment"
          placeholder="Comment"
          class="comment-input"
        />
      </div>
      
      <!-- Comment List -->
      <div class="comment-list">
        <div v-for="(item, index) in comments" :key="index" class="comment-item">
          <div class="avatar blue">
            {{ item.user.charAt(0).toUpperCase() }}
          </div>
          <div class="comment-content">
            <span class="user-name">{{ item.user }}</span>
            <div class="message-bubble">{{ item.message }}</div>
          </div>
        </div>
      </div>
  
    </div>
  </div>
</template>

<style>

body {
  font-family: sans-serif;
  background-color: #f0f2f5;
  margin: 0;
  padding: 0;
  width: 100%;
}
.main-container {
  width: 100%;
  min-height: 100vh;
  background: white;
  margin-top: 15px;
  padding: 0;
  align-items: center;
}

.top-header {
  background-color: #048000;
  color: white;
  text-align: center; 
  font-weight: bold;
  font-size: 1.2rem;
  width:100%;
  height:60px;
  position:fixed;
  top:0;
  left:0;
  padding: 15px 20px;
}

.content-body {
  width: 100%;
  padding: 20px;       /* เปลี่ยนจาก 10% เป็น 20px เพื่อให้เนื้อหาขยายเต็มจอ */
  box-sizing: border-box;
}

.image-section img {
  width: 100%;
  max-height: 600px;   /* เพิ่มความสูงหน่อยเพื่อให้สมดุลกับความกว้างที่มากขึ้น */
  object-fit: cover; 
  border-radius: 8px;
  display: block;
}

.comment-list { margin-top: 30px; margin-bottom: 20px; }

.comment-item { display: flex; gap: 10px; margin-bottom: 15px; align-items: flex-start; }

.avatar { width: 35px; height: 35px; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold; flex-shrink: 0; }
.avatar.blue { background-color: #4a90e2; }

.comment-content { display: flex; flex-direction: column; }

.message-bubble { background-color: #e9ebee; padding: 8px 15px; border-radius: 18px; font-size: 0.9rem; color: #1c1e21; }

.input-area { display: flex; align-items: center; gap: 10px; border-top: 1px solid #eee; padding-top: 15px; }
.comment-input { flex-grow: 1; background-color: #e9ebee; border: none; padding: 10px 15px; border-radius: 20px; outline: none; }

.post-header {
  display: flex;
  align-items: center;
  gap: 12px;          
  margin-bottom: 15px;
}
.user-info {
  display: flex;
  flex-direction: column;
  gap: 2px;             
}
.user-name {
  font-weight: bold;
  font-size: 1rem;
  color: #1c1e21;
}
.post-time {
  font-size: 0.75rem;
  color: #65676b;
  margin-top: -5px;
}
</style>