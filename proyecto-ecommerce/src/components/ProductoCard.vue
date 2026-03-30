<template>
  <div class="col-md-4 mb-4">
    <div class="coffee-card h-100 shadow-sm">
      <img :src="imagen" class="card-img-top coffee-card__img" :alt="nombre">
      <div class="coffee-card__body d-flex flex-column">
        <h5 class="coffee-card__title">{{ nombre }}</h5>
        <p class="coffee-card__subtitle">{{ descripcion }}</p>
        <p class="coffee-card__price">${{ precio }}</p>
        <button 
          @click="$emit('add-to-cart')" 
          class="btn coffee-card__button mt-auto w-100"
        >
          Agregar al Carrito
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['nombre', 'precio', 'imagen', 'descripcion']);
defineEmits(['add-to-cart']);
</script>

<style scoped>

.coffee-card {
  border: none;
  border-radius: 15px;
  background-color: #F3E9D7;
  transition: transform 0.3s ease;
}

.coffee-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(59, 42, 34, 0.2);
}

.coffee-card__img {
  border-radius: 15px 15px 0 0;
  object-fit: cover;
  height: 250px;
}

.coffee-card__body {
  padding: 1.5rem;
  text-align: center;
}

.coffee-card__title {
  color: #3B2A22;
  font-weight: 700;
  font-family: 'Playfair Display', serif; 
}

.coffee-card__subtitle {
  color: #7A553A;
  font-size: 0.9rem;
}

.coffee-card__price {
  color: #7A553A;
  font-size: 1.25rem;
  font-weight: bold;
}

.coffee-card__button {
  background-color: #B08968;
  color: white;
  border: none;
  border-radius: 25px;
  padding: 8px 25px;
}

.coffee-card__button:hover {
  background-color: #7A553A;
  color: #F3E9D7;
}

</style>