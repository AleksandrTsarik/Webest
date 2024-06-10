<template>
  <main class="container">
    <section>
        <header class="header">
            <h2>Каталог</h2>

            <div class="cart-count"></div>
        </header>

        <div class="catalog">
          <article v-for="(item , i) in card" :key="i" :id="`product-${item.id}`" class="product-card">
            <a href="#" class="product-card__link product-card__cover">
              <img :src="item.img" :alt="item.imgAlt" class="product-card__image">
            </a>
            <h4 class="product-card__title">{{ item.nameProduct }}</h4>
            <div class="counter">
              <button class="counter__decrease" @click="decrementItem(item.id)">-</button>
              <label>
                <input @input="replaceNumber($event, item.id)" type="text" class="counter__input" :value="getCurrentCount(item.id)" :ref="`item_count_${item.id}`">
              </label>
              <button class="counter__increase" @click="incrementItem(item.id)">+</button>
            </div>
            <p class="product-card__description" hidden>{{ item.description }}</p>
            <button class="product-card__add-to-cart" type="button" @click="viewing(item)">Просмотр товара</button>
          </article>
        </div>
    </section>

    <div v-if="viewItem"  @click="closeViewingItem" class="modal-product__backdrop"></div>

    <div v-if="viewItem" class="modal-product">
      <div @click="closeViewingItem" class="modal-product__close"></div>
      <h4 class="modal-product__title">{{ viewItem.nameProduct }}</h4>
      <img :src="viewItem.img" :alt="viewItem.imgAlt" class="product-card__image">
      <p>{{ viewItem.description }}</p>
      <div class="counter">
        <button class="counter__decrease" @click="decrementItem(viewItem.id)">-</button>
        <label>
          <input @input="replaceNumber($event, viewItem.id)" type="text" class="counter__input" :value="getCurrentCount(viewItem.id)" :ref="`item_viewing_count_${viewItem.id}`">
        </label>
        <button class="counter__increase" @click="incrementItem(viewItem.id)">+</button>
      </div>
      <button class="product-card__add-to-cart" type="button" @click="addToCart(viewItem)">В корзину</button>
    </div>    
</main>
</template>

<script>
export default {
  data() {
    return {
      viewItem: null,
      card: [
        {
          id: 1,
          img: 'https://via.placeholder.com/330x200',
          imgAlt: 'Изображение товара',
          nameProduct: 'Название товара 1',
          description: 'Легкий высококачественный портативный долговечный многофункциональный инновационный компактный надежный экономичный интуитивный',
        },
        {
          id: 2,
          img: 'https://via.placeholder.com/330x200',
          imgAlt: 'Изображение товара',
          nameProduct: 'Название товара 2',
          description: 'Легкий высококачественный портативный долговечный многофункциональный инновационный компактный надежный экономичный интуитивный',
        },
        {
          id: 3,
          img: 'https://via.placeholder.com/330x200',
          imgAlt: 'Изображение товара',
          nameProduct: 'Название товара 3',
          description: 'Легкий высококачественный портативный долговечный многофункциональный инновационный компактный надежный экономичный интуитивный',
        },
        {
          id: 4,
          img: 'https://via.placeholder.com/330x200',
          imgAlt: 'Изображение товара',
          nameProduct: 'Название товара 4',
          description: 'Легкий высококачественный портативный долговечный многофункциональный инновационный компактный надежный экономичный интуитивный',
        },
        {
          id: 5,
          img: 'https://via.placeholder.com/330x200',
          imgAlt: 'Изображение товара',
          nameProduct: 'Название товара 5',
          description: 'Легкий высококачественный портативный долговечный многофункциональный инновационный компактный надежный экономичный интуитивный',
        },
        {
          id: 6,
          img: 'https://via.placeholder.com/330x200',
          imgAlt: 'Изображение товара',
          nameProduct: 'Название товара 6',
          description: 'Легкий высококачественный портативный долговечный многофункциональный инновационный компактный надежный экономичный интуитивный',
        },
      ]
    }
  },
  methods: {
    replaceNumber($event, id) {
      $event.target.value = $event.target.value.replace(/[^0-9]/g, "");
      localStorage.setItem(`item_${id}`, $event.target.value)
    },
    getCurrentCount(id) {
      return localStorage.getItem(`item_${id}`) ? localStorage.getItem(`item_${id}`) : 1      
    },
    incrementItem(id) {
      let currentCount = localStorage.getItem(`item_${id}`) ? localStorage.getItem(`item_${id}`) : 1
      let inputItem = this.$refs[`item_count_${id}`]
      currentCount++;      
      localStorage.setItem(`item_${id}`, currentCount)
      inputItem[0].value = currentCount
      if(this.viewItem) this.$refs[`item_viewing_count_${id}`].value = currentCount
    },
    decrementItem(id) {
      let currentCount = localStorage.getItem(`item_${id}`) ? localStorage.getItem(`item_${id}`) : 1
      let inputItem = this.$refs[`item_count_${id}`]
      currentCount--;      
      if(currentCount>1) localStorage.setItem(`item_${id}`, currentCount)
      else localStorage.removeItem(`item_${id}`)
      inputItem[0].value = currentCount
      if(this.viewItem) this.$refs[`item_viewing_count_${id}`].value = currentCount
    },
    viewing(item) {
      this.viewItem = item
    },
    closeViewingItem() {
      this.viewItem = null
    },
    addToCart(item) {
      let itemCount = localStorage.getItem(`item_${item.id}`) ? localStorage.getItem(`item_${item.id}`) : 1
      alert(`Товар "${item.nameProduct}" в количестве ${itemCount} добавлен в корзину`)
      localStorage.removeItem(`item_${item.id}`)
      this.viewItem = null
    }
  },
  mounted() {}
}
</script>

