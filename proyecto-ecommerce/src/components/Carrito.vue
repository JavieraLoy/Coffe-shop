<template>
  <div class="carrito card shadow-sm">
    <div class="carrito__header card-body border-bottom">
      <h5 class="carrito__title mb-0">Tu Carrito</h5>
    </div>
    <div v-if="mensaje" class="alert alert-warning py-2">
      {{ mensaje }}
    </div> 
    <div class="carrito__body card-body">
      <div v-if="items.length === 0" class="text-center text-muted">
        No hay productos aún en tu carrito.
      </div>
      <div v-else>
        <div
          v-for="(item, index) in items"
          :key="index"
          class="carrito__item d-flex justify-content-between align-items-center mb-3"
        >
          <div>
            <h6 class="mb-0">{{ item.nombre }}</h6>
            <small class="text-muted">
              ${{ item.precio }} x {{ item.cantidad }}
            </small>
          </div>
          <div class="d-flex align-items-center gap-2">
            <button 
              class="btn btn-sm btn-outline-secondary"
              @click="$emit('decrease-item', index)"
            >
              -
            </button>
            <span>{{ item.cantidad }}</span>
            <button 
              class="btn btn-sm btn-outline-secondary"
              @click="$emit('increase-item', index)"
            >
              +
            </button>
            <button 
              class="btn btn-sm btn-outline-danger"
              @click="$emit('remove-item', index)"
            >
              ✕
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="carrito__footer card-body border-top d-flex justify-content-between">
      <strong>Total:</strong>
      <span class="carrito__total">${{ total }}</span>
    </div>  
  </div>
</template>

<script setup>
defineProps(['items', 'total', 'mensaje']);
defineEmits(['remove-item', 'increase-item', 'decrease-item']);
</script>

<style scoped>
.carrito {
  border-radius: 15px;
}

.carrito__title {
  color: #3B2A22;
  font-weight: 700;
}

.carrito__item h6 {
  color: #3B2A22;
}

.carrito__total {
  color: #a67c52;
  font-weight: bold;
}
</style>
