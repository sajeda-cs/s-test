<!-- Chatview -->
<template>
     <!-- Chat Navbar -->
     <div class = "navbar-cont">
    <nav class="sub-navbar">
      <div class="chat-container">
        <div class="chat-header">
          <div class="left">
            <span>Chat</span>
          </div>
          <div class="right">
            <i class="bi bi-house-door"></i>
            <span class="separator">/</span>
            <span class="chat-text">Chat</span>
          </div>
        </div>
      </div>
    </nav>
     </div>

  <div class="container-fluid chat-cont">
    <div class="row"> 
      <!-- Sidebar for a chat -->
      <div class="col-md-3 chat-sidebar">
        <div class="user-profile d-flex align-items-center p-2">
          <div class="position-relative me-2">
            <img src="@/assets/Mathew.svg" alt="profile" class="profile-pic-small" />
            <div class="status-dot-small status-online"></div>
          </div>
          <div class="position-relative d-none d-md-block">
            <h6 class="mb-0 small">Matthew Anderson</h6>
            <span class="text-muted smaller">Marketing Director</span>
          </div>
          <div class="actions-menu ms-auto">
            <i class="bi bi-three-dots-vertical"></i>
          </div>
        </div>
        
        <div class="search-bar p-2">
          <div class="input-group input-group-sm">
            <span class="input-group-text bg-white border-end-0 py-1"><i class="bi bi-search small"></i></span>
            <input type="text" class="form-control form-control-sm border-start-0 py-1" placeholder="Search contacts" />
          </div>
        </div>

        <div class="recent-chats-header px-2 py-1">
          <span class="text-muted smaller">Recent Chats</span>
          <i class="bi bi-chevron-down ms-1 smaller"></i>
        </div>
        
        <div class="chat-list">
          <div v-for="chat in chats" :key="chat.id" 
               class="chat-item d-flex align-items-center p-2" 
               :class="{ 'active': chat.id === selectedChat }">
            <div class="position-relative me-2">
              <img :src="chat.image" :alt="chat.name" class="chat-img-small" />
              <div v-if="chat.status" :class="getStatusClass(chat.status)" class="status-dot-small"></div>
            </div>
            <div class="chat-info flex-grow-1 d-flex flex-column">
              <div class="d-flex justify-content-between w-100">
                <h6 class="mb-0 smaller">{{ chat.name }}</h6>
                <span class="chat-time text-muted smaller">{{ chat.time }}</span>
              </div>
              <span class="message-preview text-muted smallest">{{ chat.lastMessage }}</span>
            </div>
            <span v-if="chat.unread" class="badge bg-primary rounded-pill badge-small ms-1">{{ chat.unread }}</span>
          </div>
        </div>
      </div>
      
      <!-- Chat in the middle -->
      <div class="col-md-6 chat-main p-0">
        <div class="chat-h p-2 d-flex align-items-center border-bottom">
          <div class="d-flex align-items-center">
            <div class="position-relative me-2">
              <img src="@/assets/Michell.svg" alt="Andrew" class="chat-img-small" />
              <div class="status-dot-small status-online"></div>
            </div>
            <span class="text-muted smallest">Away</span>
          </div>
          <div class="chat-actions ms-auto">
            <button class="btn btn-icon" @click="showBlockModal = true"><i class="bi bi-slash-circle smaller"></i></button>
            <button class="btn btn-icon" @click="showDeleteModal = true"><i class="bi bi-trash smaller"></i></button>
            <button class="btn btn-icon"><i class="bi bi-telephone smaller"></i></button>
            <button class="btn btn-icon"><i class="bi bi-camera-video smaller"></i></button>
            <button class="btn btn-icon" @click="showUnlockModal = true"><i class="bi bi-list smaller"></i></button>
          </div>
        </div>
        
        <div class="chat-messages">
          <div v-for="(message, index) in messages" :key="message.id" class="message-container">
            <div v-if="message.sender === 'other'" class="d-flex align-items-start mb-1">
              <div class="position-relative me-2">
                <img src="@/assets/Andrew.svg" alt="Andrew" class="chat-img-small" />
                <div class="status-dot-small status-offline"></div>
              </div>
              <div>
                <span class="text-muted smallest d-block mb-0">Andrew · {{ getMessageTime(index) }}</span>
                <div v-if="message.text" class="message receiver">
                  <p class="mb-0 small">{{ message.text }}</p>
                </div>
                <img v-if="message.img" :src="require(`@/assets/${message.imgSrc || '1.svg'}`)" class="message-image mt-2 rounded" alt="Image" />
              </div>
            </div>
            
            <div v-else class="d-flex justify-content-end mb-1">
              <div class="text-end">
                <div v-if="message.text" class="message sender">
                  <p class="mb-0 small">{{ message.text }}</p>
                </div>
                <span class="text-muted smallest d-block mt-1">{{ getMessageTime(index) }}</span>
              </div>
            </div>
          </div>
        </div>
        
        <div class="chat-input p-2 d-flex align-items-center">
          <button class="btn btn-light rounded-circle btn-sm me-2"><i class="bi bi-emoji-smile smaller"></i></button>
          <div class="input-group">
            <input type="text" class="form-control form-control-sm" placeholder="Type a Message..." />
            <button class="btn btn-light btn-sm"><i class="bi bi-image"></i></button>
          </div>
          <button class="btn btn-light rounded-circle btn-sm ms-2"><i class="bi bi-paperclip smaller"></i></button>
          <button class="btn btn-light rounded-circle btn-sm ms-2"><i class="bi bi-mic smaller"></i></button>
        </div>
          
        <!-- Block User Modal -->
        <div v-if="showBlockModal" class="modal-overlay">
          <div class="block-modal">
            <div class="block-icon">
              <i class="bi bi-slash-circle"></i>
            </div>
            <h5 class="modal-title">Block User</h5>
            <p class="modal-text">You Are Going To Block This User, Are You Sure?</p>
            <div class="modal-actions">
              <button class="btn-cancel" @click="showBlockModal = false">Cancel</button>
              <button class="btn-confirm" @click="blockUser">Yes</button>
            </div>
          </div>
        </div>
        
        <!-- Delete Conversation Modal -->
        <div v-if="showDeleteModal" class="modal-overlay">
          <div class="block-modal">
            <div class="block-icon">
              <i class="bi bi-trash"></i>
            </div>
            <h5 class="modal-title">Delete Conversation</h5>
            <p class="modal-text">Once You Sure To Delete This Conversation, You Will Not Be Able To Undo This Action</p>
            <div class="modal-actions">
              <button class="btn-cancel" @click="showDeleteModal = false">Cancel</button>
              <button class="btn-confirm" @click="deleteConversation">Yes</button>
            </div>
          </div>
        </div>
        
        <!-- Unlock User Modal -->
        <div v-if="showUnlockModal" class="modal-overlay">
          <div class="block-modal">
            <div class="block-icon">
              <i class="bi bi-unlock"></i>
            </div>
            <h5 class="modal-title">Unlock User</h5>
            <p class="modal-text">You Are Going To Unblock This User, Are You Sure?</p>
            <div class="modal-actions">
              <button class="btn-cancel" @click="showUnlockModal = false">Cancel</button>
              <button class="btn-confirm" @click="unlockUser">Yes</button>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Media -->
      <div class="col-md-3 media-sidebar p-3">
        <div class="media-section mb-4">
          <div class="d-flex justify-content-between align-items-center mb-3">
           <p class="mb-0">Media <span class="text-muted smaller">(36)</span></p>     
      </div>
          <div class="media-grid">
            <div class="row g-2">
              <div class="col-4">
                <div class="media-item">
                  <img src="@/assets/1.svg" alt="1" class="img-fluid rounded">
                </div>
              </div>
              <div class="col-4">
                <div class="media-item">
                  <img src="@/assets/2.svg" alt="2" class="img-fluid rounded">
                </div>
              </div>
              <div class="col-4">
                <div class="media-item">
                  <img src="@/assets/3.svg" alt="3" class="img-fluid rounded">
                </div>
              </div>
              <div class="col-4">
                <div class="media-item">
                  <img src="@/assets/4.svg" alt="4" class="img-fluid rounded">
                </div>
              </div>
              <div class="col-4">
                <div class="media-item">
                  <img src="@/assets/1.svg" alt="1" class="img-fluid rounded">
                </div>
              </div>
              <div class="col-4">
                <div class="media-item">
                  <img src="@/assets/2.svg" alt="2" class="img-fluid rounded">
                </div>
              </div>
            </div>
          </div>
        </div>
          
        <!-- Files -->
        <div class="files-section mb-4">
          <div class="d-flex justify-content-between align-items-center mb-3">
             <p class="mb-0">Files <span class="text-muted smaller">(36)</span> </p>
          </div>
          <div class="file-list">
            <div class="file-item p-2 mb-2">
              <div class="d-flex align-items-center">
                <div class="file-icon me-2">
                  <img src="@/assets/s.svg" alt="PDF" class="rounded" />
                </div>
                <div class="file-info">
                  <p class="mb-0 file-name">service-task.pdf</p>
                  <small class="text-muted">2 MB · 2 Dec 2022</small>
                </div>
              </div>
            </div>
            <div class="file-item p-2 mb-2">
              <div class="d-flex align-items-center">
                <div class="file-icon me-2">
                  <img src="@/assets/h.svg" alt="Fig" class="rounded" />
                </div>
                <div class="file-info">
                  <p class="mb-0 file-name">homepage-design.fig</p>
                  <small class="text-muted">3 MB · 2 Dec 2022</small>
                </div>
              </div>
            </div>
            <div class="file-item p-2 mb-2">
              <div class="d-flex align-items-center">
                <div class="file-icon me-2">
                  <img src="@/assets/a.svg" alt="HTML" class="rounded" />
                </div>
                <div class="file-info">
                  <p class="mb-0 file-name">about-us.html</p>
                  <small class="text-muted">2 MB · 2 Dec 2022</small>
                </div>
              </div>
            </div>
            <div class="file-item p-2 mb-2">
              <div class="d-flex align-items-center">
                <div class="file-icon me-2">
                  <img src="@/assets/w.svg" alt="ZIP" class="rounded" />
                </div>
                <div class="file-info">
                  <p class="mb-0 file-name">work-project.zip</p>
                  <small class="text-muted">2 MB · 2 Dec 2022</small>
                </div>
              </div>
            </div>
            <div class="file-item p-2 mb-2">
              <div class="d-flex align-items-center">
                <div class="file-icon me-2">
                  <img src="@/assets/c.svg" alt="JS" class="rounded" />
                </div>
                <div class="file-info">
                  <p class="mb-0 file-name">custom.js</p>
                  <small class="text-muted">2 MB · 2 Dec 2022</small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedChat: 1,
      showBlockModal: false,
      showDeleteModal: false,
      showUnlockModal: false,
      chats: [
        { 
          id: 1, 
          name: "Michell Flintoff", 
          lastMessage: "Your Yesterday was great...", 
          time: "15 minutes", 
          image: require('@/assets/Michell.svg'), 
          unread: 0,
          status: 'online'
        },
        { 
          id: 2, 
          name: "Bianca Anderson", 
          lastMessage: "Nice looking dress you...", 
          time: "30 minutes", 
          image: require('@/assets/Bianca.svg'), 
          unread: 0,
          status: 'offline'
        },
        { 
          id: 3, 
          name: "Andrew Johnson", 
          lastMessage: "Sent a photo", 
          time: "2 hours", 
          image: require('@/assets/Andrew.svg'), 
          unread: 0,
          status: 'away'
        },
        { 
          id: 4, 
          name: "Mark Strokes", 
          lastMessage: "Lorem ipsum text and...", 
          time: "5 days", 
          image: require('@/assets/Mark.svg'), 
          unread: 0,
          status: 'online'
        },
        { 
          id: 5, 
          name: "Mark, Stoinus & Rish...", 
          lastMessage: "Lorem ipsum text...", 
          time: "5 days", 
          image: require('@/assets/Mark&.svg'), 
          unread: 0,
          status: 'online'
        },
        { 
          id: 6, 
          name: "Bianca Anderson", 
          lastMessage: "Nice looking dress you...", 
          time: "30 minutes", 
          image: require('@/assets/Bianca.svg'), 
          unread: 0,
          status: 'offline'
        },
      ],
      messages: [
        { id: 1, sender: 'other', text: "If I don't like something, I'll stay away from it." },
        { id: 2, sender: 'me', text: "If I don't like something, I'll stay away from it." },
        { id: 3, sender: 'other', text: "I want more detailed information." },
        { id: 4, sender: 'me', text: "If I don't like something, I'll stay away from it." },
        { id: 5, sender: 'me', text: "They got there early, and they got really good seats." },
        { id: 6, sender: 'other', img: true, imgSrc: "1.svg" },
      ]
    };
  },
  methods: {
    getMessageTime(index) {
      const times = ["2 hours ago", "2 hours ago", "2 hours ago", "", "2 hours ago", "2 hours ago"];
      return times[index];
    },
    getStatusClass(status) {
      switch(status) {
        case 'online': return 'status-online';
        case 'away': return 'status-away';
        case 'offline': return 'status-offline';
        default: return '';
      }
    },
    blockUser() {
      console.log('User has been blocked');
      this.showBlockModal = false;
    },
    deleteConversation() {
      console.log('Conversation has been deleted');
      this.showDeleteModal = false;
    },
    unlockUser() {
      console.log('User has been unlocked');
      this.showUnlockModal = false;
    }
  }
};
</script>

