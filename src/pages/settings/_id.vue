<template>
  <div class="settings">
    <div class="container">
      <div class="settings__wrapper">
        <AppNavbar />
        <main class="settings-page">
          <h1 class="settings__title">Новое событие</h1>
          <form conte class="settings__form">
            <div class="settings__photo">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Основное фото <span>*</span>
                </h3>
                <hr />
              </div>
              <div class="photo-add__wrapper">
                <!--    <img v-if="preview" :src="preview" class="img-fluid" /> -->
                <app-add-photo
                  @photo="form.main_photo.file = $event"
                  :edit-preview="form.main_photo.avatar"
                />
                <p class="settings__photo-text">
                  Основное фото Вашего события. По статистике события с фото
                  получают больше просмотров
                </p>
              </div>
            </div>
            <div>
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Заголовок <span>*</span>
                </h3>
                <hr />
              </div>
              <app-input placeholder="Название события" v-model="form.title" />
            </div>
            <div class="settings__category settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Категория события <span>*</span>
                </h3>
                <hr />
              </div>
              <app-select
                label-name="name"
                input-name="category"
                placeholder="Выбрать"
                v-model="form.category"
                :options="options.categories"
              />
            </div>
            <div class="settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Дата и время события <span>*</span>
                </h3>
                <hr />
              </div>
              <app-swich
                label="Несколько дней"
                @input="dateRange"
                v-model="form.more_days"
              />
              <div class="data-time__wrapper">
                <app-date-picker
                  placeholder="Дата"
                  :range="form.more_days"
                  @date-text="dateText"
                />
                <app-select-time placeholder="Время" @time-text="timeText" />
                <p class="form-text">
                  Событие пройдет
                  <span v-if="dates.length === 2"
                    >c {{ currentDateWith }} по {{ currentDateTo }}</span
                  >
                  <span v-else-if="dates.length !== 0">{{ currentDate }}</span>
                  <span v-if="form.dates.time.full">c</span>
                  {{ form.dates.time.from }}
                  <span v-if="form.dates.time.full"> - </span>
                  {{ form.dates.time.to }}
                </p>
                <div v-show="form.more_days">
                  <app-swich
                    label="Добавить свое расписание"
                    hint="(Добавить расписание времени для каждого дня события)"
                    v-model="form.self_schedule_dates"
                  />
                  <ul
                    class="timepicker__schedule-all"
                    v-show="form.self_schedule_dates"
                  >
                    <li
                      class="timepicker__schedule-item"
                      v-for="(dateItem, index) in form.dates.items"
                      :key="index"
                    >
                      <span>{{ dateItem.day }}</span>
                      <!--  <p>{{ currentDateTime() }}</p> -->
                      <app-select-time
                        placeholder="Время"
                        @time-text="dateItemTime($event, index)"
                      />
                    </li>
                  </ul>
                  <app-swich
                    label="Непрерывное событие"
                    hint="(Событие, которое будет проходить без перерыва)"
                    v-model="form.dates_continuous"
                  />
                </div>
              </div>
            </div>
            <div class="settings__location settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Место события<span>*</span>
                </h3>
                <hr />
              </div>
              <app-input
                placeholder="Введите место проведения события"
                v-model="form.country"
              />
              <app-map />
            </div>
            <div class="settings__subavtor settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Соавтор события<span>*</span>
                </h3>
                <hr />
              </div>
              <div class="settings__subavtor-wrapper">
                <div class="input__wrapper">
                  <app-select
                    placeholder="Соавтор события"
                    v-model="form.coauthor"
                    label-name="name"
                    inputName="user"
                    :options="options.users"
                  />
                  <svg
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      d="M18.364 5.63596C19.6227 6.89463 20.4798 8.49828 20.8271 10.2441C21.1743 11.9899 20.9961 13.7995 20.3149 15.4441C19.6337 17.0886 18.4802 18.4942 17.0001 19.4831C15.5201 20.472 13.78 20.9999 12 20.9999C10.22 20.9999 8.47992 20.472 6.99988 19.4831C5.51984 18.4942 4.36629 17.0886 3.6851 15.4441C3.00391 13.7995 2.82567 11.9899 3.17293 10.2441C3.52019 8.49828 4.37734 6.89463 5.636 5.63596C6.47173 4.80022 7.46389 4.13727 8.55583 3.68497C9.64776 3.23267 10.8181 2.99988 12 2.99988C13.1819 2.99988 14.3522 3.23267 15.4442 3.68497C16.5361 4.13727 17.5283 4.80022 18.364 5.63596"
                      stroke="#E62128"
                      stroke-width="1.5"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    />
                    <path
                      d="M17.3071 19.257C16.9231 17.417 14.7001 16 12.0001 16C9.30012 16 7.07712 17.417 6.69312 19.257"
                      stroke="#E62128"
                      stroke-width="1.5"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    />
                    <path
                      d="M14.121 7.87898C14.5405 8.29856 14.8262 8.83311 14.9419 9.41504C15.0576 9.99697 14.9982 10.6001 14.7711 11.1483C14.544 11.6964 14.1595 12.1649 13.6662 12.4946C13.1728 12.8242 12.5928 13.0001 11.9995 13.0001C11.4062 13.0001 10.8262 12.8242 10.3329 12.4946C9.83952 12.1649 9.455 11.6964 9.22792 11.1483C9.00085 10.6001 8.94141 9.99697 9.05712 9.41504C9.17283 8.83311 9.45851 8.29856 9.87801 7.87898C10.1566 7.60035 10.4873 7.37933 10.8513 7.22853C11.2154 7.07774 11.6055 7.00012 11.9995 7.00012C12.3935 7.00012 12.7837 7.07774 13.1477 7.22853C13.5117 7.37933 13.8424 7.60035 14.121 7.87898"
                      stroke="#E62128"
                      stroke-width="1.5"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    />
                  </svg>
                </div>
                <p class="settings__text">
                  Укажите пользователя, который помогает вам  в организации
                  события
                </p>
              </div>
            </div>
            <div class="settings__description settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Описание события<span>*</span>
                </h3>
                <hr />
              </div>
              <div class="settings__description-wrapper">
                <!--  <div class="settings__description-top">
                  <button class="settings__description-button">
                    <svg
                      width="24"
                      height="24"
                      viewBox="0 0 24 24"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M7.34009 19V4.8H12.6801C13.7982 4.80369 14.8694 5.24948 15.66 6.04009C16.4506 6.83069 16.8964 7.90192 16.9001 9.02C16.9066 9.47312 16.8213 9.92288 16.6495 10.3422C16.4776 10.7615 16.2227 11.1417 15.9001 11.46C16.5112 11.7702 17.0228 12.246 17.3764 12.8331C17.73 13.4201 17.9115 14.0948 17.9001 14.78C17.8976 15.3402 17.7842 15.8944 17.5662 16.4106C17.3482 16.9267 17.0301 17.3945 16.6303 17.7869C16.2304 18.1793 15.7567 18.4886 15.2366 18.6968C14.7165 18.905 14.1603 19.0081 13.6001 19H7.34009ZM10.5401 16.06H13.3001C13.4902 16.0693 13.6802 16.0401 13.8587 15.974C14.0372 15.908 14.2005 15.8065 14.3388 15.6757C14.477 15.5449 14.5874 15.3875 14.6632 15.213C14.7391 15.0384 14.7788 14.8503 14.7801 14.66C14.7793 14.4686 14.74 14.2794 14.6646 14.1036C14.5892 13.9277 14.4791 13.7688 14.3411 13.6363C14.203 13.5038 14.0397 13.4005 13.8608 13.3324C13.682 13.2643 13.4913 13.2329 13.3001 13.24H10.5401V16.06ZM10.5401 10.54H12.3201C12.5102 10.5493 12.7002 10.5201 12.8787 10.454C13.0572 10.388 13.2205 10.2865 13.3588 10.1557C13.497 10.0249 13.6074 9.86753 13.6832 9.69298C13.7591 9.51842 13.7988 9.33032 13.8001 9.14C13.7993 8.94865 13.76 8.75941 13.6846 8.58355C13.6092 8.40769 13.4991 8.24879 13.3611 8.11632C13.223 7.98384 13.0597 7.88048 12.8808 7.8124C12.702 7.74431 12.5113 7.71289 12.3201 7.72H10.5401V10.54Z"
                        fill="#373737"
                      />
                    </svg>
                  </button>
                  <button class="settings__description-button">
                    <svg
                      width="24"
                      height="24"
                      viewBox="0 0 24 24"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M10 19L13 5H15L12 19H10Z"
                        fill="#373737"
                      />
                    </svg>
                  </button>
                  <button class="settings__description-button">
                    <svg
                      width="24"
                      height="24"
                      viewBox="0 0 24 24"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M12.1201 19.16C11.3597 19.2086 10.5977 19.0954 9.88435 18.8277C9.17102 18.56 8.52261 18.1439 7.98197 17.6071C7.44133 17.0702 7.02077 16.4248 6.74808 15.7133C6.47539 15.0019 6.35677 14.2407 6.40006 13.48V4.8H8.72006V13.54C8.68031 13.9974 8.74081 14.458 8.89733 14.8896C9.05385 15.3212 9.30264 15.7135 9.62633 16.0391C9.95003 16.3647 10.3408 16.6158 10.7715 16.7748C11.2022 16.9339 11.6624 16.9971 12.1201 16.96C12.5767 16.9966 13.0358 16.933 13.4652 16.7735C13.8947 16.614 14.284 16.3625 14.6061 16.0367C14.9281 15.7109 15.1749 15.3186 15.3294 14.8873C15.4838 14.456 15.5421 13.9962 15.5001 13.54V4.8H17.8201V13.48C17.8657 14.2395 17.7493 15.0001 17.4787 15.7112C17.2081 16.4223 16.7895 17.0679 16.2505 17.6049C15.7115 18.142 15.0645 18.5584 14.3524 18.8265C13.6404 19.0946 12.8794 19.2083 12.1201 19.16Z"
                        fill="#373737"
                      />
                      <path
                        opacity="0.3"
                        d="M19 21H5C4.44772 21 4 21.4477 4 22C4 22.5523 4.44772 23 5 23H19C19.5523 23 20 22.5523 20 22C20 21.4477 19.5523 21 19 21Z"
                        fill="#373737"
                      />
                    </svg>
                  </button>
                  <button class="settings__description-button">
                    <svg
                      width="24"
                      height="24"
                      viewBox="0 0 24 24"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M10.5 5H19.5C19.8978 5 20.2794 5.15804 20.5607 5.43934C20.842 5.72064 21 6.10218 21 6.5C21 6.89782 20.842 7.27936 20.5607 7.56066C20.2794 7.84196 19.8978 8 19.5 8H10.5C10.1022 8 9.72064 7.84196 9.43934 7.56066C9.15804 7.27936 9 6.89782 9 6.5C9 6.10218 9.15804 5.72064 9.43934 5.43934C9.72064 5.15804 10.1022 5 10.5 5ZM10.5 10H19.5C19.8978 10 20.2794 10.158 20.5607 10.4393C20.842 10.7206 21 11.1022 21 11.5C21 11.8978 20.842 12.2794 20.5607 12.5607C20.2794 12.842 19.8978 13 19.5 13H10.5C10.1022 13 9.72064 12.842 9.43934 12.5607C9.15804 12.2794 9 11.8978 9 11.5C9 11.1022 9.15804 10.7206 9.43934 10.4393C9.72064 10.158 10.1022 10 10.5 10ZM10.5 15H19.5C19.8978 15 20.2794 15.158 20.5607 15.4393C20.842 15.7206 21 16.1022 21 16.5C21 16.8978 20.842 17.2794 20.5607 17.5607C20.2794 17.842 19.8978 18 19.5 18H10.5C10.1022 18 9.72064 17.842 9.43934 17.5607C9.15804 17.2794 9 16.8978 9 16.5C9 16.1022 9.15804 15.7206 9.43934 15.4393C9.72064 15.158 10.1022 15 10.5 15Z"
                        fill="#373737"
                      />
                      <path
                        opacity="0.3"
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M5.5 8C5.20333 8 4.91332 7.91203 4.66665 7.74721C4.41997 7.58238 4.22771 7.34811 4.11418 7.07403C4.00065 6.79994 3.97094 6.49834 4.02882 6.20737C4.0867 5.91639 4.22956 5.64912 4.43934 5.43934C4.64912 5.22956 4.91639 5.0867 5.20737 5.02882C5.49834 4.97094 5.79994 5.00065 6.07403 5.11418C6.34811 5.22771 6.58238 5.41997 6.74721 5.66665C6.91203 5.91332 7 6.20333 7 6.5C7 6.89783 6.84197 7.27936 6.56066 7.56066C6.27936 7.84197 5.89783 8 5.5 8ZM5.5 13C5.20333 13 4.91332 12.912 4.66665 12.7472C4.41997 12.5824 4.22771 12.3481 4.11418 12.074C4.00065 11.7999 3.97094 11.4983 4.02882 11.2074C4.0867 10.9164 4.22956 10.6491 4.43934 10.4393C4.64912 10.2296 4.91639 10.0867 5.20737 10.0288C5.49834 9.97094 5.79994 10.0006 6.07403 10.1142C6.34811 10.2277 6.58238 10.42 6.74721 10.6666C6.91203 10.9133 7 11.2033 7 11.5C7 11.8978 6.84197 12.2794 6.56066 12.5607C6.27936 12.842 5.89783 13 5.5 13ZM5.5 18C5.20333 18 4.91332 17.912 4.66665 17.7472C4.41997 17.5824 4.22771 17.3481 4.11418 17.074C4.00065 16.7999 3.97094 16.4983 4.02882 16.2074C4.0867 15.9164 4.22956 15.6491 4.43934 15.4393C4.64912 15.2296 4.91639 15.0867 5.20737 15.0288C5.49834 14.9709 5.79994 15.0007 6.07403 15.1142C6.34811 15.2277 6.58238 15.42 6.74721 15.6666C6.91203 15.9133 7 16.2033 7 16.5C7 16.8978 6.84197 17.2794 6.56066 17.5607C6.27936 17.842 5.89783 18 5.5 18Z"
                        fill="#373737"
                      />
                    </svg>
                  </button>
                  <button class="settings__description-button">
                    <svg
                      width="24"
                      height="24"
                      viewBox="0 0 24 24"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M11 7L9 13H11V18H6V13L8 7H11Z"
                        fill="#373737"
                      />
                      <path
                        opacity="0.3"
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M19 7L17 13H19V18H14V13L16 7H19Z"
                        fill="#373737"
                      />
                    </svg>
                  </button>
                  <button class="settings__description-button">
                    <svg
                      width="24"
                      height="24"
                      viewBox="0 0 24 24"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        opacity="0.3"
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M10.5831 11.9955L8.46179 14.1168C8.27425 14.3043 8.16889 14.5587 8.16889 14.8239C8.16889 15.0891 8.27425 15.3435 8.46179 15.531C8.64932 15.7185 8.90368 15.8239 9.16889 15.8239C9.43411 15.8239 9.68846 15.7185 9.876 15.531L11.9973 13.4097L12.7044 14.1168C13.0795 14.4919 13.2902 15.0006 13.2902 15.531C13.2902 16.0614 13.0795 16.5701 12.7044 16.9452L9.876 19.7736C9.50093 20.1487 8.99222 20.3594 8.46179 20.3594C7.93135 20.3594 7.42265 20.1487 7.04757 19.7736L4.21915 16.9452C3.84407 16.5701 3.63336 16.0614 3.63336 15.531C3.63336 15.0006 3.84407 14.4919 4.21915 14.1168L7.04757 11.2884C7.42265 10.9133 7.93135 10.7026 8.46179 10.7026C8.99222 10.7026 9.50093 10.9133 9.876 11.2884L10.5831 11.9955Z"
                        fill="#373737"
                      />
                      <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M13.4115 11.9955L15.5328 9.87421C15.7204 9.68668 15.8257 9.43232 15.8257 9.16711C15.8257 8.90189 15.7204 8.64754 15.5328 8.46C15.3453 8.27246 15.0909 8.16711 14.8257 8.16711C14.5605 8.16711 14.3062 8.27246 14.1186 8.46L11.9973 10.5813L11.2902 9.87421C10.9151 9.49914 10.7044 8.99043 10.7044 8.46C10.7044 7.92957 10.9151 7.42086 11.2902 7.04579L14.1186 4.21736C14.4937 3.84229 15.0024 3.63157 15.5328 3.63157C16.0633 3.63157 16.572 3.84229 16.947 4.21736L19.7755 7.04579C20.1505 7.42086 20.3613 7.92957 20.3613 8.46C20.3613 8.99043 20.1505 9.49914 19.7755 9.87421L16.947 12.7026C16.572 13.0777 16.0633 13.2884 15.5328 13.2884C15.0024 13.2884 14.4937 13.0777 14.1186 12.7026L13.4115 11.9955Z"
                        fill="#373737"
                      />
                    </svg>
                  </button>
                  <button class="settings__description-button">
                    <svg
                      width="24"
                      height="24"
                      viewBox="0 0 24 24"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        opacity="0.3"
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M5.223 5H18.837C19.5851 5.00011 20.3062 5.27976 20.8588 5.78408C21.4114 6.2884 21.7557 6.98099 21.824 7.726C21.9413 9.002 22 10.401 22 11.923C22 13.705 21.9197 15.487 21.759 17.269C21.6918 18.0151 21.348 18.709 20.795 19.2144C20.2421 19.7198 19.5201 20 18.771 20H5.223C4.47581 20 3.7555 19.7212 3.20304 19.2181C2.65058 18.7151 2.30574 18.0239 2.236 17.28C2.07867 15.604 2 14.0107 2 12.5C2 10.9893 2.07867 9.396 2.236 7.72C2.30574 6.97607 2.65058 6.28494 3.20304 5.78186C3.7555 5.27878 4.47581 4.99998 5.223 5Z"
                        fill="#373737"
                      />
                      <path
                        fill-rule="evenodd"
                        clip-rule="evenodd"
                        d="M10.869 15.913L15.759 12.92C15.8177 12.8876 15.8687 12.843 15.9086 12.7891C15.9484 12.7352 15.9761 12.6733 15.9899 12.6077C16.0037 12.5421 16.0031 12.4744 15.9883 12.409C15.9735 12.3436 15.9448 12.2822 15.904 12.229C15.8644 12.1775 15.8155 12.1338 15.76 12.1L10.87 9.087C10.7473 9.01243 10.602 8.98434 10.4603 9.00783C10.3187 9.03131 10.1901 9.10482 10.098 9.215C10.0344 9.29531 9.99918 9.39451 9.99805 9.497V15.503C10.0055 15.6428 10.0682 15.774 10.1722 15.8677C10.2762 15.9614 10.4132 16.0101 10.553 16.003C10.6647 16.003 10.7741 15.9718 10.869 15.913Z"
                        fill="#373737"
                      />
                    </svg>
                  </button>
                </div> -->
                <!--  <app-textarea
                  placeholder="Опишите ваше событие"
                  @textarea-text="form.descr = $event"
                /> -->
                <ckeditor
                  v-model="form.descr"
                  placeholder="Опишите ваше событие"
                ></ckeditor>
              </div>
            </div>
            <div class="settings__all-photo settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">Фото<span>*</span></h3>
                <hr />
              </div>
              <div class="settings__images-wrapper">
                <div
                  v-for="(photo, index) in preview"
                  :key="index"
                  class="settings__img-wrapper"
                >
                  <img
                    :src="`${preview[index] || form.photos[index].url}`"
                    class="settings__img"
                    alt=""
                  />

                  <button
                    class="settings__img-button"
                    @click.prevent="deletePhoto(index)"
                  >
                    <svg
                      width="32"
                      height="32"
                      viewBox="0 0 32 32"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M27 0H5C2.23858 0 0 2.23858 0 5V27C0 29.7614 2.23858 32 5 32H27C29.7614 32 32 29.7614 32 27V5C32 2.23858 29.7614 0 27 0Z"
                        fill="white"
                      />
                      <path
                        d="M9 11H23"
                        stroke="#E62128"
                        stroke-width="1.5"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                      <path
                        d="M22.0001 11V22.846C22.0198 23.3967 21.8204 23.9327 21.4454 24.3365C21.0705 24.7403 20.5507 24.9789 20.0001 25H12.0001C11.4495 24.9789 10.9297 24.7403 10.5548 24.3365C10.1799 23.9327 9.98038 23.3967 10.0001 22.846V11"
                        stroke="#E62128"
                        stroke-width="1.5"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                      <path
                        d="M19 7.75H13"
                        stroke="#E62128"
                        stroke-width="1.5"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                      <path
                        d="M14 15V21"
                        stroke="#E62128"
                        stroke-width="1.5"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                      <path
                        d="M18 15V21"
                        stroke="#E62128"
                        stroke-width="1.5"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                    </svg>
                  </button>
                </div>

                <app-add-photo
                  :clearphoto="false"
                  @photo="form.photos.push({ file: $event })"
                  @preview="preview.push($event)"
                />
              </div>
            </div>
            <div class="settings__privety settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Приватность события<span>*</span>
                </h3>
                <hr />
              </div>
              <div class="settings__privety-wrapper">
                <app-select
                  label-name="name"
                  inputName="privacy"
                  placeholder="Выбрать"
                  v-model="form.is_privacy"
                  :options="options.privacy"
                />
                <p class="settings__text">
                  Это событие будет доступно всем участникам
                </p>
              </div>
              <div class="settings__radiobutton-wrapper radiobutton-wrapper">
                <app-radio-button
                  label="Участие в событии всех пользователей, которые подтвердили свое  участие"
                  v-model="form.confirm_user"
                  @option="form.confirm_user = !form.confirm_user"
                />
                <app-radio-button
                  label="Вручную подтверждать участие каждого пользователя"
                  v-model="form.confirm_user"
                  @option="form.confirm_user = !form.confirm_user"
                />
              </div>
            </div>
            <div class="settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Участники события <span>*</span>
                </h3>
                <hr />
              </div>
              <div class="settings__user-wrapper">
                <app-input
                  placeholder="Не имеет значения"
                  v-model="form.count_seats"
                />
                <app-select
                  label-name="name"
                  input-name="age"
                  placeholder="Не имеет значения"
                  v-model="form.age"
                  :options="options.ages"
                />
                <app-select
                  label-name="name"
                  placeholder="Не имеет значения"
                  v-model="form.sex"
                  :options="options.gender"
                />
              </div>
            </div>
            <div class="settings__block">
              <div class="settings__subtitle">
                <h3 class="settings__subtitle-text">
                  Действия посетителей <span>*</span>
                </h3>
                <hr />
              </div>
              <app-swich
                label="Разрешить комментарии"
                v-model="form.comment_allowed"
              />
              <app-swich
                label="Разрешить добавление фотографий в альбом"
                v-model="form.photos_allowed"
              />
            </div>
            <div class="settings__form-buttons">
              <button class="default-button">отмена</button>
              <button @click.prevent="getSetting()" class="default-button__red">
                Опубликовать
              </button>
            </div>
          </form>
        </main>
      </div>
    </div>
  </div>
