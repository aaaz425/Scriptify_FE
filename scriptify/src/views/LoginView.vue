<template>
  <div class="login-container">
    <div class="login-box">
      <h1 class="login-title"></h1>
      <form @submit.prevent="handleLogin">
        <div class="form-group">
          <label for="email">이메일</label>
          <input
            type="email"
            id="email"
            v-model="email"
            placeholder="이메일을 입력해주세요."
          />
        </div>
        <div class="form-group">
          <label for="password">비밀번호</label>
          <input
            type="password"
            id="password"
            v-model="password"
            placeholder="비밀번호를 입력해주세요"
          />
        </div>
        <button type="submit" class="login-button">로그인</button>
      </form>
      <div class="login-footer">
        <p class="p-guide">
          Scripify가 처음이신가요?
          <RouterLink to="/signup" class="signup-link">회원가입</RouterLink>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useAuthStore } from "@/stores/auth";
import { useRouter } from "vue-router";

const router = useRouter();
const authStore = useAuthStore();
const email = ref("");
const password = ref("");

const handleLogin = () => {
  const requestUser = {
    email: email.value,
    password: password.value,
  };

  authStore.login(requestUser);
  router.push({ name: "home" });
};
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #d5c2b4;
  color: #fff;
}

.login-box {
  padding: 40px;
  border-radius: 20px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
  text-align: center;
  background: linear-gradient(to bottom, #d5c2b4, #f0e5dd);
  border: 1px ridge #d5c2b4;
}

.login-title {
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
  text-align: left;
}

label {
  display: block;
  font-size: 14px;
  margin-bottom: 8px;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  outline: none;
}

.login-button {
  width: 100%;
  background-color: #9f8d80;
  color: #fff;
  padding: 12px;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.login-button:hover {
  background-color: #7a695d;
}

.login-footer {
  margin-top: 20px;
  font-size: 14px;
}

.signup-link {
  color: #8d4925;
  text-decoration: none;
  font-weight: bold;
}

.signup-link:hover {
  text-decoration: underline;
}
.p-guide {
  color: #7a695d;
  text-decoration: none;
  font-weight: bold;
}
</style>
