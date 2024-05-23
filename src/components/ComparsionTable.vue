<template>
  <div>
    <!-- заголовок -->
    <div class="head">
      <h1 class="title">Смартфоны</h1>
      <p class="display-products">
        Отобразить товары:
        <a
          v-for="num in availbleCounts"
          :key="num"
          class="button-product"
          :class="{ active: num === numberOfProduct }"
          @click="updateCount(num)"
        >
          {{ num }}
        </a>
      </p>
    </div>
    <!-- начало таблицы -->
    <div class="table-container">
      <table class="table">
        <!-- Верхняя часть таблицы -->
        <thead class="thead">
          <tr>
            <th>
              <label class="checkbox-label">
                <input
                  type="checkbox"
                  v-model="showDifferences"
                  class="checkbox-input"
                />
                <span>Показать различия</span>
              </label>
            </th>
            <!-- ячейки с товарами -->
            <th
              v-for="(phone, index) in displayedPhones"
              :key="index"
              class="phone-header"
              @click="selectedPhoneIndex = index"
            >
              <!-- карточка телефона -->
              <div class="phone-card">
                <div>
                  <img
                    :src="require(`@/assets/${phone.image}`)"
                    :alt="phone.name"
                    class="phone-image"
                  />
                  <p class="phone-name">{{ phone.name }}</p>
                </div>
                <!-- дропдаун -->
                <details v-if="hiddenPhones.length" class="dropdown">
                  <summary role="button">
                    <svg
                      width="30"
                      height="27"
                      viewBox="0 0 30 27"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M24.375 10.125L15 18.5625L5.625 10.125"
                        stroke="#0D5ADC"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      />
                    </svg>
                  </summary>
                  <!-- дропдаун элементы -->
                  <div class="dropdown-content">
                    <input
                      v-if="hiddenPhones.length >= 3"
                      class="input-search"
                      type="text"
                      v-model="searchQuery"
                      placeholder="Поиск"
                      @input="filterPhones"
                    />
                    <ul>
                      <li
                        v-for="hiddenPhone in filteredPhones"
                        :key="hiddenPhone.name"
                      >
                        <button
                          class="dropdown-btn double-arrow"
                          @click="replacePhone(hiddenPhone)"
                        >
                          <svg
                            class="reverse-arrow"
                            width="20"
                            height="20"
                            viewBox="0 0 20 20"
                            fill="none"
                            xmlns="http://www.w3.org/2000/svg"
                          >
                            <path
                              d="M19.0909 4.54548H3.10384L6.09735 1.55191C6.45238 1.19694 6.45238 0.621302 6.09735 0.266272C5.74239 -0.0887575 5.16675 -0.0887575 4.81172 0.266272L0.266272 4.81172C-0.0887575 5.16669 -0.0887575 5.74233 0.266272 6.09735L4.81172 10.6428C4.98923 10.8203 5.2219 10.9091 5.45457 10.9091C5.68723 10.9091 5.9199 10.8203 6.09735 10.6428C6.45238 10.2878 6.45238 9.7122 6.09735 9.35717L3.10384 6.36366H19.0909C19.593 6.36366 20 5.95663 20 5.45457C20 4.95251 19.593 4.54548 19.0909 4.54548Z"
                              fill="#36935B"
                            />
                            <path
                              d="M15.1883 9.35717C14.8333 9.0022 14.2576 9.0022 13.9026 9.35717C13.5476 9.71214 13.5476 10.2878 13.9026 10.6428L16.8962 13.6364H0.90912C0.40706 13.6364 3.02863e-05 14.0434 3.02863e-05 14.5455C3.02863e-05 15.0475 0.40706 15.4546 0.90912 15.4546H16.8962L13.9027 18.4481C13.5476 18.8031 13.5476 19.3787 13.9027 19.7338C14.0801 19.9112 14.3128 20 14.5455 20C14.7781 20 15.0108 19.9112 15.1883 19.7337L19.7337 15.1883C20.0887 14.8333 20.0887 14.2576 19.7337 13.9026L15.1883 9.35717Z"
                              fill="#36935B"
                            />
                          </svg>
                        </button>
                        <img
                          :src="require(`@/assets/${hiddenPhone.image}`)"
                          :alt="hiddenPhone.name"
                          class="phone-image-dropdown"
                        />
                        <span class="hidPhone-text">
                          {{ hiddenPhone.name }}
                        </span>
                      </li>
                    </ul>
                  </div>
                </details>
              </div>
            </th>
          </tr>
        </thead>
        <!-- блок характерик -->
        <tbody>
          <div class="fon"></div>
          <tr
            v-for="(attribute, index) in filteredAttributes"
            :key="index"
            class="table-row"
          >
            <td class="labels">{{ attribute.label }}</td>
            <!-- свойства да/нет -->
            <td v-for="(phone, idx) in displayedPhones" :key="idx">
              <span
                v-if="
                  attribute.key === 'nfc' ||
                  attribute.key === 'esim' ||
                  attribute.key === 'wirelessCharging'
                "
              >
                <span v-if="phone[attribute.key]">
                  <svg
                    width="22"
                    height="22"
                    viewBox="0 0 22 22"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      fill-rule="evenodd"
                      clip-rule="evenodd"
                      d="M22 11C22 17.0751 17.0751 22 11 22C4.92487 22 0 17.0751 0 11C0 4.92487 4.92487 0 11 0C17.0751 0 22 4.92487 22 11ZM9.01982 12.9508L16.7961 5.17451L18.7334 7.11161L9.01982 16.8255L3.2667 11.0722L5.20408 9.13514L9.01982 12.9508Z"
                      fill="#36935B"
                    />
                  </svg>
                </span>
                <span v-else>
                  <svg
                    width="22"
                    height="22"
                    viewBox="0 0 22 22"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      fill-rule="evenodd"
                      clip-rule="evenodd"
                      d="M11 22C17.0751 22 22 17.0751 22 11C22 4.92487 17.0751 0 11 0C4.92487 0 0 4.92487 0 11C0 17.0751 4.92487 22 11 22ZM8.75981 11.0304L4 15.7888L6.2734 18.0629L11.0338 13.3038L15.7266 17.9952L18 15.7211L13.3079 11.0304L17.9979 6.34168L15.7245 4.06764L11.0338 8.75697L6.27549 3.99996L4.00209 6.274L8.75981 11.0304Z"
                      fill="#EF4058"
                    />
                  </svg>
                </span>
              </span>
              <span v-else>{{ phone[attribute.key] }}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from "vue";

