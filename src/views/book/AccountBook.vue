<template>
  <div
    class="main-container d-flex flex-column justify-content-center align-items-center"
  >
    <div>
      <!-- CalendarComponent 또는 리스트 보기 -->
      <div v-if="isCalendarView">
        <CalendarComponent :year="selectedYear" :month="selectedMonth" />
      </div>
      <div v-else>
        <!-- 리스트 보기 -->
        <AccountBookList />
      </div>
    </div>

    <!-- 캘린더/리스트 전환 버튼 -->
    <div class="view-toggle">
      <button
        :class="{ active: isCalendarView }"
        @click="toggleView(true)"
        class="toggle-btn"
      >
        캘린더
      </button>
      <div class="divider-line"></div>
      <button
        :class="{ active: !isCalendarView }"
        @click="toggleView(false)"
        class="toggle-btn"
      >
        리스트
      </button>
    </div>

    <div class="divider"></div>
  </div>

  <!-- FooterNav 컴포넌트 사용 -->
  <FooterNav :buttonType="'plus'" :buttonAction="goToAddList" />
</template>

<script>
import "vue-cal/dist/vuecal.css";
import VueCal from "vue-cal";
import CalendarComponent from "../../components/CalendarComponent.vue"; // CalendarComponent를 import
import FooterNav from "../../components/FooterNav.vue";
import AccountBookList from "../../components/AccountBookList.vue"; // AccountBookList 컴포넌트 import

export default {
  name: "AccountBook",
  components: {
    FooterNav,
    VueCal,
    CalendarComponent,
    AccountBookList, // 리스트 보기 컴포넌트 추가
  },
  data() {
    return {
      selectedYear: new Date().getFullYear(),
      selectedMonth: new Date().getMonth() + 1,
      years: [],
      months: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
      isCalendarView: true, // 캘린더 보기 여부 상태 관리
    };
  },
  methods: {
    toggleView(isCalendar) {
      this.isCalendarView = isCalendar;
    },
    goToAddList() {
      this.$router.push("/addlist"); // AddList 페이지로 이동
    },
  },
};
</script>

<style scoped>
a {
  text-decoration: none;
}

.main-container {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  min-height: 100vh;
  background-color: white;
  overflow-y: auto;
  margin-bottom: 60px;
}

.calendar-container {
  text-align: center;
  font-family: "Poppins", sans-serif; /* Poppins 폰트 적용 */
}

.calendar {
  width: 100%;
  max-width: 800px;
  margin-bottom: 15px;
}

/* 캘린더/리스트 전환 버튼 스타일 */
.view-toggle {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 300px;
  height: 30px;
  margin: 20px 0;
  background-color: #d9d9d9;
  border-radius: 25px;
  padding: 5px;
}

.toggle-btn {
  flex: 1;
  padding: 10px;
  border: none;
  background: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

.toggle-btn.active {
  font-weight: bold;
  color: white;
  background: #8d8a8a;
  border-radius: 25px;
  padding: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50%;
  max-width: 250px;
  height: 17px;
  margin: 20px 0;
}

.divider-line {
  width: 1px;
  height: 30px;
  background-color: #ddd;
}

.list-view {
  width: 100%;
  max-width: 800px;
  padding: 10px;
  background-color: #f9f9f9;
}

.list-view ul {
  list-style: none;
  padding: 0;
}

.list-view li {
  padding: 10px;
  border-bottom: 1px solid #ddd;
}
</style>
