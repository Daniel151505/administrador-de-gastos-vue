<script setup>
import { ref } from "vue";
import AlertaVue from "./Alerta.vue";

const presupuesto = ref(0);
const error = ref("");
const emit = defineEmits(["definir-presupuesto"]);

const definirPresupuesto = () => {
  if (presupuesto.value <= 0 || presupuesto.value === "") {
    error.value = "Presupuesto no válido";
    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }
  emit("definir-presupuesto", presupuesto.value);
};
</script>

<template>
  <form class="presupuesto" @submit.prevent="definirPresupuesto">
    <AlertaVue v-if="error">
      {{ error }}
    </AlertaVue>
    <div class="campo">
      <label for="nuevo-presupuesto">Definir Presupuesto</label>
      <input
        id="nuevo-presupuesto"
        class="nuevo-presupuesto"
        placeholder="Añade tu presupuesto"
        type="number"
        min="0"
        v-model.number="presupuesto"
      />
    </div>
    <input type="submit" value="Definir Presupuesto" />
  </form>
</template>

<style scoped>
.presupuesto {
  width: 100%;
  max-width: 400px; /* Limitar el ancho máximo para evitar que sea demasiado ancho en pantallas grandes */
  margin: 0 auto; /* Centrar el contenido horizontalmente */
}
.campo {
  display: grid;
  gap: 2rem;
}
.presupuesto label {
  font-size: 2.2rem;
  text-align: center;
  color: var(--azul);
  width: 100%; /* El label ocupará todo el ancho disponible */
}
.presupuesto input[type="number"] {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2.2rem;
  text-align: center;
  width: 100%; /* El input ocupará todo el ancho disponible */
}
.presupuesto input[type="submit"] {
  background-color: var(--azul);
  border: none;
  padding: 1rem;
  text-align: center;
  font-size: 2rem;
  margin-top: 2rem;
  color: var(--blanco);
  font-weight: 900;
  width: 100%; /* El botón ocupará todo el ancho disponible */
  transition: background-color 300ms ease;
}
.presupuesto input[type="submit"]:hover {
  background-color: #1048a4;
  cursor: pointer;
}
/* Media Query para dispositivos de pantalla más pequeños */
@media screen and (max-width: 768px) {
  .presupuesto label,
  .presupuesto input[type="submit"] {
    font-size: 0.9rem; /* Reducir el tamaño de la fuente en pantallas más pequeñas */
  }

  .presupuesto input[type="number"] {
    font-size: 1.6rem; /* Reducir el tamaño de la fuente en pantallas más pequeñas */
  }
}
</style>