<style scoped>

.navbar-cont{
  margin-bottom: 0;
  padding: 0; 
  
}

.chat-cont {
  background-color: #ffff;
  padding: 0;
  border-radius: 8px;
  overflow: hidden;
  padding: 0 18px;
  margin-top: -40px; 
  position: relative;
}


.sub-navbar {
  display: flex;
  justify-content: center;
  margin-bottom: 0px;
  margin-top: 0px;
  width: 100%; 
  position: relative;
}

.chat-container {
  position: relative; 
  top: -50px;  
  width: 100%; 
  padding: 12px;
}


.chat-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: auto; 
  background: #ffff;
  padding: 10px 18px; 
  border-radius: 10px;
  margin-bottom: -5px;
  }

.chat-header .right {
  display: flex;
  align-items: center;
  gap: 6px;
}

.separator {
  color: #888;
}

.chat-text {
  background-color: #e6e1ff;
  color: #6318f0;
  padding: 3px 8px; 
  border-radius: 6px;
  font-weight: bold;
  font-size: 13px;
}  

.row {
  height: 100%;
  margin: 0;
}

.chat-sidebar {
  background: #ffff;
  border-right: 1px solid #eaedf2;
  height: 100%;
  overflow-y: auto;
  padding: 0;
  font-size: 14px;
}

