<template>
  <div>
    <div class="datapicker-wrapper">
      <date-picker
        v-model="date"
        :lang="lang"
     
        :format="formatDate"
        class="default__datapicker"
        range-separator=" - "
        :range="range"
        :placeholder="placeholder"
        @change="emitDate()"
        @focus="dateFocus = !dateFocus"
      ></date-picker>
      <!--    @change="$emit('date-text',date)" -->
      <svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        class="datapicker-icon"
        :class="{ active: dateFocus }"
      >
        <path
          d="M15 3V5"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M7 3V5"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M3 8H19"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M19 12V6C19 5.46957 18.7893 4.96086 18.4142 4.58579C18.0391 4.21071 17.5304 4 17 4H5C4.46957 4 3.96086 4.21071 3.58579 4.58579C3.21071 4.96086 3 5.46957 3 6V17C3 17.5304 3.21071 18.0391 3.58579 18.4142C3.96086 18.7893 4.46957 19 5 19H12"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M7 10.75C6.95055 10.75 6.90222 10.7647 6.86111 10.7921C6.82 10.8196 6.78795 10.8586 6.76903 10.9043C6.75011 10.95 6.74516 11.0003 6.7548 11.0488C6.76445 11.0973 6.78826 11.1418 6.82322 11.1768C6.85819 11.2117 6.90273 11.2356 6.95123 11.2452C6.99972 11.2548 7.04999 11.2499 7.09567 11.231C7.14135 11.212 7.1804 11.18 7.20787 11.1389C7.23534 11.0978 7.25 11.0494 7.25 11C7.25013 10.9671 7.24376 10.9346 7.23124 10.9042C7.21872 10.8738 7.20031 10.8462 7.17707 10.8229C7.15383 10.7997 7.12622 10.7813 7.09583 10.7688C7.06544 10.7562 7.03287 10.7499 7 10.75Z"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M7 14.75C6.95055 14.75 6.90222 14.7647 6.86111 14.7921C6.82 14.8196 6.78795 14.8586 6.76903 14.9043C6.75011 14.95 6.74516 15.0003 6.7548 15.0488C6.76445 15.0973 6.78826 15.1418 6.82322 15.1768C6.85819 15.2117 6.90273 15.2356 6.95123 15.2452C6.99972 15.2548 7.04999 15.2499 7.09567 15.231C7.14135 15.212 7.1804 15.18 7.20787 15.1389C7.23534 15.0978 7.25 15.0494 7.25 15C7.25013 14.9671 7.24376 14.9346 7.23124 14.9042C7.21872 14.8738 7.20031 14.8462 7.17707 14.8229C7.15383 14.7997 7.12622 14.7813 7.09583 14.7688C7.06544 14.7562 7.03287 14.7499 7 14.75Z"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M10.75 11C10.75 11.0494 10.7647 11.0978 10.7921 11.1389C10.8196 11.18 10.8587 11.212 10.9043 11.231C10.95 11.2499 11.0003 11.2548 11.0488 11.2452C11.0973 11.2356 11.1418 11.2117 11.1768 11.1768C11.2117 11.1418 11.2356 11.0973 11.2452 11.0488C11.2549 11.0003 11.2499 10.95 11.231 10.9043C11.2121 10.8586 11.18 10.8196 11.1389 10.7921C11.0978 10.7647 11.0495 10.75 11 10.75C10.9671 10.7499 10.9344 10.7563 10.9039 10.7689C10.8735 10.7814 10.8458 10.7999 10.8226 10.8233C10.7993 10.8466 10.7809 10.8744 10.7685 10.9049C10.756 10.9354 10.7497 10.9681 10.75 11.001"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M17 19H21"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
        <path
          d="M19 21V17"
          stroke="#E62128"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          :class="{ active: dateFocus }"
        />
      </svg>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    placeholder: {
      type: String,
      default: "",
    },
    label: {
      type: String,
      default: "",
    },
    range: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return { date: "", lang: "Ru", formatDate: "DD MMMM", dateFocus: false };
  },
  methods: {
    emitDate() {
      this.$emit("date-text", this.date);
      this.dateFocus = false;
    },
  },
  mounted() {
    window.addEventListener("click", (e) => {
      let target = e.target;
      if (
        !target.closest(".datapicker-wrapper") &&
        !target.closest(".mx-datepicker-popup")
      ) {
        this.dateFocus = false;
      }
    });
  },
};
</script>