// Определение интерфейса Phone, который описывает структуру объекта телефона
interface Phone {
  name: string;
  image: string;
  manufacturer: string;
  releaseYear: number;
  screenSize: number;
  country: string;
  memory: string;
  refreshRate: string;
  nfc: boolean;
  esim: boolean;
  wirelessCharging: boolean;
  price: string;
}

// Определение интерфейса Attribute, который описывает структуру объекта атрибута
interface Attribute {
  key: keyof Phone;
  label: string;
}

export default defineComponent({
  name: "ComparsionTable",
  setup() {
    // Состояние, определяющее, показывать ли только различающиеся атрибуты
    const showDifferences = ref(false);
    // Состояние, определяющее количество отображаемых товаров
    const numberOfProduct = ref(3);
    // Состояние для поиска
    const searchQuery = ref("");
    // Состояние, определяющее индекс выбранного телефона
    const selectedPhoneIndex = ref(-1);
    // Список телефонов
    const phones = ref<Phone[]>([
      {
        name: "Apple iPhone 12",
        image: "iphone12.png",
        manufacturer: "Apple",
        releaseYear: 2020,
        screenSize: 6.1,
        country: "Китай",
        memory: "128 ГБ",
        refreshRate: "60",
        nfc: false,
        esim: true,
        wirelessCharging: true,
        price: "81990 ₽",
      },
      {
        name: "Xiaomi Mi 11 Lite",
        image: "xiaomi_mi_11_lite.png",
        manufacturer: "Xiaomi",
        releaseYear: 2021,
        screenSize: 6.55,
        country: "Китай",
        memory: "128 ГБ",
        refreshRate: "90",
        nfc: true,
        esim: true,
        wirelessCharging: false,
        price: "27490 ₽",
      },
      {
        name: "Samsung Galaxy A72",
        image: "samsung_a72.png",
        manufacturer: "Samsung",
        releaseYear: 2021,
        screenSize: 6.7,
        country: "Вьетнам",
        memory: "128 ГБ",
        refreshRate: "90",
        nfc: true,
        esim: false,
        wirelessCharging: true,
        price: "32890 ₽",
      },
      {
        name: "Samsung Galaxy S21",
        image: "samsung_s21.png",
        manufacturer: "Samsung",
        releaseYear: 2021,
        screenSize: 6.2,
        country: "Вьетнам",
        memory: "128 ГБ",
        refreshRate: "120",
        nfc: true,
        esim: true,
        wirelessCharging: true,
        price: "69990 ₽",
      },
      {
        name: "Apple iPhone Xr",
        image: "iphone_xr.png",
        manufacturer: "Apple",
        releaseYear: 2018,
        screenSize: 6.1,
        country: "Китай",
        memory: "64 ГБ",
        refreshRate: "60",
        nfc: true,
        esim: true,
        wirelessCharging: true,
        price: "59990 ₽",
      },
      {
        name: "Realme 8 Pro",
        image: "realme_8pro.png",
        manufacturer: "Realme",
        releaseYear: 2021,
        screenSize: 6.4,
        country: "Китай",
        memory: "128 ГБ",
        refreshRate: "60",
        nfc: true,
        esim: false,
        wirelessCharging: false,
        price: "24990 ₽",
      },
    ]);

    // Список атрибутов для отображения в таблице
    const attributes: Attribute[] = [
      { key: "manufacturer", label: "Производитель" },
      { key: "releaseYear", label: "Год Релиза" },
      { key: "screenSize", label: "Диагональ экрана (дюйм)" },
      { key: "country", label: "Страна-производитель" },
      { key: "memory", label: "Объем Памяти" },
      { key: "refreshRate", label: "Частота обновления экрана" },
      { key: "nfc", label: "NFC" },
      { key: "esim", label: "Поддержка esim" },
      { key: "wirelessCharging", label: "Беспроводная зарядка" },
      { key: "price", label: "Стоимость" },
    ];

    // Функция для проверки наличия различий в значениях атрибутов
    const hasDifference = (attribute: keyof Phone): boolean => {
      const firstValue = displayedPhones.value[0][attribute];
      return displayedPhones.value.some(
        (phone) => phone[attribute] !== firstValue
      ); // проверяем отличается ли хоть одно значение и возвращаем true/false
    };

    // Используем вычисляемое свойство для фильтрации и отображения строк
    const filteredAttributes = computed(() => {
      if (!showDifferences.value) {
        return attributes; // Если чекбокс без галки возвращаем все атрибуты
      }
      return attributes.filter((attribute) => hasDifference(attribute.key)); // Если чекбокс с галкой, проходимся фильтром по массиву атрибутов и оставляем те, где hasDifference возвращает true
    });

    // Функция для обновления количества отображаемых товаров
    const updateCount = (count: number) => {
      numberOfProduct.value = count;
    };

    // Вычисляемое свойство для получения доступных значений количества отображаемых товаров
    const availbleCounts = computed(() => {
      const maxCount = Math.min(6, phones.value.length);
      return Array.from({ length: maxCount - 1 }, (_, i) => i + 2);
    });

    // Вычисляемое свойство для получения отображаемых телефонов
    const displayedPhones = computed(() => {
      return phones.value.slice(0, numberOfProduct.value);
    });

    // Вычисляемое свойство для получения скрытых телефонов
    const hiddenPhones = computed(() => {
      return phones.value.slice(numberOfProduct.value);
    });

    // Функция для фильтрации телефонов по поисковому запросу
    const filterPhones = (): Phone[] => {
      if (!searchQuery.value) return hiddenPhones.value;
      return hiddenPhones.value.filter((phone) =>
        phone.name.toLocaleLowerCase().includes(searchQuery.value.toLowerCase())
      );
    };

    // Вычисляемое свойство для получения отфильтрованных телефонов
    const filteredPhones = computed(() => filterPhones());

    // Функция для замены телефона
    const replacePhone = (newPhone: Phone) => {
      if (selectedPhoneIndex.value === -1) return;

      // Получаем индекс старого телефона, который будет заменен
      const oldPhone = displayedPhones.value[selectedPhoneIndex.value];

      // Находим индекс нового телефона в списке hiddenPhones
      const newPhoneIndex = phones.value.findIndex(
        (phone) => phone === newPhone
      );

      // Заменяем старый телефон на новый
      phones.value.splice(newPhoneIndex, 1, oldPhone);
      phones.value.splice(phones.value.indexOf(oldPhone), 1, newPhone);

      // Сбрасываем индекс выбранного телефона
      selectedPhoneIndex.value = -1;
    };

    return {
      showDifferences,
      phones,
      numberOfProduct,
      displayedPhones,
      filteredAttributes,
      updateCount,
      hiddenPhones,
      searchQuery,
      filterPhones,
      filteredPhones,
      availbleCounts,
      replacePhone,
      selectedPhoneIndex,
    };
  },
});
</script>