.profile-pic-small {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  object-fit: cover;
}

.online-indicator-small {
  width: 8px;
  height: 8px;
  background-color: #4CAF50;
  border-radius: 50%;
  position: absolute;
  bottom: 0;
  right: 0;
}

.status-dot-small {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  position: absolute;
  bottom: 0;
  right: 0;
  border: 1px solid white;
}

.chat-img-small {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  object-fit: cover;
}

.smaller {
  font-size: 12px;
}

.smallest {
  font-size: 11px;
}

.badge-small {
  font-size: 10px;
  padding: 3px 5px;
}

.user-profile {
  padding: 10px;
}

.status-online {
  background-color: #4CAF50; 
}

.status-away {
  background-color: #FFC107; 
}

.status-offline {
  background-color: #F44336; 
}

.chat-main .status-offline {
  background-color: transparent;
}

.mb-0 {
  font-size: 14px;
}

.actions-menu {
  color: #6c757d;
  cursor: pointer;
}

.search-bar .form-control {
  background-color: #ffff;
  border-radius: 4px;
}

.recent-chats-header {
  display: flex;
  align-items: center;
  color: #6c757d;
  font-weight: 500;
}

.chat-list {
  overflow-y: auto;
}

.chat-item {
  border-bottom: 1px solid #eaedf2;
  cursor: pointer;
  transition: all 0.2s;
}

