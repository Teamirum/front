<template>
  <div class="main-container">
    <Header />
    <div class="transfer-page">
      <div class="transfer-form">
        <label for="recipient">
          <span class="point">얼마</span>를<br /> 보내겠습니까?
        </label>
        <input
          type="text"
          id="recipient"
          v-model="recipient"
          placeholder="보낼 금액"
          @input="validateInput"
          @keyup.enter="sendMoney"
        />
        
        <div class="amount-info">
          <span class="available-amount">이체 가능 금액</span>
          <span class="currency">원</span>
        </div>

        <button 
          type="button" 
          class="stroke-button" 
          @click="sendMoney"
        >
          송금하기
        </button>

      </div>
    </div>
    <FooterNav :buttonType="'pay'" :buttonAction="goToGroupPayPage" />
  </div>
</template>

<script>
import FooterNav from '../../components/FooterNav.vue';
import Header from '../../components/Header.vue';

export default {
  name: 'Transfer2',
  components: {
    FooterNav,
    Header,
  },
  data() {
    return {
      recipient: '',
    };
  },
  methods: {
    validateInput() {
      this.recipient = this.recipient.replace(/[^0-9]/g, '');
    },
    async sendMoney() {
      const amount = parseInt(this.recipient);
      if (amount >= 1) {
        // 송금 요청을 다음 페이지로 전달
        this.$router.push({ 
          path: '/transfer3',
        });
      } else {
        alert("보낼 금액은 1원 이상이어야 합니다."); // 조건에 맞지 않을 때 alert 추가
      }
    },
  },
};
</script>

<style scoped>
.transfer-page {
  max-width: 400px;
  border-radius: 10px;
  padding: 30px;
  text-align: left;
}

.point {
  color: #6981d9;
}

.transfer-form {
  margin-bottom: 70px;
}

label {
  font-size: 20px;
  font-weight: bold;
  display: block;
  margin-bottom: 10px;
}

input[type="text"] {
  width: 100%;
  padding: 10px 90px 3px 2px;
  border: none;
  border-bottom: 2px solid #6981d9;
  font-size: 16px;
  margin-bottom: 0; /* 여백을 없앰 */
  outline: none;
  background-color: transparent;
}

input[type="text"]::placeholder {
  color: #aaa;
}

.amount-info {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  margin-top: 5px;
  color: #aaa;
}

.stroke-button {
  background-color: #6981d9;
  color: white;
  border: 2px solid #6981d9;
  padding: 8px 13px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: bold;
  transition: background-color 0.3s, color 0.3s;
  margin-top: 20px; /* 버튼과 위 요소 사이의 여백 */
}

.stroke-button:hover {
  background-color: white;
  color: #6981d9;
}
</style>
