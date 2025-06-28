<template>
  <aside class="sidebar">
    <nav class="sidebar-links">
      <router-link to="/Home" class="sidebar-link" active-class="active">
        <span class="sidebar-text">首页</span>
      </router-link>
      <router-link to="/post" class="sidebar-link" active-class="active">
        <span class="sidebar-text">发布</span>
      </router-link>
      <router-link to="/profile" class="sidebar-link" active-class="active">
        <span class="sidebar-text">个人中心</span>
      </router-link>
      <router-link to="/Reviewer" class="sidebar-link" active-class="active">
        <span class="sidebar-text">审核</span>
      </router-link>
    </nav>
  </aside>
</template>

<script setup>
</script>

<style scoped>
.sidebar {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding-top: 20px;
}

.sidebar-card {
  background: white;
  border-radius: 8px;
  padding: 1.25rem;
  margin-bottom: 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  border: 1px solid var(--border-color);
  min-height: 120px;
}

.sidebar-title {
  font-size: var(--font-size-sidebar);
  font-weight: 600;
  margin-bottom: 0.75rem;
  color: var(--text-color);
  height: 20px;
  line-height: 20px;
}

.sidebar-links {
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: center;
  width: 100%;
}

.sidebar-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: calc(100% + 20px);
  height: 50px;
  padding: 0;
  color: #333;
  text-decoration: none;
  border-radius: 10px;
  transition: all 0.2s ease;
  font-size: 1.1rem;
  font-weight: 400;
  font-family: 'KaiTi', '楷体', serif;
  box-sizing: border-box;
  background: rgba(179, 157, 219, 0.12);
  border: none;
}

.sidebar-link:hover {
  color: #5B3A8E;
  background-color: rgba(162, 147, 209, 0.18);
  transform: scale(1.05);
}

.sidebar-link.active {
  color: #5B3A8E;
  background-color: #b39ddb;
  font-weight: 800;
}

.sidebar-icon {
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-light);
  flex-shrink: 0;
}

.sidebar-text {
  flex: none;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 1.1rem;
  font-family: 'KaiTi', '楷体', serif;
  font-weight: 400;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.profile-text {
  flex-direction: column;
  align-items: center;
  justify-content: center;
  line-height: 1.1;
}

.sidebar-divider {
  height: 1px;
  background-color: var(--border-color);
  margin: 0.75rem 0;
}

.sidebar-tips {
  padding: 1rem;
  background-color: rgba(52, 152, 219, 0.1);
  border-radius: 6px;
  margin-top: 0.75rem;
}

.sidebar-tip-item {
  display: flex;
  align-items: flex-start;
  gap: 0.5rem;
  padding: 0.5rem 0;
  font-size: var(--font-size-sidebar);
  color: var(--text-color);
  min-height: 32px;
}

.sidebar-tip-item:last-child {
  padding-bottom: 0;
}

.sidebar-tip-icon {
  color: var(--primary-color);
  font-size: 1.1em;
  flex-shrink: 0;
}

.user-avatar {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  margin-bottom: 0.75rem;
  object-fit: cover;
}

.user-name {
  font-size: calc(var(--font-size-sidebar) + 2px);
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--text-color);
  line-height: 1.2;
}

.user-bio {
  font-size: var(--font-size-sidebar);
  color: var(--text-light);
  margin-bottom: 0.75rem;
  line-height: 1.4;
  height: 40px;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.sidebar-button {
  width: 100%;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--primary-gradient);
  color: white;
  border: none;
  border-radius: 6px;
  font-size: var(--font-size-sidebar);
  cursor: pointer;
  transition: all 0.2s ease;
}

.sidebar-button:hover {
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(41, 128, 185, 0.2);
}
</style> 