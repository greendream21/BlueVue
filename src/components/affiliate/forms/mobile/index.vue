<script lang="ts" setup>
import { ref, computed, h, shallowRef, watch, onMounted } from "vue";
import { Options } from "@popperjs/core";
import Pagination from "@/components/global/pagination/index.vue";
import { useI18n } from "vue-i18n";
import { useDisplay } from "vuetify";
import moment from "moment-timezone";
import { refferalStore } from "@/store/refferal";
import { inviteStore } from "@/store/invite";
import { storeToRefs } from "pinia";
import { appBarStore } from "@/store/appBar";

const { t } = useI18n();
const { width } = useDisplay();
const { dispatchInviteHistoryCfg } = inviteStore();

const customPrefix = shallowRef({
  render() {
    return h("p", "");
  },
});

const customClear = shallowRef({
  render() {
    return h("p", "");
  },
});

const popperOptions = ref<Options>({
  modifiers: [
    {
      name: "preventOverflow",
      options: {
        boundary: "viewport",
      },
    },
  ],
  placement: "top",
  strategy: "fixed",
});

// const popperOptions = ref({
//     dayNamesShort: ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
//     dayNames: ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
// })

const dateFormat = ref("YYYY/MM/DD");

const selectedBonusItem = ref("Invitation Bonus");

const datePickerShow = ref<boolean>(false);

const bonusItems = ref<Array<string>>(["Invitation Bonus", "Betting Commission"]);

const bonusMenuShow = ref<boolean>(false);
const cashMenuShow = ref<boolean>(false);

const selectedCashItem = ref<number>(10);

const cashItems = ref<Array<number>>([10, 11, 12]);

const formsList = ref<Array<any>>([
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
  {
    time: "04/08 12:29:42",
    user: "User6910821714",
    bonus: "R$ 12",
  },
]);

const handleDropdown = (item: any) => {
  selectedBonusItem.value = item.name;
};

const handleCashDropdown = (item: number) => {
  selectedCashItem.value = item;
};

const selectedDate = ref([
  moment().tz("Asia/Hong_Kong").format("YYYY/MM/DD"),
  moment().tz("Asia/Hong_Kong").format("YYYY/MM/DD"),
]);

const refferalAppBarShow = computed(() => {
  const { getRefferalAppBarShow } = storeToRefs(refferalStore());
  return getRefferalAppBarShow.value;
});

window.addEventListener("scroll", function () {
  datePickerShow.value = false;
  bonusMenuShow.value = false;
  cashMenuShow.value = false;
});

const mobileWidth = computed(() => {
  return width.value;
});

const inviteHistoryConfig = computed(() => {
  const { getInviteHistoryConfig } = storeToRefs(inviteStore());
  return getInviteHistoryConfig.value;
});

onMounted(async () => {
  await dispatchInviteHistoryCfg();
});

const fixPositionShow = computed(() => {
  const { getFixPositionEnable } = storeToRefs(appBarStore());
  return getFixPositionEnable.value;
});

