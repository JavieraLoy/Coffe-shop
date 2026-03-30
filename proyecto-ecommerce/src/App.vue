<template>
  <div class="app-container" id="app">
    <header class="hero d-flex align-items-center text-white">
      <div class="container text-center">
        <div class="hero__content">
          <img :src="HeroImg" alt="Sello marca" class="hero__stamp">
          <h1 class="hero__title">Café con carácter, directo a tu taza</h1>
          <p class="hero__subtitle">Granos seleccionados, tostado artesanal y sabor auténtico.</p>
          <button class="hero-btn btn mt-3" @click="scrollToMenu">
            Ver menú
          </button>
        </div>
      </div>  
    </header>

    <main class="menu" ref="menuSection">
      <div class="menu__header text-center">
        <h2 class="menu__title">Nuestro Café</h2>
        <p class="menu__description">
          Selección de granos tostados artesanalmente, preparados para cada momento del día.
        </p>
      </div>
      <div class="menu__content row g-4"> 
        <div class="menu__products col-12 col-lg-8">
          <div class="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-4">
            <ProductoCard 
              v-for="p in stock" :key="p.id"
              v-bind="p"
              @add-to-cart="agregarAlCarrito(p)"
            />
          </div>
        </div>
        <div class="menu__cart col-12 col-lg-4">
          <aside class="menu__cart-sticky">
            <Carrito 
              :items="carrito" 
              :total="totalPagar"
              :mensaje="mensaje" 
              @remove-item="eliminarDelCarrito"
              @increase-item="aumentarCantidad"
              @decrease-item="disminuirCantidad" />
          </aside>
        </div>

      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import ProductoCard from './components/ProductoCard.vue';
import Carrito from './components/Carrito.vue';
import espressoImg from './assets/img/espresso.jpg';
import capuchinoImg from './assets/img/capuchino.jpg';
import latteImg from './assets/img/latte.jpg';
import mochaccinoImg from './assets/img/mochaccino.jpg';
import matchaImg from './assets/img/matchalatte.jpg';
import americanoImg from './assets/img/americano.jpg';
import HeroImg from './assets/img/brand-image.png';

const menuSection= ref(null);
const scrollToMenu= ()=> {
  menuSection.value?.scrollIntoView({ behavior: 'smooth'});
};
const stock = ref([
  { id: 1, nombre: 'Espresso', precio: 2500, imagen: espressoImg, descripcion: 'Un clásico intenso y aromático, perfecto para quienes disfrutan el café en su forma más pura.'},
  { id: 2, nombre: 'Capuchino', precio: 3000, imagen: capuchinoImg, descripcion: 'Espuma suave y cuerpo equilibrado, con el toque justo de intensidad y cremosidad.' },
  { id: 3, nombre: 'Latte', precio: 3400, imagen: latteImg, descripcion: 'Suave y cremoso, el equilibrio perfecto entre café y leche para disfrutar en cualquier momento.'},
  { id: 4, nombre: 'Mochaccino', precio: 3500, imagen: mochaccinoImg, descripcion: 'La combinación perfecta de café y chocolate, con un sabor dulce y envolvente.'},
  { id: 5, nombre: 'Matcha Latte', precio: 3500, imagen: matchaImg, descripcion: 'Energía natural en cada sorbo, con un sabor herbal y una textura delicadamente cremosa.'},
  { id: 6, nombre: 'Americano', precio: 2900, imagen: americanoImg, descripcion: 'Aroma delicado y cuerpo suave, pensado para quienes prefieren un café más sutil.'}
]);

const carrito = ref([]);
const mensaje= ref('');

const agregarAlCarrito = (producto) => {
  const itemEnCarrito = carrito.value.find(item => item.id === producto.id);
  
  if (itemEnCarrito) {
    if (itemEnCarrito.cantidad < 10) {
      itemEnCarrito.cantidad++;
      mensaje.value= '';
    } else {
      mensaje.value= "Máximo 10 unidades por café";
    }
  } else {
    carrito.value.push({ ...producto, cantidad: 1 });
    mensaje.value= '';
  }
};

const eliminarDelCarrito = (index) => {
  carrito.value.splice(index, 1);
  mensaje.value= '';
};

const totalPagar = computed(() => {
  return carrito.value.reduce((acc, item) => acc + (item.precio * item.cantidad), 0);
});

const aumentarCantidad = (index) => {
  const item = carrito.value[index];

  if (item.cantidad < 10) {
    item.cantidad++;
    mensaje.value = '';
  } else {
    mensaje.value = "Máximo 10 unidades por café";
  }
};

const disminuirCantidad = (index) => {
  mensaje.value= '';
  if (carrito.value[index].cantidad > 1) {
    carrito.value[index].cantidad--;
  } else {
    carrito.value.splice(index, 1);
  }
};
</script>

<style>
body, html{
  margin: 0;
  padding: 0;
  width: 100%;
  overflow-x: hidden; 
  background-color: #fdfaf5; 
  font-family: 'Segoe UI', Tahoma, Verdana, sans-serif;
}
#app{
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.custom-container {
  width: 95%;     
  max-width: 1400px; 
  margin-top: 50px;  
  padding: 2rem ;
}
.hero {
  height: 70vh;
  background-image: url('./assets/img/hero-cafe.jpg'); 
  background-size: cover;
  background-position: center;
  position: relative;
  display: flex;
  justify-content: center;
}

.hero::before {
  content: "";
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.55);
}

.hero .container {
  position: relative;
  z-index: 2;
}

.hero__content {
  position: relative;
  display: inline-block;
}

.hero__stamp {
  position: absolute;
  width: 200px;
  opacity: 0.7; 
  top: -150px;
  left: 50%;
  transform: translateX(-50%);
  pointer-events: none; 
}

.hero__title {
  margin-top: 20px;
  font-size: 3rem;
  font-weight: 800;
  letter-spacing: 2px;
}

.hero__subtitle {
  font-size: 1.3rem;
  color: #e0d3c2;
}

.hero-btn {
  background-color: #F3E9D7;
  color: #3B2A22;
  border: none;
  padding: 12px 28px;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 30px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.hero-btn:hover {
  background-color: #e0d3c2;
  transform: scale(1.05);
}

.hero-btn:active {
  transform: scale(0.98);
}

.menu {
  width: 95%;
  max-width: 1400px;
  margin: 40px;
  padding: 3rem 2rem;
  background-color: #fdfaf5;
  border-radius: 20px;
}

.menu__header {
  margin-bottom: 2.5rem;
}

.menu__title {
  font-size: 2rem;
  font-weight: 800;
  color: #3B2A22;
}

.menu__description {
  color: #7a6a5a;
  max-width: 500px;
  margin: 0.5rem auto 0;
}

.menu__content {
  align-items: flex-start;
}

.menu__cart-sticky {
  position: sticky;
  top: 20px;
}
</style>



