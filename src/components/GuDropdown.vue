<script setup>
import { gu } from "@/constant/seoul.js";
import { defineEmits, onMounted, onUnmounted, ref } from "vue";

const selectedGu = ref(gu[0]);
const openDropdown = ref(false);

const clickGu = (idx) => {
  selectedGu.value = gu[idx];
  openDropdown.value = false;
  emit("change", idx);
};

const emit = defineEmits(["change"]);

const isLast = (idx) => idx === gu.length - 1;

// Add click outside handler
const handleClickOutside = (event) => {
  const dropdown = document.querySelector(".dropdown");
  const dropdownList = document.querySelector(".dropdown-list");

  if (
    !dropdown?.contains(event.target) &&
    !dropdownList?.contains(event.target)
  ) {
    openDropdown.value = false;
  }
};

// Add and remove event listener
onMounted(() => {
  document.addEventListener("click", handleClickOutside);
});

onUnmounted(() => {
  document.removeEventListener("click", handleClickOutside);
});
</script>

<template>
  <div style="position: relative; margin-left: 8px">
    <div class="dropdown" @click="() => (openDropdown = !openDropdown)">
      <div class="dropdown_text">{{ selectedGu }}</div>
      <img
        style="width: 22px; height: 22px"
        src="../assets/arrowDropDown.svg"
      />
    </div>
    <div class="dropdown-list" v-if="openDropdown">
      <div
        :class="['dropdown-item', isLast(idx) ? '' : 'not-last']"
        v-for="(g, idx) in gu"
        v-bind:key="idx"
        @click="clickGu(idx)"
      >
        {{ g }}
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.dropdown {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 88px;
  height: 24px;
  border-radius: 12px;
  border: solid 2px #ffefeb;
  padding-left: 8px;
  box-sizing: border-box;
}

.dropdown:hover {
  cursor: pointer;
}

.dropdown_text {
  color: #ffefeb;
  font-size: 14px;
  font-weight: bold;
  text-align: center;
}

.dropdown-list {
  position: absolute;
  top: 32px;
  border: solid 1px #ffefeb;
  border-radius: 9px;
  background-color: #6172e2;
  width: 88px;
  height: 140px;
  overflow-y: auto;
  overflow-x: hidden;
  box-sizing: border-box;

  // Add custom scrollbar styles
  &::-webkit-scrollbar {
    // display: none;
    width: 4px;
  }

  &::-webkit-scrollbar-track {
    background-color: #6172e2;
    border-radius: 9px;
    margin: 4px 0;
  }
  &::-webkit-scrollbar-thumb {
    background-color: #ffefeb;
    border-radius: 9px;
    margin-right: 2px;
  }

  &::-webkit-scrollbar-thumb:hover {
    background: #ffd9cf; // scrollbar thumb hover color
  }
}

.dropdown-item {
  width: 86px;
  height: 28px;
  font-size: 14px;
  line-height: 28px;
  box-sizing: border-box;
  color: #ffefeb;
  font-weight: bold;
  text-align: center;
}

.not-last {
  border-bottom: 1px solid #ffefeb;
}
</style>