</script>
<template>
  <v-row class="mt-4 mx-4">
    <div class="relative m-date-picker" @click="datePickerShow = true">
      <el-date-picker
        v-model="selectedDate"
        :popper-class="
          refferalAppBarShow ? 'm-date-picker-background' : 'm-date-picker-background-1'
        "
        type="daterange"
        value-format="YYYY/MM/DD"
        :format="dateFormat"
        :prefix-icon="customPrefix"
        :visible="datePickerShow"
        :clear-icon="customClear"
        start-placeholder="Start date"
        end-placeholder="End date"
        :popper-options="popperOptions"
        @change="datePickerShow = false"
        @blur="datePickerShow = false"
      >
        <template #range-separator>
          <img src="@/assets/public/svg/icon_public_83.svg" width="18" />
        </template>
      </el-date-picker>
      <img
        src="@/assets/public/svg/icon_public_23.svg"
        class="date-icon-position"
        width="20"
      />
    </div>
  </v-row>
  <v-row class="mt-6 mx-4">
    <v-menu offset="10" content-class="m-bonus-menu" v-model="bonusMenuShow">
      <template v-slot:activator="{ props }">
        <v-card color="#1C1929" theme="dark" class="m-bonus-menu-card">
          <v-list-item
            class="bonus-item"
            v-bind="props"
            :title="selectedBonusItem"
            append-icon="mdi-chevron-down"
            value=""
          >
          </v-list-item>
        </v-card>
      </template>
      <v-list theme="dark" bg-color="#1C1929">
        <v-list-item
          v-for="(item, i) in inviteHistoryConfig.list"
          :key="i"
          :value="item.name"
          class="bonus-item mx-2"
          @click="handleDropdown(item)"
          :class="selectedBonusItem == item.name ? 'm-bonus-menu-selected-item' : ''"
        >
          <v-list-item-title>{{ item.name }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
    <v-menu offset="10" content-class="m-bonus-menu" v-model="cashMenuShow">
      <template v-slot:activator="{ props }">
        <v-card color="#1C1929" theme="dark" class="mr-2 ml-auto m-bonus-menu-card">
          <v-list-item
            class="bonus-item"
            v-bind="props"
            :title="selectedCashItem"
            append-icon="mdi-chevron-down"
            value=""
          >
          </v-list-item>
        </v-card>
      </template>
      <v-list theme="dark" bg-color="#1C1929">
        <v-list-item
          v-for="(item, i) in cashItems"
          :key="i"
          :value="item"
          class="bonus-item mx-2"
          :class="selectedCashItem == item ? 'm-bonus-menu-selected-item' : ''"
          @click="handleCashDropdown(item)"
        >
          <v-list-item-title>{{ item }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-row>
  <v-row class="mx-2 mt-6 m-forms-bonus-table" >
    <v-table class="m-forms-bonus-table-bg" :class="fixPositionShow ? 'table-position-overflow' : ''" theme="dark" fixed-header>
      <thead class="forms-table-header">
        <tr>
          <th class="m-forms-table-header-text" style="border-radius: 43px 0px 0px 43px">
            {{ t("affiliate.forms.table.time") }}
          </th>
          <th class="m-forms-table-header-text">
            <div class="forms-table-border">{{ t("affiliate.forms.table.user") }}</div>
          </th>
          <th class="m-forms-table-header-text" style="border-radius: 0px 43px 43px 0px">
            {{ t("affiliate.forms.table.bonus") }}
          </th>
        </tr>
      </thead>
      <tbody class="m-forms-table-body">
        <tr v-for="(item, index) in formsList" :key="index">
          <td class="text-500-12">{{ item.time }}</td>
          <td class="text-500-12">{{ item.user }}</td>
          <td class="text-500-12">{{ item.bonus }}</td>
        </tr>
      </tbody>
    </v-table>
  </v-row>
  <v-row class="mt-4 justify-center mx-4 pb-2">
    <Pagination />
  </v-row>
</template>
<style lang="scss">
.m-bonus-menu::after {
  content: "";
  position: absolute;
  align-self: center;
  float: right;
  top: -16px;
  border: 9px solid #1c1929;
  border-right-color: transparent;
  border-left-color: transparent;
  border-top-color: transparent;
  border-right-width: 7px;
  border-left-width: 7px;
}

.m-bonus-menu-selected-item {
  border-radius: 12px;
  background: #29253c;
}

.m-bonus-menu-card {
  border-radius: 12px;
  background: var(--bg-51-c-1929, #1c1929);

  /* Text Box */
  box-shadow: 2px 0px 4px 1px rgba(0, 0, 0, 0.12) inset;
}

.bonus-item {
  height: 100% !important;
  padding: 4px 14px !important;
  text-align: center !important;

  .v-list-item-title {
    font-weight: 500;
    font-size: 14px;
    color: #7782aa;
  }
}

.date-icon-position {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: 13px;
}

.m-forms-bonus-table-bg {
  padding: 8px;
  background: #1c1929 !important;
  box-shadow: inset 2px 0px 4px 1px rgba(0, 0, 0, 0.12) !important;
  border-radius: 12px !important;
  width: 100% !important;
}

.table-position-overflow {
  .v-table__wrapper {
    overflow: hidden!important;
  }
}

.m-forms-bonus-table {
  .v-table.v-table--fixed-header > .v-table__wrapper > table > thead > tr > th {
    background: #414968;
    height: 36px !important;
  }

  .v-table .v-table__wrapper > table > tbody > tr:not(:last-child) > td,
  .v-table .v-table__wrapper > table > tbody > tr:not(:last-child) > th {
    border-bottom: 1px solid #414968;
  }

  .forms-table-header {
    border-radius: 43px !important;
  }

  .m-forms-table-body {
    font-weight: 500;
    font-size: 14px;
    line-height: 19px;
    color: #ffffff;
    text-align: center;
  }

  .m-forms-table-header-text {
    font-weight: 700 !important;
    font-size: 14px !important;
    text-align: center !important;
    color: #000000 !important;
  }

  .forms-table-border {
    border-left: 1px solid #000000 !important;
    border-right: 1px solid #000000 !important;
  }
}

.m-date-picker {
  .el-date-editor.el-input__wrapper {
    width: 264px;
    height: 40px !important;
    background: #1c1929 !important;
    box-shadow: inset 2px 0px 4px 1px rgba(0, 0, 0, 0.12) !important;
    border-radius: 12px !important;
    justify-content: start;
  }

  .el-date-editor .el-range-input {
    font-weight: 500 !important;
    font-size: 12px !important;
    color: #7782aa !important;
  }

  .el-range__icon {
    display: none;
  }

  .el-popper.is-light {
    background-color: #181522 !important;
    border: none !important;
    border-radius: 10px !important;
  }
}

.m-date-picker-background-1 {
  z-index: 2002 !important;
  position: absolute !important;
  inset: 184px auto auto 0px !important;

  .el-popper.is-light .el-popper__arrow::before {
    border: 1px solid #181522;
    background: #181522;
    right: 0;
  }

  .el-popper.is-light {
    background-color: #181522 !important;
    border: none !important;
    border-radius: 10px !important;
  }

  .el-picker-panel {
    background-color: #181522;
    font-family: "Inter";
    font-style: normal;
    font-weight: 600;
    font-size: 12px;
    line-height: 19px;
    color: #ffffff;
    border-color: #181522;
    border: none;
    box-shadow: none;
    border-radius: 10px !important;
    box-shadow: 0px 5px 5px -3px var(--v-shadow-key-umbra-opacity, rgba(0, 0, 0, 0.2)),
      0px 8px 10px 1px var(--v-shadow-key-penumbra-opacity, rgba(0, 0, 0, 0.14)),
      0px 3px 14px 2px var(--v-shadow-key-penumbra-opacity, rgba(0, 0, 0, 0.12));
  }

  .el-date-range-picker__content.is-left {
    border-right: none;
  }

  .el-date-table td.in-range .el-date-table-cell {
    background-color: transparent;
  }

  .el-picker-panel__icon-btn .el-icon {
    cursor: pointer;
    font-weight: 900;
    font-size: 18px;
    color: #7782aa;
  }

  .el-date-table th {
    padding: 5px;
    font-weight: 600;
    font-size: 10px;
    color: #ffffff;
    border-bottom: none;
  }

  .el-date-range-picker {
    width: 100% !important;
  }

  .el-date-range-picker .el-picker-panel__body {
    min-width: unset !important;
  }

  .el-date-range-picker__content {
    padding: 16px 8px !important;
  }

  .el-date-table td {
    padding: 0 !important;
    font-weight: 600;
    font-size: 10px;
    color: #ffffff;
  }

  .el-picker-panel__icon-btn {
    margin-top: 0px !important;
  }

  .el-date-range-picker__content .el-date-range-picker__header div {
    font-weight: 600;
    font-size: 12px;
    margin-left: 20px;
    margin-right: 20px;
  }

  .el-date-table th {
    padding: 0px !important;
  }

  .el-popper__arrow {
    left: 50% !important;
  }

  .el-date-table td.end-date .el-date-table-cell__text,
  .el-date-table td.start-date .el-date-table-cell__text {
    background-color: unset;
    display: flex;
    width: 24px;
    padding: 2px 5px 1px 5px;
    justify-content: center;
    align-items: center;
    flex-shrink: 0;
    align-self: stretch;
    border-radius: 4px;
    border: 1px solid var(--primary-button-32-cfec, #32cfec);
  }
}

.m-date-picker-background {
  z-index: 2002 !important;
  position: absolute !important;
  inset: 216px auto auto 0px !important;

  .el-popper.is-light .el-popper__arrow::before {
    border: 1px solid #181522;
    background: #181522;
    right: 0;
  }

  .el-popper.is-light {
    background-color: #181522 !important;
    border: none !important;
    border-radius: 10px !important;
  }

  .el-picker-panel {
    background-color: #181522;
    font-family: "Inter";
    font-style: normal;
    font-weight: 600;
    font-size: 12px;
    line-height: 19px;
    color: #ffffff;
    border-color: #181522;
    border: none;
    box-shadow: none;
    border-radius: 10px !important;
    box-shadow: 0px 5px 5px -3px var(--v-shadow-key-umbra-opacity, rgba(0, 0, 0, 0.2)),
      0px 8px 10px 1px var(--v-shadow-key-penumbra-opacity, rgba(0, 0, 0, 0.14)),
      0px 3px 14px 2px var(--v-shadow-key-penumbra-opacity, rgba(0, 0, 0, 0.12));
  }

  .el-date-range-picker__content.is-left {
    border-right: none;
  }

  .el-date-table td.in-range .el-date-table-cell {
    background-color: transparent;
  }

  .el-picker-panel__icon-btn .el-icon {
    cursor: pointer;
    font-weight: 900;
    font-size: 18px;
    color: #7782aa;
  }

  .el-date-table th {
    padding: 5px;
    font-weight: 600;
    font-size: 10px;
    color: #ffffff;
    border-bottom: none;
  }

  .el-date-range-picker {
    width: 100% !important;
  }

  .el-date-range-picker .el-picker-panel__body {
    min-width: unset !important;
  }

  .el-date-range-picker__content {
    padding: 16px 8px !important;
  }

  .el-date-table td {
    padding: 0 !important;
    font-weight: 600;
    font-size: 10px;
    color: #ffffff;
  }

  .el-picker-panel__icon-btn {
    margin-top: 0px !important;
  }

  .el-date-range-picker__content .el-date-range-picker__header div {
    font-weight: 600;
    font-size: 12px;
    margin-left: 20px;
    margin-right: 20px;
  }

  .el-date-table th {
    padding: 0px !important;
  }

  .el-popper__arrow {
    left: 50% !important;
  }

  .el-date-table td.end-date .el-date-table-cell__text,
  .el-date-table td.start-date .el-date-table-cell__text {
    background-color: unset;
    display: flex;
    width: 24px;
    padding: 2px 5px 1px 5px;
    justify-content: center;
    align-items: center;
    flex-shrink: 0;
    align-self: stretch;
    border-radius: 4px;
    border: 1px solid var(--primary-button-32-cfec, #32cfec);
  }
}
</style>
