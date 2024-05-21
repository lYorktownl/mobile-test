<template>
  <div>
    <div class="head">
      <h1 class="title">Смартфоны</h1>
      <p class="display-products">
        Отобразить товары:
        <a class="button"> 2 </a>
        <a class="button">3 </a>
        <a class="button">4 </a>
        <a class="button">5 </a>
        <a class="button">6</a>
      </p>
    </div>
    <label class="checkbox-label">
      <input type="checkbox" v-model="showDifferences" class="checkbox-input" />
      <span>Показать различия</span>
    </label>
    <div class="table-container">
      <table class="table">
        <thead class="thead">
          <tr>
            <th></th>
            <th
              v-for="(phone, index) in phones"
              :key="index"
              class="phone-header"
            >
              <img
                :src="require(`@/assets/${phone.image}`)"
                :alt="phone.name"
                class="phone-image"
              />
              <p class="phone-name">{{ phone.name }}</p>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(attribute, index) in filteredAttributes"
            :key="index"
            class="table-row"
          >
            <td class="labels">{{ attribute.label }}</td>
            <td v-for="(phone, idx) in phones" :key="idx">
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
interface Attribute {
  key: keyof Phone;
  label: string;
}
export default defineComponent({
  name: "ComparsionTable",
  setup() {
    const showDifferences = ref(false);
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
        name: "Samsung Galaxy A72",
        image: "samsung_s21.png",
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
        name: "Samsung Galaxy A72",
        image: "iphone_xr.png",
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
        name: "Samsung Galaxy A72",
        image: "realme_8pro.png",
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
    ]);
    const attributes: Attribute[] = [
      { key: "manufacturer", label: "Производитель" },
      { key: "releaseYear", label: "Год Релиза" },
      { key: "screenSize", label: "Диагональ экрана (дюйм)" },
      { key: "country", label: "Страна-производитель" },
      { key: "memory", label: "Объем Памяти" },
      { key: "refreshRate", label: "Частота обновлеия экрана" },
      { key: "nfc", label: "NFC" },
      { key: "esim", label: "Поддержка esim" },
      { key: "wirelessCharging", label: "Беспроводная зарядка" },
      { key: "price", label: "Стоимость" },
    ];
    //проверка аттрибутов на отличия
    const hasDifference = (attribute: keyof Phone): boolean => {
      const firstValue = phones.value[0][attribute]; // берем атрибут первого телефона из списка
      return phones.value.some((phone) => phone[attribute] !== firstValue); // проверяем отличается ли хоть одно значение и возвращаем true/false
    };
    //используем вычисляемое свойство для фильтрации и отображения строк
    const filteredAttributes = computed(() => {
      //Если чекбокс без галки возвращаем все атрибуты
      if (!showDifferences.value) {
        return attributes;
      }
      return attributes.filter((attribute) => hasDifference(attribute.key)); //если чекбокс с галкой проходимся фильтром по массиву атрибутов и оставляем те где hasDifference возвращает true
    });

    return {
      showDifferences,
      phones,
      filteredAttributes,
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
.head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0 166px 20px 166px;
  height: 60px;
}
a.button {
  cursor: pointer;
  color: #0d5adc;
}
a.button :active {
  text-decoration: underline;
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
.phones-block {
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 100%;
}
.checkbox-label {
  display: flex;
  @extend %typography;
  color: #0d5adc;
  cursor: pointer;
  margin: 0 0 0 0;
  position: absolute;
  top: 330px;
  left: 166px;
}
.checkbox-input {
  margin-right: 10px;
}
.phone-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}
.thead {
  background: white;
}
.phone-header {
  text-align: center;
  margin-left: -50px;
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
.table-container {
  padding-left: 166px;
  padding-right: 166px;
  padding-bottom: 91px;
}
.table {
  width: 100%;
  border-collapse: collapse;
  // background-color: #f4f9fc;
}
.table-row {
  border-bottom: 1px solid #cdcfd2;
  height: 114px;
  background-color: #f4f9fc;
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
svg {
  margin: auto;
}
</style>
