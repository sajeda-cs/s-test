 <!-- AppSidebar -->
<template>
  <div class="sidebar">
    <div class="sidebar-header">
      <img src="@/assets/pic.svg" alt="Logo" class="logo">
      <span class="team-text">HI TEAM</span>
    </div>

    <div class="list-group">
      <a href="#" class="list-group-item active">
        <i class="bi bi-columns-gap"></i><span>Dashboard</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-box"></i><span>Orders</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-people"></i><span>Passengers</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-truck"></i><span>Captains</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-clipboard-check"></i><span>Categories</span>
      </a>

      <hr class="divider">

      <a href="#" class="list-group-item">
        <i class="bi bi-file-text"></i><span>Settlement</span>
      </a>

      <hr class="divider">

      <a href="#" class="list-group-item">
        <i class="bi bi-envelope"></i><span>Contact</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-star"></i><span>Reviews</span>
      </a>

      <hr class="divider">

      <div class="dropdown">
        <a href="#" class="list-group-item dropdown-toggle" @click="toggleDropdown">
          <i class="bi bi-shield-lock"></i><span>Permissions</span>
        </a>
        <div v-if="isDropdownOpen" class="dropdown-menu">
          <a href="#" class="sub-item">User Roles</a>
          <a href="#" class="sub-item">Access Control</a>
        </div>
      </div>

      <a href="#" class="list-group-item">
        <i class="bi bi-translate"></i><span>Translation</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-film"></i><span>Education Video</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-gear"></i><span>Settings</span>
      </a>
      <a href="#" class="list-group-item">
        <i class="bi bi-square"></i><span>Content</span>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDropdownOpen: false
    };
  },
  methods: {
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    }
  }
};
</script>

<style scoped>
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: 180px;
  height: 100vh;
  background-color: #f8f9fa;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.05);
  padding: 6px;
  transition: width 0.3s ease;
  overflow-y: auto;
  z-index: 100;
}

.sidebar-header {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 0 6px 8px 6px;
  margin-bottom: 8px;
}

.logo {
  width: 40px;
  height: auto;
}

.team-text {
  font-size: 13px;
  color: #333;
  font-weight: bold;
  letter-spacing: 0.5px;
}

.list-group-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 7px 8px;
  color: #555;
  text-decoration: none;
  font-size: 12px;
  transition: background 0.2s, color 0.2s;
  border-radius: 6px;
  margin-bottom: 2px;
  border: none;
  width: calc(100% - 12px);
}

.list-group-item i {
  font-size: 16px;
  min-width: 18px;
  display: flex;
  justify-content: center;
}

.list-group-item:hover {
  background: #f5f5f5;
  color: #333;
}

.active {
  background: #6318f0;
  color: white;
}

.active i {
  color: white;
}

.divider {
  border: none;
  height: 1px;
  background: #585858;
  margin: 5px 0;
  width: 100%;
}

.dropdown-menu {
  display: flex;
  flex-direction: column;
  padding-left: 15px;
  transition: max-height 0.3s ease-in-out;
}

.sub-item {
  font-size: 12px;
  color: #666;
  padding: 7px 12px;
  margin-bottom: 2px;
}

.sub-item:hover {
  color: #333;
  background: #f5f5f5;
}
</style>