.chat-item:hover {
  background: #ffff;
}

.chat-item.active {
  background-color: #F4F7FB;
}

.chat-main {
  display: flex;
  flex-direction: column;
  background: #ffff;
  border-right: 1px solid #eaedf2;
  gap: 10px; 
  height: auto;
  position: relative;
}

.chat-header {
  background-color: #fff;
}

.profile-pic {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  object-fit: cover;
}

.btn-icon {
  width: 32px;
  height: 32px;
  padding: 0;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  margin-left: 5px;
  color: #6c757d;
  background: transparent;
  border: none;
}

.btn-icon .bi-slash-circle {
  color: #8A2BE2;
}

.btn-icon .bi-trash {
  color: #FF0000;
}

.btn-icon:hover {
  background-color: #f8f9fa;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  background-color: #ffff;
  padding: 0.75rem; 
  padding-bottom: 70px; 
}

.message-container {
  margin-bottom: 0.75rem; 
  height: auto;
}

.message {
  padding: 8px 12px; 
  border-radius: 12px;
  display: inline-block;
}

.message.receiver {
  background-color: #EFF4FA;
  color: #333;
}

.message.sender {
  background-color: #ddf3f3;
  color: #333;
}

.chat-avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  object-fit: cover;
}

.message-image {
  max-width: 200px;
  max-height: 150px;
  border-radius: 12px;
}

