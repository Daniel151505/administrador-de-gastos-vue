<script setup>
defineProps(["filtro"]);
defineEmits(["update:filtro"]);
</script>

<template>
  <div class="filtros sombra contenedor">
    <form action="">
      <div class="campo">
        <label for="filtro">Filtrar Gastos</label>
        <select
          id="filtro"
          :value="filtro"
          @input="$emit('update:filtro', $event.target.value)"
        >
          <option value="">--Seleccione--</option>
          <option value="ahorro">Ahorro</option>
          <option value="comida">Comida</option>
          <option value="casa">Casa</option>
          <option value="ocio">Ocio</option>
          <option value="salud">Salud</option>
          <option value="suscripciones">Suscripciones</option>
        </select>
      </div>
    </form>
  </div>
</template>

<style scoped>
/* Estilos generales */
.filtros {
  margin-top: 10rem;
}

.filtros .campo {
  display: flex;
  flex-wrap: wrap; /* Permite que los elementos se ajusten en filas en pantallas pequeñas */
  align-items: center;
  gap: 2rem;
  text-align: center;
}

.filtros label {
  font-size: 3rem;
  font-weight: 900;
  color: var(--gris-oscuro);
}

.filtros select {
  flex: 1;
  padding: 1rem;
  border: none;
  border-radius: 1rem;
  background-color: var(--gris-claro);
  text-align: center;
}

/* Media Query para dispositivos de pantalla más pequeños */
@media (max-width: 768px) {
  .filtros .campo {
    flex-direction: column; /* Cambia la dirección de flexión en pantallas pequeñas */
  }

  .filtros select {
    width: 100%; /* Ocupa todo el ancho disponible en pantallas pequeñas */
    margin-top: 1rem; /* Añade un margen superior para separar los elementos */
  }

  .filtros label {
    font-size: 2rem; /* Reducir el tamaño de la fuente en pantallas pequeñas */
  }
}
</style>
