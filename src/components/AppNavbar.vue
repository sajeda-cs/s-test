 <!-- AppNavbar -->
<template>
  <div class="navbar-container">
    <nav class="navbar">
      <div class="left">
        <i class="bi bi-search"></i>
        <i class="bi bi-ui-radios-grid"></i>
      </div>
       
      <div class="right">
        <i class="bi bi-moon"></i>
        <i class="bi bi-bell"></i>
        <img src="@/assets/flag.svg" alt="flag" class="icon">
        <div class="profile-section">
          <img src="@/assets/Mathew.svg" alt="profile" class="profile-pic">
           <i class="bi bi-chevron-down"></i>
        </div>
      </div>
    </nav>
  </div>
</template>

<style scoped>
.navbar-container {
  position: fixed;
  top: 0;
  left: 180px; 
  right: 0;
  z-index: 99;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #fff;
  padding: 8px 18px; 
  box-shadow: 0 2px 5px #e6e1ff;
  height: 55px;
}
 
.left, .right {
  display: flex;
  align-items: center;
  gap: 12px;
}

.icon, .profile-pic {
  width: 28px; 
  height: 28px; 
  border-radius: 50%;
  object-fit: cover;
}

.icon {
  width: 20px;
  height: 20px;
}

.profile-section {
  display: flex;
  align-items: center;
}

</style>