</template>
<script>
import qs from "qs";
import _ from "lodash";
import { mapActions } from "vuex";
import { serialize } from "object-to-formdata";
export default {
  components: {},
  data() {
    return {
      form: {
        lang: "ru",
        main_photo: { file: null },
        photos: [],
        title: "",
        descr: "",
        coauthor: null,
        category: "",
        country: "Ukraine",
        place: "",
        street: "",
        address: "Lutsk",
        latitude: 12,
        longitude: 21,
        is_privacy: "",
        confirm_user: true,
        users: [],
        count_seats: "",
        age: "",
        sex: "",
        comment_allowed: false,
        self_schedule_dates: false,
        external: {
          name: "",
          link: "",
        },
        more_days: false,
        dates_continuous: false,
        external_source: false,
        chat_allowed: false,
        photos_allowed: false,
        dates: {
          date: {
            full: "",
            from: "",
            to: "",
          },
          time: {
            full: "",
            from: "",
            to: "",
          },
          items: [],
        },
      },

      dates: "",
      preview: [],
      options: {},
    };
  },
  async created() {
    this.fetchData();
  },
  computed: {
    currentDate() {
      return moment(this.dates).format("DD MMMM");
    },
    currentDateWith() {
      return moment(this.dates[0]).format("DD MMMM");
    },
    currentDateTo() {
      return moment(this.dates[1]).format("DD MMMM");
    },
  },

  methods: {
    async fetchData() {
      try {
        let editSetting = await this.$api.event.eventEdit(
          this.$route.params.id
        );
        this.form = editSetting.data.event;
        this.options.ages = editSetting.data.ages;
        this.options.categories = editSetting.data.categories;
        this.options.gender = editSetting.data.gender;
        this.options.privacy = editSetting.data.privacy;
        this.photoPreview(this.form.photos);
        this.form = editSetting.data.event;
        this.form.category = editSetting.data.categories.find(
          (element) => element.name === this.form.category
        );
        this.form.age = editSetting.data.ages.find(
          (element) => element.name === this.form.age
        );
        this.form.privacy = editSetting.data.privacy.find(
          (element) => element.name === this.form.privacy
        );
        this.form.sex = editSetting.data.gender.find(
          (element) => element.name === this.form.sex
        );
      } catch (e) {
        console.log(e);
      }
    },
    timeText(e) {
      this.form.dates.time.full = [e.split(" ")[1], e.split(" ")[3]];
      this.form.dates.time.from = e.split(" ")[1];
      this.form.dates.time.to = e.split(" ")[3];
    },

    dateText(e) {
      this.dates = e;
      this.dateSubmit();
      this.form.dates.items = [];
      this.addListDate();
    },
    dateItemTime(e, index) {
      this.form.dates.items[index].time.full = [
        e.split(" ")[1],
        e.split(" ")[3],
      ];
      this.form.dates.items[index].time.from = e.split(" ")[1];
      this.form.dates.items[index].time.to = e.split(" ")[3];
    },
    dateSubmit() {
      if (this.dates.length > 1) {
        this.form.dates.date.full = [this.dates[0], this.dates[1]];
        this.form.dates.date.from = this.dates[0];
        this.form.dates.date.to = this.dates[1];
      } else {
        this.form.dates.date.full = this.dates;
        this.form.dates.date.from = "";
        this.form.dates.date.to = "";
      }
    },
    dateRange(e) {
      this.form.more_days = e;
      this.dates = "";
      this.form.dates.date.full = "";
      this.form.dates.date.from = "";
      this.form.dates.date.to = "";
      this.form.dates.items = [];
    },
    async deletePhoto(index) {
      try {
        if (this.form.photos[index]?.url) {
          await this.$api.medias.deletePhoto(this.form.photos[index].id);
        }
        this.form.photos.splice(index, 1);
        this.preview.splice(index, 1);
      } catch (e) {
        console.log(e);
      }
    },
    async getSetting() {
      try {
        const options = {
          indices: true,
          nullsAsUndefineds: false,
          booleansAsIntegers: true,
        };
        const formData = serialize(this.form, options);
        formData.append("_method", "PUT");
        let { data } = await this.$api.event.eventUpdate(
          this.$route.params.id,
          formData
        );
      } catch (e) {
        console.log(e);
      }
    },
    buildQuery(f) {
      return qs.stringify(JSON.parse(JSON.stringify(f)));
    },
    addListDate() {
      let new_date = moment(this.dates[0]).format("DD.MM.YYYY");
      let new_date2 = moment(this.dates[1]).format("DD.MM.YYYY");
      this.form.dates.items.push({
        day: moment(this.dates[0]).format("DD MMMM"),
        time: { full: "", from: "", to: "" },
      });
      while (new_date !== new_date2) {
        new_date = moment(new_date, "DD.MM.YYYY").add("days", 1);
        let day = new_date.format("DD");
        let month = new_date.format("MM");
        let month2 = new_date.format("MMMM");
        let year = new_date.format("YYYY");
        new_date = `${day + "." + month + "." + year}`;
        this.form.dates.items.push({
          day: `${day + " " + month2}`,
          time: { full: "", to: "", from: "" },
        });
      }
    },
    photoPreview(photos) {
      photos.forEach((element) => {
        this.preview.push(element.url);
      });
    },
  },
};
</script>