<style scoped lang="scss">
%typography {
  font-family: Roboto;
  font-size: 18px;
  font-weight: 500;
  line-height: 21.09px;
  color: #3b4157;
}
/* стили заголовка */
.head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0 166px 20px 166px;
  height: 60px;
}

.title {
  font-family: Roboto;
  font-size: 48px;
  font-weight: 700;
  line-height: 60px;
  letter-spacing: 0.02em;
  text-align: left;
  color: #828286;
  margin-bottom: 20px;
}
.display-products {
  font-family: Roboto;
  font-size: 18px;
  font-weight: 400;
  line-height: 60px;
  letter-spacing: 0.02em;
  text-align: left;
  color: #0d5adc;
  margin-bottom: 20px;
}
.button-product {
  cursor: pointer;
  margin-right: 5px;
}
.button-product.active {
  font-weight: bold;
  text-decoration: underline;
}
/* Таблица */
.table-container {
  padding-left: 166px;
  padding-right: 166px;
  padding-bottom: 91px;
}
.table {
  width: 100%;
  border-collapse: collapse;
}
/* верхяя часть таблицы */
.thead {
  background: white;
}
.checkbox-label {
  display: flex;
  @extend %typography;
  color: #0d5adc;
  cursor: pointer;
  margin: 76px 0 0 0;
  justify-self: end;
}
.checkbox-input {
  margin-right: 10px;
}
/* карточка телефона */
.phone-card {
  width: fit-content;
  text-align: center;
  display: flex;
}
.phone-image {
  width: 64.8px;
  height: 120px;
  margin-bottom: 8px;
}
.phone-name {
  @extend %typography;
  text-align: center;
  margin-bottom: 74px;
}
/* дропдаун */
.dropdown {
  position: relative;
  top: 71px;
}
.dropdown summary {
  list-style: none;
  list-style-type: none;
  padding: 0;
  cursor: pointer;
}

