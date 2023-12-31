<script setup>
import { ref, computed } from "vue";
import Alerta from "./Alerta.vue";
import cerrarModal from "../assets/img/cerrar.svg";

const error = ref("");

const emit = defineEmits([
  "ocultar-modal",
  "guardar-gasto",
  "update:nombre",
  "update:cantidad",
  "update:categoria",
  "eliminar-gasto",
]);
const props = defineProps({
  modal: {
    type: Object,
    require: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  cantidad: {
    type: [String, Number],
    required: true,
  },
  categoria: {
    type: String,
    required: true,
  },
  disponible: {
    type: Number,
    required: true,
  },
  id: {
    type: [String, null],
    required: true,
  },
});

const old = props.cantidad;

const agregarGasto = () => {
  // Validar que no haya campos vacios
  const { nombre, cantidad, categoria, disponible, id } = props;

  if ([nombre, cantidad, categoria].includes("")) {
    error.value = "Todos los campos son obligatorios";

    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }

  // Validar la cantidad
  if (cantidad <= 0) {
    error.value = "Cantidad no válida";

    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }

  // Validar que el usuario no gaste más de lo disponible
  if (id) {
    // Tomar en cuenta el gasto ya realizado
    if (cantidad > old + disponible) {
      error.value = "Has excedido el presupuesto";

      setTimeout(() => {
        error.value = "";
      }, 3000);
      return;
    }
  } else {
    if (cantidad > disponible) {
      error.value = "Has excedido el presupuesto";

      setTimeout(() => {
        error.value = "";
      }, 3000);
      return;
    }
  }

  emit("guardar-gasto");
};

const isEditing = computed(() => {
  return props.id;
});
</script>

<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img :src="cerrarModal" alt="cerrar" @click="$emit('ocultar-modal')" />
    </div>

    <div
      class="contenedor contenedor-formulario"
      :class="[modal.animar ? 'animar' : 'cerrar']"
    >
      <form class="nuevo-gasto" @submit.prevent="agregarGasto">
        <legend>{{ isEditing ? "Guardar Cambios" : "Añadir Gastos" }}</legend>
        <Alerta v-if="error"> {{ error }}</Alerta>

        <div class="campo">
          <label for="nombre">Nombre Gasto:</label>
          <input
            type="text"
            id="nombre"
            placeholder="Añade el nombre del gasto"
            :value="nombre"
            @input="$emit('update:nombre', $event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="cantidad">Cantidad:</label>
          <input
            type="number"
            id="cantidad"
            placeholder="Añade la cantidad del gasto, ej. 300"
            :value="cantidad"
            @input="$emit('update:cantidad', +$event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="categoria">Categoría:</label>
          <select
            id="categoria"
            :value="categoria"
            @input="$emit('update:categoria', $event.target.value)"
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
        <input
          type="submit"
          :value="[isEditing ? 'Guardar Cambios' : 'Añadir Gasto']"
        />
      </form>
      <div class="pt-1">
              <button
        type="button"
        class="btn-eliminar"
        v-if="isEditing"
        @click="$emit('eliminar-gasto')"
      >
        Eliminar
      </button>
      </div>

    </div>
  </div>
</template>

<style scoped>
/* Estilos generales */
.modal {
  position: absolute;
  background-color: rgb(0 0 0 / 0.9);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.cerrar-modal {
  position: absolute;
  right: 1rem; 
  top: 1rem;
}
.cerrar-modal img {
  width: 2rem;
  cursor: pointer;
}

/* Estilos para el formulario */
.contenedor-formulario {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}
.contenedor-formulario.animar {
  opacity: 1;
}
.contenedor-formulario.cerrar {
  opacity: 0;
}

/* Estilos para el formulario de nuevo gasto */
.nuevo-gasto {
  margin: 1rem auto 0 auto; 
  display: grid;
  gap: 1rem;
}
.nuevo-gasto legend {
  text-align: center;
  color: var(--blanco);
  font-size: 1.6rem;
  font-weight: 700;
}
.campo {
  display: grid;
  gap: 1rem;
}
.nuevo-gasto input,
.nuevo-gasto select {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 1.8rem;
}
.nuevo-gasto label {
  color: var(--blanco);
  font-size: 1.4rem;
}
.nuevo-gasto input[type="submit"] {
  background-color: var(--azul);
  color: var(--blanco);
  font-weight: 700;
  cursor: pointer;
}

/* Estilos para el botón de eliminar */
.btn-eliminar {
  background-color: #ef4444;
  color: var(--blanco);
  font-weight: 700;
  cursor: pointer;
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 1.8rem;
  width: 100%;
}

.pt-1 {
  padding-top: 1rem;
}

/* Media Query para dispositivos de pantalla más pequeños*/
@media (max-width: 375px) {
  .modal {
    position: fixed; /* Cambiar a posición fija en pantallas pequeñas */
  }

  .cerrar-modal {
    right: 0.5rem; /* Ajustar el espacio desde la derecha */
    top: 0.5rem; /* Ajustar el espacio desde la parte superior */
  }

  .cerrar-modal img {
    width: 1.5rem; /* Reducir el tamaño del icono de cierre */
  }

  .nuevo-gasto {
    margin-top: 1rem; /* Ajustar el margen */
  }

  .nuevo-gasto legend {
    font-size: 1.4rem; /* Reducir el tamaño de la fuente */
  }

  .nuevo-gasto input,
  .nuevo-gasto select {
    font-size: 1.6rem; /* Reducir el tamaño de la fuente */
  }

  .nuevo-gasto label {
    font-size: 1.2rem; /* Reducir el tamaño de la fuente */
  }

  .btn-eliminar {
    margin-top: 0.5rem; /* Ajustar el margen */
  }
}


</style>
