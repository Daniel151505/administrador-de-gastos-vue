<script setup>
import { computed } from "vue";
import "vue3-circle-progress/dist/circle-progress.css";
import CircleProgress from "vue3-circle-progress";
import { formatearCantidad } from "../helpers";

defineEmits(["reset-app"]);

const props = defineProps({
  presupuesto: {
    type: Number,
    required: true,
  },
  disponible: {
    type: Number,
    required: true,
  },
  gastado: {
    type: Number,
    required: true,
  },
});

const porcentaje = computed(() => {
  return parseInt(
    ((props.presupuesto - props.disponible) / props.presupuesto) * 100
  );
});
</script>

<template>
  <div class="dos-columnas">
    <div class="contenedor-grafico">
      <p class="porcentaje">{{ porcentaje }}%</p>
      <div class="circle">
        <CircleProgress
          :percent="porcentaje"
          :size="170"
          :border-width="20"
          :border-bg-width="20"
          fill-color="#3b82f6"
          empty-color="#e1e1e1"
        />
      </div>
    </div>
    <div class="contenedor-presupuesto">
      <button class="reset-app" type="button" @click="$emit('reset-app')">
        Resetear App
      </button>
      <p>
        <span>Presupuesto:</span>
        {{ formatearCantidad(presupuesto) }}
      </p>
      <p>
        <span>Disponible:</span>
        {{ formatearCantidad(disponible) }}
      </p>
      <p>
        <span>Gastado:</span>
        {{ formatearCantidad(gastado) }}
      </p>
    </div>
  </div>
</template>

<style scoped>
/* Estilos generales */
.dos-columnas {
  display: flex;
  flex-direction: column;
}

.dos-columnas > :first-child {
  margin-bottom: 3rem;
}

.contenedor-presupuesto {
  width: 100%;
  text-align: center;
}

.contenedor-presupuesto p {
  font-size: 2.4rem;
  color: var(--gris-oscuro);
}

.contenedor-presupuesto span {
  font-weight: 900;
  color: var(--azul);
}

.reset-app {
  background-color: #db2777;
  border: none;
  padding: 1rem;
  width: 100%;
  color: var(--blanco);
  font-weight: 900;
  text-transform: uppercase;
  border-radius: 1rem;
  transition-property: background-color;
  transition-duration: 300ms;
}

.reset-app:hover {
  cursor: pointer;
  background-color: #c11d67;
}

.contenedor-grafico {
  position: relative;
  align-items: center;
  text-align: center;
}

.porcentaje {
  position: absolute;
  margin: auto;
  top: calc(50% - 1.5rem);
  left: 0;
  right: 0;
  text-align: center;
  z-index: 100;
  font-size: 3rem;
  font-weight: 900;
  color: var(--gris-oscuro);
}

.circle {
  text-align: center;
  align-items: center;
  display: inline-block;
}

/* Media Query para dispositivos de pantalla más grandes (por ejemplo, tabletas y escritorios) */
@media (min-width: 768px) {
  .dos-columnas {
    flex-direction: row;
    gap: 4rem;
    align-items: center;
  }

  .dos-columnas > :first-child {
    margin-bottom: 0;
  }

  .contenedor-presupuesto p {
    font-size: 2.4rem;
    text-align: left;
  }
}
</style>
