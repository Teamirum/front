<template>
  <div class="main-container">
    <div class="content-wrapper">
      <header class="header">
        <img src="../assets/images/logo.png" alt="Logo" class="header-logo" />
      </header>
      <main class="main-content">
        <form @submit.prevent="handleSubmit">
          <div class="input-group">
            <input
              type="text"
              id="email"
              v-model="email"
              placeholder="아이디 입력"
              class="icon-input email-icon"
            />
          </div>
          <br />
          <div class="input-group">
            <div class="password-container">
              <input
                type="password"
                id="password"
                v-model="password"
                placeholder="비밀번호 입력"
                class="icon-input password-icon"
              />
              <a href="#" class="forgot-password">비밀번호를 잊으셨나요?</a>
            </div>
          </div>
          <br />
          <button type="submit" class="login-btn">로그인</button>
          <div class="signup-link">
            <p>처음 오셨나요?</p>
            <a @click.prevent="$router.push('/signup')" class="signup-btn">회원가입</a>
          </div>
        </form>
      </main>
      <hr>
      <!-- 간편 로그인 섹션 -->
      <div class="social-login">
        <p>간편 로그인</p>
        <div class="social-icons">
          <img
            src="../assets/images/kakao.png"
            alt="Kakao Login"
            class="social-icon"
            @click="kakaoLogin"
          />
          <img
            src="../assets/images/naver.png"
            alt="Naver Login"
            class="social-icon"
            @click="handleSocialLogin('naver')"
          />
          <img
            src="../assets/images/google.png"
            alt="Google Login"
            class="social-icon"
            @click="handleSocialLogin('google')"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '../api/axios.js'; // Axios 설정 파일을 임포트

export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    async handleSubmit() {
      const loginData = {
        memberId: this.email,
        password: this.password,
      };

      try {
        const response = await axios.post("/member/login", loginData);
        if (response.data.isSuccess) {
          localStorage.setItem("accessToken", response.data.result.accessToken);
          this.$router.push("/");
        } else {
          alert(`${response.data.message || "알 수 없는 오류"}`);
        }
      } catch (error) {
        console.error("로그인 오류:", error);
        if (error.response) {
          alert(`${error.response.data.message || "알 수 없는 오류"}`);
        } else {
          alert("서버에 연결할 수 없습니다. 네트워크를 확인해주세요.");
        }
      }
    },

    // 카카오 로그인 리다이렉트
    kakaoLogin() {
      const kakaoAuthUrl = `https://kauth.kakao.com/oauth/authorize?client_id=305950b46b9c5e693c2be97edfa59770&redirect_uri=http://localhost:5173/login&response_type=code`;
      window.location.href = kakaoAuthUrl;
    },

    async handleKakaoAuth(code) {
      try {
        const response = await axios.post(`/member/login/oauth2/kakao?code=${code}`);
        if (response.data.isSuccess) {
          localStorage.setItem("accessToken", response.data.result.accessToken);
          this.$router.push("/phone");
        } else {
          alert(`카카오 로그인 실패: ${response.data.message || "알 수 없는 오류"}`);
        }
      } catch (error) {
        console.error("카카오 로그인 오류:", error);
        alert("카카오 로그인 중 오류가 발생했습니다.");
      }
    },

    handleSocialLogin(platform) {
      let loginUrl = "";
      switch (platform) {
        case "naver":
          loginUrl = `https://nid.naver.com/oauth2.0/authorize?client_id=YOUR_NAVER_CLIENT_ID&redirect_uri=http://localhost:8080/login/oauth2/code/naver&response_type=code`;
          break;
        case "google":
          loginUrl = `https://accounts.google.com/o/oauth2/auth?client_id=YOUR_GOOGLE_CLIENT_ID&redirect_uri=http://localhost:8080/login/oauth2/code/google&response_type=code&scope=email`;
          break;
        default:
          alert("지원하지 않는 소셜 로그인입니다.");
          return;
      }
      window.location.href = loginUrl;
    },
  },
  async mounted() {
    const urlParams = new URLSearchParams(window.location.search);
    const code = urlParams.get("code");

    if (code) {
      await this.handleKakaoAuth(code); // handleKakaoAuth 메서드 호출
    }
  },
};
</script>

<style scoped>
/* 기존 스타일 유지 */

.content-wrapper {
  width: 100%;
  max-width: 1200px;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* header */
.header {
  color: white;
  text-align: center;
  padding: 10px 0;
  position: relative;
  margin-top: 60px;
  margin-bottom: 150px;
}
.header-logo {
  width: 300px;
  height: auto;
  position: absolute;
  top: 5px;
  left: 30px;
}

/* main-content */
.main-content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}
form {
  width: 300px;
}
.input-group {
  margin-bottom: 15px;
}
.input-group label {
  display: block;
  margin-bottom: 5px;
}
.input-group input {
  width: 100%;
  padding: 8px;
  padding-left: 40px; /* 아이콘 공간 */
  border: 1px solid #ddd;
  border-radius: 10px;
  background-position: 10px center;
  background-repeat: no-repeat;
  background-size: 20px 20px;
}

.email-icon {
  background-image: url("../assets/images/ID_Icon.png");
}

.password-icon {
  background-image: url("../assets/images/PW_Icon.png");
}

.password-container {
  position: relative;
  width: 100%;
}
.forgot-password {
  position: absolute;
  right: 0;
  bottom: -25px;
  font-size: 0.9em;
  color: #6981d9;
  text-decoration: none;
}
.forgot-password:hover {
  text-decoration: underline;
}
.login-btn {
  width: 100%;
  padding: 10px;
  background-color: #6981d9;
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  margin-top: 20px;
}
.signup-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}
.signup-btn {
  font-size: 0.9em;
  color: #6981d9;
  text-decoration: none;
  margin-top: 5px;
}
.signup-btn:hover {
  text-decoration: underline;
}

/* 간편 로그인 섹션 */
.social-login {
  text-align: center;
}

.social-login p {
  font-size: 1.1em;
  margin-bottom: 15px;
}

.social-icons {
  display: flex;
  justify-content: space-between; /* 아이콘 사이 간격을 넓힘 */
  width: 100%;
  max-width: 200px; /* 너비를 늘려 아이콘 간격 확보 */
  margin: 0 auto;
}

.social-icon {
  width: 50px;
  height: 50px;
}

hr {
  border: none;
  border-top: 1px solid #9d9d9d;
  margin: 20px auto; /* 수평 방향으로 가운데 정렬 */
  width: 80%;
  margin-bottom: 40px;
}
</style>