.chat-input {
  border-top: 1px solid #eaedf2;
  background: #ffff;
  margin-top: 10px; 
  position: relative;
  bottom: -150px; 
  display: auto;
  }

.chat-input .form-control {
  border-radius: 4px;
  background-color: #f9fafb;
}

.chat-input .btn {
  border: none;
  background: transparent;
}

.chat-input .btn-light {
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.media-sidebar {
  background: #ffff;
  height: 100%;
  overflow-y: auto;
  
}

.media-grid {
  margin-bottom: 20px;
}

.media-item {
  border-radius: 8px;
  overflow: hidden;
  height: 70px;
  position: relative;
  margin: 0;
}

.media-item img {
  width: 90%;
  height: 90%;
  object-fit: cover;
}

.file-item {
  border-radius: 8px;
  background-color: #ffff;
  border: 1px solid #eaedf2;
  transition: all 0.2s;
}

.file-item:hover {
  background-color: #ffff;
}

.file-icon img {
  width: 40px;
  height: 40px;
}

.file-info {
  flex: 1;
  overflow: hidden;
}

.file-name {
  font-weight: 500;
  font-size: 14px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.block-modal {
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  width: 350px;
  text-align: center;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.block-icon {
  margin-bottom: 20px;
}

.block-icon i {
  font-size: 30px;
  color: #6366f1;
  background-color: #ffff;
  border-radius: 50%;
  padding: 15px;
}

.modal-title {
  font-weight: 600;
  margin-bottom: 10px;
  font-size: 20px;
}

.modal-text {
  color: #6c757d;
  margin-bottom: 25px;
}

.modal-actions {
  display: flex;
  justify-content: space-between;
  gap: 15px;
}

.btn-cancel {
  flex: 1;
  padding: 12px;
  border: none;
  background-color: #9ca3af;
  color: white;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: background-color 0.2s;
}

.btn-cancel:hover {
  background-color: #8a8f98;
}

.btn-confirm {
  flex: 1;
  padding: 12px;
  border: none;
  background-color: #6366f1;
  color: white;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: background-color 0.2s;
}

.btn-confirm:hover {
  background-color: #4f46e5;
}

.message-preview {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 150px;
}

.truncate-text {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 120px;
}
</style>