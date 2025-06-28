<template>
  <header class="header">
    <nav class="main-nav">
      <div class="nav-left">
        <button 
          @click="handleConnection" 
          class="connect-button"
          :class="{ 'connected': connected }"
        >
          {{ connected ? accountDisplay : "连接钱包" }}
        </button>
      </div>
      <div class="nav-center">
        <img src="../assets/image/loge.png" alt="Logo" class="logo-image">
      </div>
      <div class="nav-right">
        <div class="search-box">
          <input type="text" placeholder="搜索文章..." class="search-input">
          <button class="search-button">
            <span class="search-icon">🔍</span>
          </button>
        </div>
      </div>
    </nav>
  </header>
</template>

<script setup>
import { ethers } from "ethers";
import { ref, onMounted, computed } from "vue";
import { useRouter, useRoute } from "vue-router";
import useContentReviewDAO from "../composables/useContentReviewDAO";

const router = useRouter();
const route = useRoute();
// 用户是否已连接钱包
const connected = ref(false);
// 连接的账户地址
const account = ref("");
// 是否是审核员
const isReviewer = ref(false);

// 计算属性：显示账户地址的缩略形式
const accountDisplay = computed(() => {
  if (!account.value) return "";
  return `${account.value.substring(0, 6)}...${account.value.substring(38)}`;
});

// 计算属性：判断是否在审核页面
const isReviewerPage = computed(() => {
  return route.path === '/Reviewer' || route.path === '/noReviewer';
});

// 处理钱包连接
const handleConnection = async () => {
  try {
    if (!connected.value) {
      if (window.ethereum) {
        // 使用ethers v5或v6版本的方式创建provider
        let provider;
        try {
          // 尝试ethers v6的方式
          provider = new ethers.BrowserProvider(window.ethereum);
          const signer = await provider.getSigner();
          account.value = await signer.getAddress();
          connected.value = true;
        } catch (e) {
          // 回退到ethers v5的方式
          provider = new ethers.providers.Web3Provider(window.ethereum);
          const accounts = await provider.send("eth_requestAccounts", []);
          account.value = accounts[0];
          connected.value = true;
        }
        console.log("钱包连接成功:", account.value);
        // 连接成功后检查是否是审核员
        checkReviewerStatus();
      } else {
        alert("请安装MetaMask钱包!");
      }
    } else {
      // 如果已连接，用户点击按钮则断开连接
      connected.value = false;
      account.value = "";
      isReviewer.value = false;
      console.log("钱包已断开连接");
      router.push("/Home");
    }
  } catch (error) {
    console.error("连接钱包时出错:", error);
    alert("连接钱包失败，请重试!");
  }
};

// 组件挂载时检查钱包是否已连接
onMounted(async () => {
  try {
    if (window.ethereum) {
      let provider;
      try {
        // 尝试ethers v6的方式
        provider = new ethers.BrowserProvider(window.ethereum);
        const accounts = await provider.listAccounts();
        if (accounts.length > 0) {
          account.value = accounts[0].address; // v6中账户是对象，需要获取address属性
          connected.value = true;
          console.log("已检测到连接的钱包:", account.value);
          // 检查是否是审核员
          checkReviewerStatus();
        }
      } catch (e) {
        // 回退到ethers v5的方式
        provider = new ethers.providers.Web3Provider(window.ethereum);
        const accounts = await provider.listAccounts();
        if (accounts.length > 0) {
          account.value = accounts[0];
          connected.value = true;
          console.log("已检测到连接的钱包:", account.value);
          // 检查是否是审核员
          checkReviewerStatus();
        }
      }

      // 监听账户变化
      window.ethereum.on("accountsChanged", (accounts) => {
        if (accounts.length > 0) {
          account.value = accounts[0];
          connected.value = true;
          console.log("钱包账户已切换:", account.value);
          // 账户变化时重新检查是否是审核员
          checkReviewerStatus();
        } else {
          account.value = "";
          connected.value = false;
          isReviewer.value = false;
          console.log("钱包已断开连接");
        }
      });
    }
  } catch (error) {
    console.error("检查钱包连接状态时出错:", error);
  }
});

// 检查用户是否是审核员
const checkReviewerStatus = async () => {
  try {
    if (connected.value && account.value) {
      const dao = useContentReviewDAO();
      if (dao) {
        isReviewer.value = await dao.isAddressReviewer(account.value);
        console.log("当前用户是否为审核员:", isReviewer.value);
      }
    }
  } catch (error) {
    console.error("检查审核员状态时出错:", error);
  }
};

// 处理审核链接点击事件
const handleModerationClick = () => {
  if (!connected.value) {
    alert("请先连接钱包");
    return;
  }
  
  if (isReviewer.value) {
    router.push("/Reviewer");
  } else {
    router.push("/noReviewer");
  }
};
</script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background-color: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  height: var(--header-height);
  display: flex;
  align-items: center;
}

.main-nav {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100%;
}

.nav-left {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding-left: 2rem;
}

.nav-center {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-image {
  height: 48px;
  width: auto;
}

.nav-right {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding-right: 2rem;
}

.search-box {
  display: flex;
  align-items: center;
  background: #f2f2f2;
  border-radius: 20px;
  padding: 0.25rem 0.75rem;
}

.search-input {
  border: none;
  background: transparent;
  outline: none;
  padding: 0.5rem 0.5rem;
  font-size: 1rem;
  width: 180px;
}

.search-button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.1rem;
  margin-left: 0.25rem;
}

.connect-button {
  background: var(--primary-gradient);
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 16px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.connect-button.connected {
  background: var(--primary-hover);
}
</style>