.dropdown > summary::-webkit-details-marker {
  display: none;
}

.dropdown summary:focus {
  outline: none;
}
.dropdown[open] > summary::before {
  content: " ";
  display: block;
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  z-index: 1;
}
/* дропдаун элементы */
.dropdown-content {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 421px;
  max-height: 336px;
  overflow-y: auto;
  box-sizing: border-box;
  z-index: 2;
  background: #ffffff;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
}
.input-search {
  width: 364px;
  height: 47px;
  border-radius: 5px;
  margin: 34px 0 18px 0;
  border-width: 1px;
  border-color: #c1c1c1;
  padding: 0 0 0 16px;
  font-family: Roboto;
  font-size: 24px;
  font-weight: 400;
  line-height: 28.13px;
  text-align: left;
  outline: none;
}
/* дропдаун лист */
.dropdown ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.dropdown ul li {
  padding: 15px 0 15px 0;
  display: flex;
  align-items: center;
}
.dropdown ul li a:link,
.dropdown ul li a:visited {
  width: 100%;
  box-sizing: border-box;
  color: var(--dropdown-color);
  text-decoration: none;
}
.dropdown ul li a:hover {
  background-color: #f0f0f0;
}
.dropdown ul::before {
  content: " ";
  position: absolute;
  width: 0;
  height: 0;
  top: -10px;
  left: 50%;
  transform: translateX(-50%);
  border-style: solid;
  border-width: 0 10px 10px 10px;
  border-color: transparent transparent #ffffff transparent;
}
/* дропдаун item */
.dropdown-btn {
  background: none;
  border: none;
  cursor: pointer;
}
.reverse-arrow {
  width: fit-content;
  margin: 0 18px 0 18px;
}
.phone-image-dropdown {
  width: 24px;
  height: 50px;
}
.hidPhone-text {
  font-family: Roboto;
  font-size: 18px;
  font-weight: 400;
  line-height: 40px;
  text-align: left;
  margin-left: 23px;
}
/* блок характеристик */
.table-row {
  border-bottom: 1px solid #cdcfd2;
  height: 114px;
  background-color: #f4f9fc;
}
.fon {
  z-index: -1;
  width: 100%;
  left: 0;
  bottom: 0;
  background-color: #f4f9fc;
  padding-bottom: 91px;
}
.labels {
  width: 255px;
  @extend %typography;
  text-align: left;
  text-transform: uppercase;
  color: #a4a9b9;
  padding: 10px;
}
td {
  @extend %typography;
  text-align: left;
}
/* скроллбар */
/* width */
::-webkit-scrollbar {
  width: 15px;
}
/* Track */
::-webkit-scrollbar-track {
  background-color: #ffffff;
}
/* Handle */
::-webkit-scrollbar-thumb {
  background: #e3e3e3;
  border: 5px solid #ffffff;
  border-radius: 8px;
}
</style>
