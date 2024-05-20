<template>
  <div class="">
    <div class="head"><!-- заголовок -->      
      <h1 class="title">Смартфоны</h1>
      <p>Отобразить товары</p>
    </div>
     <div><!--картинки телефонов + чекбокс -->
      <p v-for="(phone, index) in phones" :key="index">{{ phone.name }}</p>
      <label>
        <input type="checkbox"  v-model="showDifferences"/> <!-- чекбокс управляющий filteredAtributes -->
        Показать различия
      </label>
    </div>
    <hr>
    <table><!--  таблица характеристик-->
        <tbody>
          <tr v-for="(attribute, index) in filteredAttributes" :key="index"><!-- итерируемся по строкам и вставляем значения в ячейки из массива -->
            <td>{{ attribute.label }}</td> <!-- ячейки с названиями -->
            <td v-for="(phone, idx) in phones" :key="idx"> <!-- ячейки с значениями -->
              <span v-if=" 
                attribute.key === 'nfc' ||
                attribute.key === 'esim' ||
                attribute.key === 'wirelessCharging'
              ">
                <span v-if="phone[attribute.key]">yes</span> <!-- для 3 полей определяем логику отображения да/нет. Для остальных просто выводим значения из массива -->
                <span v-else>no</span>
              </span>
              <span v-else>{{ phone[attribute.key] }}</span> 
            </td> 
          </tr>
        </tbody>
    </table>

    
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from "vue";

interface Phone {
  name: string;
  manufacturer: string;
  releaseYear: number;
  screenSize: number;
  country: string;
  memory: string;
  refreshRate: string;
  nfc: boolean;
  esim: boolean;
  wirelessCharging: boolean;
  price: number;
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
        name: 'Apple iPhone 12',
        manufacturer: 'Apple', 
        releaseYear: 2020, 
        screenSize: 6.1, 
        country: 'Китай', 
        memory: '128 ГБ', 
        refreshRate: '60', 
        nfc: false, 
        esim: true, 
        wirelessCharging: true, 
        price: 81990, 
      },
      {
        name: 'Xiaomi Mi 11 Lite',
        manufacturer: 'Xiaomi', 
        releaseYear: 2021, 
        screenSize: 6.55, 
        country: 'Китай', 
        memory: '128 ГБ', 
        refreshRate: '90', 
        nfc: true, 
        esim: true, 
        wirelessCharging: false, 
        price: 27490, 
      },
      {
        name: 'Samsung Galaxy A72',
        manufacturer: 'Samsung', 
        releaseYear: 2021, 
        screenSize: 6.7, 
        country: 'Вьетнам', 
        memory: '128 ГБ', 
        refreshRate: '90', 
        nfc: true, 
        esim: false, 
        wirelessCharging: true, 
        price: 32890, 
      },
    ])
  const attributes: Attribute[] = [
    {key: "manufacturer", label: "Производитель"},
    {key: "releaseYear", label: "Год Релиза"},
    {key: "screenSize", label: "Диагональ экрана (дюйм)"},
    {key: "country", label: "Страна-производитель"},
    {key: "memory", label: "Объем Памяти"},
    {key: "refreshRate", label: "Частота обновлеия экрана"},
    {key: "nfc", label: "NFC"},
    {key: "esim", label: "Поддержка esim"},
    {key: "wirelessCharging", label: "Безпроводная зарядка"},
    {key: "price", label: "Стоимость"},
  ];
  const hasDifference = (attribute: keyof Phone) :boolean => { //проверка аттрибутов на отличия 
    const firstValue = phones.value[0][attribute]; // берем атрибут первого телефона из списка
    return phones.value.some((phone) => phone[attribute] !== firstValue); // проверяем отличается ли хоть одно значение и возвращаем true/false 
  };

  const filteredAttributes = computed (() => { //используем вычисляемое свойство для фильтрации и отображения строк
    if (!showDifferences.value) { //Если чекбокс без галки возвращаем все атрибуты
      return attributes;
    }
    return attributes.filter((attribute)=> hasDifference(attribute.key)); //если чекбокс с галкой проходимся фильтром по массиву атрибутов и оставляем те где hasDifference возвращает true
  })
    return {
      showDifferences,
      phones,
      filteredAttributes,
    };
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.title {
  font-family: Roboto;
  font-size: 48px;
  font-weight: 700;
  line-height: 60px;
  letter-spacing: 0.02em;
  text-align: left;
  color: #828286;
}
</style>
