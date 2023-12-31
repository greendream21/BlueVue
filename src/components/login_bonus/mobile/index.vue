<script lang="ts" setup>
import { ref, computed } from "vue";
import { useI18n } from "vue-i18n";
import { vipStore } from "@/store/vip";
import { storeToRefs } from "pinia";
import { Swiper, SwiperSlide } from "swiper/vue";
// Import Swiper styles
import "swiper/css";
import "swiper/css/pagination";
// import Swiper core and required modules
import { Pagination } from "swiper/modules";

const emit = defineEmits<{ (e: "closeLoginBonusDialog"): void }>();
const { t } = useI18n();

const vipGrade = ref("VIP1");
const loginBonusItem = ref({
  award: [8, 10, 12, 14, 16, 18, 20, 22], // reward list
  signin_day: 0, // The number of days that have been signed in
  is_signin: true, // You can sign in today
});
const swiper = ref<any>(null);

const modules = [Pagination];

const vipLevels = computed(() => {
  const { getVipLevels } = storeToRefs(vipStore());
  return getVipLevels.value;
});

const goToPrev = () => {
  swiper.value.slidePrev();
};

const goToNext = () => {
  swiper.value.slideNext();
};

const getSwiperRef = (swiperInstance: any) => {
  swiper.value = swiperInstance;
};

const handleLoginBonus = (day: number) => {
  if (day == loginBonusItem.value.signin_day + 1) {
    loginBonusItem.value.signin_day = day;
  }
};
</script>

