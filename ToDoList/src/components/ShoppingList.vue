<template>
  <div class="shopping-list">
    <h1>Список покупок</h1>
    <div class="input-container">
      <input
          type="text"
          v-model="newItem.name"
          placeholder="Название товара"
          @keyup.enter="addItem"
      />
      <input
          type="number"
          v-model.number="newItem.price"
          placeholder="Цена"
          @keyup.enter="addItem"
      />
      <button @click="addItem">Добавить</button>
    </div>
    <ul class="item-list">
      <li v-for="(item, index) in items" :key="index" class="item">
        <span class="item-name">{{ item.name }}</span>
        <span class="item-price">{{ item.price.toFixed(2) }} ₽</span>
        <button class="delete-button" @click="removeItem(index)">Удалить</button>
      </li>
    </ul>
    <p v-if="items.length === 0" class="empty-list">Список пуст</p>
    <p v-else class="total-price">Итого: {{ total.toFixed(2) }} ₽</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from "vue";

interface Item {
  name: string;
  price: number;
}

export default defineComponent({
  name: "ShoppingList",
  setup() {
    // Reactive данные
    const items = reactive<Item[]>([]);
    const newItem = reactive<Item>({ name: "", price: 0 });

    // Вычисляемая общая стоимость
    const total = computed(() =>
        items.reduce((sum, item) => sum + item.price, 0)
    );

    // Добавить элемент в список
    const addItem = () => {
      if (!newItem.name.trim() || newItem.price <= 0) {
        alert("Введите корректные данные");
        return;
      }
      items.push({ ...newItem });
      newItem.name = "";
      newItem.price = 0;
    };

    // Удалить элемент из списка
    const removeItem = (index: number) => {
      items.splice(index, 1);
    };

    return {
      items,
      newItem,
      total,
      addItem,
      removeItem,
    };
  },
});
</script>

<style scoped>
/* Основной контейнер */
.shopping-list {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: #1e1e1e; /* Темный фон */
  color: #f9f9f9;      /* Светлый текст */
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.5);
}

/* Контейнер для ввода */
.input-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  flex: 1 1 calc(50% - 20px);
  padding: 10px;
  font-size: 16px;
  border: 1px solid #555; /* Цвет рамки */
  background: #2e2e2e;   /* Темный фон для ввода */
  color: #f9f9f9;        /* Цвет текста */
  border-radius: 5px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #007bff; /* Яркий синий */
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0056b3; /* Темнее при наведении */
}

/* Список товаров */
.item-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #555; /* Цвет разделителей */
}

.item-name {
  font-weight: bold;
}

.item-price {
  color: #f0c674; /* Золотистый цвет */
}

.delete-button {
  background-color: #dc3545; /* Красный */
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #a71d2a; /* Темнее при наведении */
}

/* Общая стоимость */
.total-price {
  font-size: 18px;
  font-weight: bold;
  text-align: right;
  margin-top: 20px;
}

.empty-list {
  font-size: 16px;
  text-align: center;
  color: #888; /* Серый цвет */
}

/* Адаптивность */
@media (max-width: 768px) {
  .input-container {
    flex-direction: column;
    gap: 10px;
  }

  input {
    flex: 1 1 100%;
  }

  button {
    width: 100%;
  }

  .item {
    flex-wrap: wrap;
    text-align: center;
  }

  .item-name,
  .item-price,
  .delete-button {
    flex: 1 1 100%;
    margin: 5px 0;
  }
}
</style>