<style lang="scss">
:root {
  --card-shadow: rgba(0, 0, 0, .2);
}
* {
  box-sizing: border-box;
}
[hidden] {
  display: none;
}
body {
  margin: 0;
  font-family: Arial, sans-serif;
  font-size: 1rem;
}
input {
  outline: none;
}
.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 1rem;
  white-space: nowrap;
}
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
}
.cart-count {
  width: 1.4rem;
  height: 1.4rem;
  border-radius: 50%;
  background: red;
  color: white;
  font-size: .8rem;
  display: grid;
  place-items: center;
  &:empty {
    display: none;
  }
}
.catalog {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 2rem;
  @media(max-width: 1023px) {
    grid-template-columns: 1fr 1fr;
  }
  @media(max-width: 575px) {
    grid-template-columns: 1fr;
  }
}
.product-card {
  padding: .5rem;
  border-radius: .8rem;
  box-shadow: 0 0 10px 0 var(--card-shadow);
  display: flex;
  flex-direction: column;
  gap: 1rem;
  &__cover {
    border-radius: .8rem;
    overflow: hidden;
  }
  &__image {
    max-width: 100%;
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: .8rem;
  }
  &__link {
    display: flex;
  }
  &__add-to-cart {
    width: 100%;
    background: coral;
    color: white;
    border: none;
    padding: .8rem;
    border-radius: .8rem;
    cursor: pointer;
  }
}

.modal-product {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  max-width: 600px;
  padding: 30px;
  background-color: rgba(255,255,255,1);
  border-radius: 8px;
  z-index: 999;
  @media(max-width: 1023px) {
    max-width: 70%;
  }
  @media(max-width: 575px) {
    max-width: 98%;
  }
  &__backdrop {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0,0,0,.6);
    z-index: 998;
  }
  &__close {
    display: block;
    position: absolute;
    width: 20px;
    height: 20px;
    right: 15px;
    top: 15px;
    cursor: pointer;
    transform: rotate(45deg);
    &::after, &::before {
      content: '';
      display: block;
      width: 100%;
      height: 2px;
      background-color: rgba(0,0,0,1);
      position: absolute;
      top: 50%;
      left: 0;
      transform: translateY(-50%);
    }
    &:before {
      transform: rotate(90deg);
    }
  }
  .counter {
    margin: 1em 0;
  }
  p {
    white-space: normal;
  }
}

</style>