<template>
  <div class="m-login-bonus-dialog-container">
    <img
      src="@/assets/public/image/bg_public_03_01.png"
      class="m-header-bar-img-position"
    />
    <v-btn
      class="m-close-login-bonus-button"
      icon="true"
      @click="emit('closeLoginBonusDialog')"
      height="32"
      width="32"
    >
      <img src="@/assets/public/svg/icon_public_52.svg" width="20" />
    </v-btn>
    <img src="@/assets/vip/image/img_agent_02.png" class="m-login-bonus-agent-img" />
    <div class="m-login-bonus-body">
      <Swiper :modules="modules" :slidesPerView="1" :loop="true" @swiper="getSwiperRef">
        <SwiperSlide
          v-for="(item, index) in vipLevels"
          :key="index"
          :virtualIndex="index"
        >
          <div class="mt-2 text-center">
            <Font class="color-F9BC01 text-900-18">VIP{{ item.level }}</Font>
            <Font class="text-900-18 white">
              {{ t("vip.login_bonus.title_text") }}
            </Font>
          </div>
          <v-row class="mt-2 mx-2">
            <v-col cols="4" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg relative"
                v-if="loginBonusItem.signin_day >= 1"
              >
                <p class="text-900-14 gray login-bonus-text-position">
                  {{ t("vip.login_bonus.day_1_text") }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="m-login-bonus-card-cash-position"
                  width="32"
                />
                <p class="text-900-14 gray m-login-bonus-card-money-position">
                  $ {{ item.signin_award[0].award }}
                </p>
              </div>
              <div
                class="m-login-bonus-card-bg-1 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(1)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 1"
                />
                <p class="text-900-12 white m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_1_text") }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_21.png"
                  class="m-login-bonus-card-cash-position"
                  width="29"
                />
                <p class="text-900-12 white m-login-bonus-card-money-position">
                  $ {{ item.signin_award[0].award }}
                </p>
              </div>
            </v-col>
            <v-col cols="4" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg relative"
                v-if="loginBonusItem.signin_day >= 2"
              >
                <p class="text-900-14 gray m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_2_text") }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="m-login-bonus-card-cash-position"
                  width="32"
                />
                <p class="text-900-14 gray m-login-bonus-card-money-position">
                  $ {{ item.signin_award[1].award }}
                </p>
              </div>
              <div
                class="m-login-bonus-card-bg-2 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(2)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 2"
                />
                <p class="text-900-12 white m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_2_text") }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_22.png"
                  class="m-login-bonus-card-cash-position"
                  width="40"
                />
                <p class="text-900-12 white m-login-bonus-card-money-position">
                  $ {{ item.signin_award[1].award }}
                </p>
              </div>
            </v-col>
            <v-col cols="4" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg relative"
                v-if="loginBonusItem.signin_day >= 3"
              >
                <p class="text-900-14 gray m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_3_text") }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="m-login-bonus-card-cash-position"
                  width="32"
                />
                <p class="text-900-14 gray m-login-bonus-card-money-position">
                  $ {{ item.signin_award[2].award }}
                </p>
              </div>
              <div
                class="m-login-bonus-card-bg-3 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(3)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 3"
                />
                <p class="text-900-12 white m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_3_text") }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_23.png"
                  class="m-login-bonus-card-cash-position"
                  width="50"
                />
                <p class="text-900-12 white m-login-bonus-card-money-position">
                  $ {{ item.signin_award[2].award }}
                </p>
              </div>
            </v-col>
          </v-row>
          <v-row class="mt-5 mx-2">
            <v-col cols="4" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg relative"
                v-if="loginBonusItem.signin_day >= 4"
              >
                <p class="text-900-14 gray m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_4_text") }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="m-login-bonus-card-cash-position"
                  width="32"
                />
                <p class="text-900-14 gray login-bonus-card-money-position">
                  $ {{ item.signin_award[3].award }}
                </p>
              </div>
              <div
                class="m-login-bonus-card-bg-4 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(4)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 4"
                />
                <p class="text-900-12 white m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_4_text") }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_24.png"
                  class="m-login-bonus-card-cash-position"
                  width="48"
                />
                <p class="text-900-12 white m-login-bonus-card-money-position">
                  $ {{ item.signin_award[3].award }}
                </p>
              </div>
            </v-col>
            <v-col cols="4" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg relative"
                v-if="loginBonusItem.signin_day >= 5"
              >
                <p class="text-900-14 gray m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_5_text") }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="login-bonus-card-cash-position"
                  width="32"
                />
                <p class="text-900-14 gray m-login-bonus-card-money-position">
                  $ {{ item.signin_award[4].award }}
                </p>
              </div>
              <div
                class="m-login-bonus-card-bg-4 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(5)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 5"
                />
                <p class="text-900-12 white m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_5_text") }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_25.png"
                  class="m-login-bonus-card-cash-position"
                  width="45"
                />
                <p class="text-900-12 white m-login-bonus-card-money-position">
                  $ {{ item.signin_award[4].award }}
                </p>
              </div>
            </v-col>
            <v-col cols="4" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg relative"
                v-if="loginBonusItem.signin_day >= 6"
              >
                <p class="text-900-14 gray m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_6_text") }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="m-login-bonus-card-cash-position"
                  width="32"
                />
                <p class="text-900-14 gray m-login-bonus-card-money-position">
                  $ {{ item.signin_award[5].award }}
                </p>
              </div>
              <div
                class="m-login-bonus-card-bg-4 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(6)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 6"
                />
                <p class="text-900-12 white m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_6_text") }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_26.png"
                  class="m-login-bonus-card-cash-position"
                  width="50"
                />
                <p class="text-900-12 white m-login-bonus-card-money-position">
                  $ {{ item.signin_award[5].award }}
                </p>
              </div>
            </v-col>
          </v-row>
          <v-row class="mt-5 mx-2 mb-1">
            <v-col cols="4" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg relative"
                v-if="loginBonusItem.signin_day >= 7"
              >
                <p class="text-900-14 gray m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_7_text") }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="m-login-bonus-card-cash-position"
                  width="32"
                />
                <p class="text-900-14 gray m-login-bonus-card-money-position">
                  $ {{ item.signin_award[6].award }}
                </p>
              </div>
              <div
                class="m-login-bonus-card-bg-4 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(7)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 7"
                />
                <p class="text-900-12 white m-login-bonus-text-position">
                  {{ t("vip.login_bonus.day_7_text") }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_26.png"
                  class="m-login-bonus-card-cash-position"
                  width="50"
                />
                <p class="text-900-12 white m-login-bonus-card-money-position">
                  $ {{ item.signin_award[6].award }}
                </p>
              </div>
            </v-col>
            <v-col cols="8" class="pa-0 d-flex justify-center">
              <div
                class="m-login-bonus-card-checkout-bg-1 relative"
                v-if="loginBonusItem.signin_day >= 8"
              >
                <p class="text-900-14 gray m-login-bonus-text-position-1">
                  {{ t("vip.login_bonus.day_8_text") }}
                </p>
                <p class="text-900-14 gray mt-2 m-login-bonus-card-money-position-1">
                  $ {{ loginBonusItem.award[7] }}
                </p>
                <img
                  src="@/assets/public/svg/icon_public_18.svg"
                  class="m-login-bonus-card-cash-position"
                  width="32"
                />
              </div>
              <div
                class="m-login-bonus-card-bg-5 relative"
                v-ripple.center
                v-else
                @click="handleLoginBonus(8)"
              >
                <img
                  src="@/assets/vip/image/img_vip_32.png"
                  class="m-login-bonus-sunshine-img"
                  v-if="loginBonusItem.signin_day + 1 == 8"
                />
                <p class="text-900-12 white m-login-bonus-text-position-1">
                  {{ t("vip.login_bonus.day_8_text") }}
                </p>
                <p class="text-900-12 white mt-2 m-login-bonus-card-money-position-1">
                  $ {{ loginBonusItem.award[7] }}
                </p>
                <img
                  src="@/assets/vip/image/img_vip_27.png"
                  width="113"
                  height="61"
                  class="m-login-bonus-card-cash-position-1"
                />
              </div>
            </v-col>
          </v-row>
        </SwiperSlide>
      </Swiper>
    </div>
    <v-row class="m-login-bonus-footer mx-0 align-center">
      <v-col cols="2" class="pa-0 ma-0">
        <v-btn
          class="m-left-login-bonus-button"
          icon="true"
          height="32"
          width="32"
          @click="goToPrev"
        >
          <v-icon color="#FFFFFF">mdi-chevron-left</v-icon>
        </v-btn>
      </v-col>
      <v-col cols="8" class="pa-0 ma-0">
        <p class="text-center" style="line-height: normal">
          <Font class="text-900-10 white">{{ t("vip.login_bonus.footer_text_1") }}</Font>
          <Font class="text-900-10 purple">R$20 </Font>
          <Font class="text-900-10 white">{{ t("vip.login_bonus.footer_text_2") }}</Font>
          <Font class="text-900-10 purple">{{ t("vip.login_bonus.footer_text_3") }}</Font>
        </p>
        <p class="text-center" style="line-height: normal">
          <Font class="text-900-10 white">{{ t("vip.login_bonus.footer_text_4") }}</Font>
        </p>
      </v-col>
      <v-col cols="2" class="pa-0 ma-0 text-right">
        <v-btn
          class="m-left-login-bonus-button"
          icon="true"
          height="32"
          width="32"
          @click="goToNext"
        >
          <v-icon color="#FFFFFF">mdi-chevron-right</v-icon>
        </v-btn>
      </v-col>
    </v-row>
  </div>
</template>

<style lang="scss">
.m-login-bonus-footer {
  position: absolute;
  bottom: 34px;
  width: 300px;
  left: 50%;
  transform: translateX(-50%);
}

.m-login-bonus-dialog-container {
  width: 340px;
  height: 428px;
  border-radius: 16px;
  background: #29263c;
  position: relative;
}

.m-header-bar-img-position {
  position: absolute;
  top: -28px;
  right: 26px;
}

.m-close-login-bonus-button {
  position: absolute;
  top: -56px;
  right: 12px;
  background: #29263c;
  z-index: 1000;
}

.m-login-bonus-agent-img {
  position: absolute;
  top: -95px;
  left: 15px;
  width: 281px;
  height: 186px;
}

.m-login-bonus-body {
  position: absolute;
  top: 12px;
  left: 50%;
  transform: translateX(-50%);
  width: 320px;
  height: 338px;
  border-radius: 14px;
  background: #211f31;
  box-shadow: 2px 0px 4px 1px rgba(0, 0, 0, 0.12) inset;
  z-index: 20;
}

.m-login-bonus-card-checkout-bg {
  width: 94px;
  height: 90px;
  border-radius: 6px;
  background: #29263c;

  /* Button Shadow */
  box-shadow: 0px 3px 4px 1px rgba(0, 0, 0, 0.21);
}

.m-login-bonus-card-checkout-bg-1 {
  width: 195px;
  height: 90px;
  border-radius: 6px;
  background: #29263c;

  /* Button Shadow */
  box-shadow: 0px 3px 4px 1px rgba(0, 0, 0, 0.21);
}

.m-login-bonus-card-bg-1 {
  width: 94px;
  height: 90px;
  border-radius: 6px;
  background: linear-gradient(180deg, #c6428b 0%, #f08734 100%);
  /* Button Shadow */
  box-shadow: 0px 3px 4px 1px rgba(0, 0, 0, 0.21);
  cursor: pointer;
  overflow: hidden;
}

.m-login-bonus-card-bg-2 {
  width: 94px;
  height: 90px;
  border-radius: 6px;
  background: linear-gradient(180deg, #2087e8 0%, #0e4987 100%);

  /* Button Shadow */
  box-shadow: 0px 3px 4px 1px rgba(0, 0, 0, 0.21);
  cursor: pointer;
  overflow: hidden;
}

.m-login-bonus-card-bg-3 {
  width: 94px;
  height: 90px;
  border-radius: 6px;
  background: linear-gradient(180deg, #2087e8 0%, #0e4987 100%);

  /* Button Shadow */
  box-shadow: 0px 3px 4px 1px rgba(0, 0, 0, 0.21);
  cursor: pointer;
  overflow: hidden;
}

.m-login-bonus-card-bg-4 {
  width: 94px;
  height: 90px;
  border-radius: 6px;
  background: linear-gradient(180deg, #2087e8 0%, #0e4987 100%);

  /* Button Shadow */
  box-shadow: 0px 3px 4px 1px rgba(0, 0, 0, 0.21);
  cursor: pointer;
  overflow: hidden;
}

.m-login-bonus-sunshine-img {
  width: 350px;
  height: 350px;
  border-radius: 6px;
  animation: rotate 5s linear infinite;
  position: absolute;
  top: 50%;
  left: 50%;
  transform-origin: center center;
  transform: translate(-50%, -50%) rotate(45deg);
  object-fit: cover;
}

@keyframes rotate {
  0% {
    transform: translate(-50%, -50%) rotate(0deg);
  }

  100% {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}

.m-login-bonus-card-bg-5 {
  width: 195px;
  height: 90px;
  border-radius: 6px;
  background: linear-gradient(180deg, #9419f0 0%, #275798 100%);
  cursor: pointer;
  overflow: hidden;
}

.m-login-bonus-text-position {
  position: absolute;
  top: 12px;
  left: 50%;
  transform: translateX(-50%);
}

.m-login-bonus-text-position-1 {
  position: absolute;
  top: 26px;
  left: 20px;
}

.m-login-bonus-card-cash-position {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.m-login-bonus-card-cash-position-1 {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
}

.m-login-bonus-card-money-position {
  position: absolute;
  bottom: 12px;
  left: 50%;
  transform: translateX(-50%);
}

.m-login-bonus-card-money-position-1 {
  position: absolute;
  top: 40px;
  left: 20px;
}

.m-left-login-bonus-button {
  background: #211f31;
  /* Text Box */
  box-shadow: 2px 0px 4px 1px rgba(0, 0, 0, 0.12) inset;

  .v-btn__content {
    font-size: 16px;
  }
}
</style>
