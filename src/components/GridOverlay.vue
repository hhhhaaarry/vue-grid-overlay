<template>
  <div v-if="isGridVisible" class="grid-overlay">
    <div class="grid-overlay__inner">
      <!-- Contenedor para las filas -->
      <div class="grid-rows" :style="rowsStyle">
        <!-- Aquí se usa rowHeightComputed para definir la altura de cada fila -->
        <template v-if="effectiveRowCount > 0">
          <div
            v-for="row in effectiveRowCount"
            :key="`row-${row}`"
            class="grid-row"
            :style="{ ...rowStyle, height: rowHeightComputed }"
          ></div>
        </template>
      </div>
      <!-- Contenedor para las columnas -->
      <div class="grid-columns" :style="columnsStyle">
        <div
          v-for="column in columnCount"
          :key="`col-${column}`"
          class="grid-column"
          :style="columnStyle"
        ></div>
      </div>
      <div class="grid-baseline" :style="baselinStyle" v-if="baseline">
        <div
          v-for="baseline in baselineCount"
          :key="`baseline-${baseline}`"
          :style="baselineStyle"
        ></div>
      </div>
      <div class="grid-controls" v-if="isControlsVisible">
        <div class="grid-controls__header">
          <div class="grid-controls__selector">
            <select v-model="selectedInput">
              <option
                v-for="(input, index) in inputs"
                :key="`input-${index}`"
                :value="input.value"
              >
                {{ input.text }}
              </option>
            </select>
          </div>
          <div class="grid-controls__toggle">
            <button @click="toggleControls">
              <svg
                width="16"
                height="16"
                viewBox="0 0 16 16"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M3.64623 3.64669C3.73998 3.55305 3.86706 3.50046 3.99956 3.50046C4.13206 3.50046 4.25914 3.55305 4.35289 3.64669L7.99956 7.29335L11.6462 3.64669C11.692 3.59756 11.7472 3.55816 11.8085 3.53083C11.8699 3.50351 11.9361 3.48881 12.0032 3.48763C12.0703 3.48644 12.137 3.49879 12.1993 3.52394C12.2616 3.54909 12.3181 3.58652 12.3656 3.634C12.4131 3.68147 12.4505 3.73803 12.4756 3.80029C12.5008 3.86255 12.5131 3.92923 12.512 3.99637C12.5108 4.0635 12.4961 4.12971 12.4687 4.19105C12.4414 4.25238 12.402 4.30758 12.3529 4.35335L8.70623 8.00002L12.3529 11.6467C12.402 11.6925 12.4414 11.7477 12.4687 11.809C12.4961 11.8703 12.5108 11.9365 12.512 12.0037C12.5131 12.0708 12.5008 12.1375 12.4756 12.1998C12.4505 12.262 12.4131 12.3186 12.3656 12.366C12.3181 12.4135 12.2616 12.451 12.1993 12.4761C12.137 12.5013 12.0703 12.5136 12.0032 12.5124C11.9361 12.5112 11.8699 12.4965 11.8085 12.4692C11.7472 12.4419 11.692 12.4025 11.6462 12.3534L7.99956 8.70669L4.35289 12.3534C4.25811 12.4417 4.13275 12.4898 4.00321 12.4875C3.87368 12.4852 3.75009 12.4327 3.65848 12.3411C3.56687 12.2495 3.5144 12.1259 3.51211 11.9964C3.50982 11.8668 3.55791 11.7415 3.64623 11.6467L7.29289 8.00002L3.64623 4.35335C3.55259 4.2596 3.5 4.13252 3.5 4.00002C3.5 3.86752 3.55259 3.74044 3.64623 3.64669Z"
                  fill="#F6F6F6"
                />
              </svg>
            </button>
          </div>
        </div>
        <!-- Rows -->
        <div class="grid-controls__body rows" v-if="selectedInput === 'rows'">
          <div class="row">
            <div class="control-group">
              <label for="row-count">Count</label>
              <select v-model="rowCount">
                <option value="auto">Auto</option>
                <option :value="0">0</option>
                <option v-for="num in 24" :value="num">{{ num }}</option>
              </select>
            </div>
            <div class="control-group">
              <label for="row-color">Color</label>
              <input type="color" id="row-color" v-model="rowColor" />
            </div>
            <div class="control-group">
              <label for="row-opacity">Opacity</label>
              <div class="input-percentage-wrapper">
                <input
                  type="number"
                  id="row-opacity"
                  min="0"
                  max="100"
                  v-model.number="rowOpacity"
                />
                <span class="percentage-sign">%</span>
              </div>
            </div>
          </div>
          <div class="row">
            <div class="control-group">
              <label for="row-type">Type</label>
              <select id="row-type" v-model="rowType" class="capitalize">
                <option
                  v-for="(type, index) in rowTypes"
                  :key="index"
                  :value="type"
                >
                  {{ type }}
                </option>
              </select>
            </div>
            <div class="control-group">
              <label for="row-height">Height</label>
              <input
                type="text"
                id="row-height"
                v-model="rowHeight"
                :disabled="rowType === 'stretch'"
                class="capitalize"
              />
            </div>
            <div class="control-group">
              <label for="row-offset">Offset</label>
              <input
                type="number"
                id="row-offset"
                min="0"
                max="100"
                v-model.number="rowOffset"
              />
            </div>
          </div>
          <div class="row">
            <div class="control-group">
              <label for="row-gutter">Gutter</label>
              <input
                type="number"
                id="row-gutter"
                min="0"
                max="100"
                v-model.number="rowGutter"
              />
            </div>
          </div>
        </div>
        <!-- Columns -->
        <div
          class="grid-controls__body columns"
          v-if="selectedInput === 'columns'"
        >
          <div class="row">
            <div class="control-group">
              <label for="column-count">Count</label>
              <select v-model="columnCount">
                <option value="auto">Auto</option>
                <option :value="0">0</option>
                <option v-for="num in 24" :value="num">{{ num }}</option>
              </select>
            </div>
            <div class="control-group">
              <label for="column-color">Color</label>
              <input type="color" id="column-color" v-model="columnColor" />
            </div>
            <div class="control-group">
              <label for="column-opacity">Opacity</label>
              <div class="input-percentage-wrapper">
                <input
                  type="number"
                  id="column-opacity"
                  min="0"
                  max="100"
                  v-model.number="columnOpacity"
                />
                <span class="percentage-sign">%</span>
              </div>
            </div>
          </div>
          <div class="row">
            <div class="control-group">
              <label for="column-type">Type</label>
              <select id="column-type" v-model="columnType" class="capitalize">
                <option
                  v-for="(type, index) in columnTypes"
                  :key="index"
                  :value="type"
                >
                  {{ type }}
                </option>
              </select>
            </div>
            <div class="control-group">
              <label for="column-width">Width</label>
              <input
                type="text"
                id="column-width"
                v-model="columnWidth"
                :disabled="columnType === 'stretch'"
                class="capitalize"
              />
            </div>
            <div class="control-group">
              <label for="column-margin">Margin</label>
              <input
                type="number"
                id="column-margin"
                min="0"
                max="100"
                v-model.number="columnMargin"
              />
            </div>
          </div>
          <div class="row">
            <div class="control-group">
              <label for="column-gutter">Gutter</label>
              <input
                type="number"
                id="column-gutter"
                min="0"
                max="100"
                v-model.number="columnGutter"
              />
            </div>
          </div>
        </div>
        <div
          class="grid-controls__body baseline"
          v-if="selectedInput === 'baseline'"
        >
          <div class="row">
            <div class="control-group">
              <label for="baseline">Baseline</label>
              <div class="basline-px-wrapper">
                <input
                  type="number"
                  id="baseline"
                  min="0"
                  max="100"
                  v-model.number="baseline"
                />
                <span class="px-units">px</span>
              </div>
            </div>
            <div class="control-group">
              <label for="baseline-color">Color</label>
              <input type="color" id="baseline-color" v-model="baselineColor" />
            </div>
            <div class="control-group">
              <label for="baseline-opacity">Opacity</label>
              <div class="input-percentage-wrapper">
                <input
                  type="number"
                  id="baseline-opacity"
                  min="0"
                  max="100"
                  v-model.number="baselineOpacity"
                />
                <span class="percentage-sign">%</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, watch, onMounted, onUnmounted } from "vue";

// Verifica si está en el cliente de manera segura para ambos, Nuxt y Vue puro
const isClient = typeof window !== "undefined";

const columnCount = ref(12);
const rowCount = ref(0);
const columnColor = ref("#FF0000");
const rowColor = ref("#FF0000");
const columnOpacity = ref(10);
const rowOpacity = ref(10);
const columnWidth = ref("auto");
const rowHeight = ref("auto");
const columnMargin = ref(0);
const rowOffset = ref(0);
const columnGutter = ref(8);
const rowGutter = ref(8);
const columnType = ref("stretch");
const rowType = ref("stretch");
const baselineColor = ref("#FF0000");
const baselineOpacity = ref(10);
const baseline = ref(0);

const inputs = [
  { text: "Columns", value: "columns" },
  { text: "Rows", value: "rows" },
  { text: "Baseline", value: "baseline" },
];
const selectedInput = ref(inputs[0].value); // Inicializa con el valor por defecto o el primer valor de tus inputs

const rowTypes = ["top", "center", "bottom", "stretch"];

const columnTypes = ["left", "center", "right", "stretch"];

const isControlsVisible = ref(true); // Controla la visibilidad de los controles
const toggleControls = () => {
  isControlsVisible.value = !isControlsVisible.value;
};
const isGridVisible = ref(false); // Controla la visibilidad de la retícula

let gridState = ref(0); // 0: Oculto, 1: Visible, 2: Visible con opacidad al 50%

function toggleGridVisibility(event) {
  if (event.shiftKey && event.key === "G") {
    gridState.value = (gridState.value + 1) % 3;
    isGridVisible.value = gridState.value !== 0;
    isControlsVisible.value = gridState.value !== 0;
  }
}

// Calcula el número efectivo de filas y el espacio restante
const effectiveRowCount = computed(() => {
  // Caso cuando rowCount es un número específico y mayor que cero
  if (typeof rowCount.value === "number" && rowCount.value > 0) {
    return rowCount.value;
  }

  // Caso cuando rowCount es 'auto'
  if (rowCount.value === "auto") {
    const viewportHeight = window.innerHeight - rowOffset.value * 2;
    let rowHeightInt = parseInt(rowHeight.value, 10);

    if (isNaN(rowHeightInt)) {
      rowHeightInt = 50; // Valor predeterminado para la altura de fila si no se puede parsear
    }

    const totalRowsPossible = Math.floor(
      (viewportHeight + rowGutter.value) / (rowHeightInt + rowGutter.value)
    );
    return Math.max(0, totalRowsPossible);
  }
  // Retorna 0 si rowCount.value es 0, null, undefined, o cualquier otro valor que no sea 'auto'
  return 0;
});
const rowHeightComputed = computed(() => {
  console.log("Calculando rowHeightComputed");
  // Caso para 'stretch'
  if (rowType.value === "stretch") {
    const viewportHeight = window.innerHeight - rowOffset.value * 2;
    const totalGutterHeight = rowGutter.value * (effectiveRowCount.value - 1);
    const availableHeight = viewportHeight - totalGutterHeight;
    const heightPerRow = availableHeight / effectiveRowCount.value; // Asegúrate de usar .value aquí
    return `${heightPerRow}px`;
  }
  // Caso para 'top', 'center', 'bottom' con 'auto'
  else if (rowHeight.value === "auto") {
    const viewportHeight = window.innerHeight - rowOffset.value * 2;
    // Si rowCount es 'auto' o no está definido, calcula el número posible de filas basado en un alto estimado
    if (rowCount.value === "auto" || !rowCount.value) {
      const estimatedRowHeight = 50; // Estimación de altura de fila para cálculo
      const totalRowsPossible = Math.floor(
        (viewportHeight + rowGutter.value) /
          (estimatedRowHeight + rowGutter.value)
      );
      const heightPerRow =
        (viewportHeight - rowGutter.value * (totalRowsPossible - 1)) /
        totalRowsPossible;
      return `${heightPerRow}px`;
    } else {
      // Si rowCount.value está definido, divide el espacio igualmente
      const totalGutterHeight = rowGutter.value * (rowCount.value - 1);
      const availableHeight = viewportHeight - totalGutterHeight;
      const heightPerRow = availableHeight / rowCount.value;
      return `${heightPerRow}px`;
    }
  }
  // Caso por defecto: maneja tanto valores numéricos como strings con unidades
  else {
    // Verifica si rowHeight.value es un número
    if (!isNaN(parseFloat(rowHeight.value)) && isFinite(rowHeight.value)) {
      // Si es un número, asume que es un valor en píxeles y agrega 'px'
      return `${rowHeight.value}px`;
    } else {
      // Si no es un número, devuelve el valor tal cual (puede ser 'auto' o un valor con unidades)
      return rowHeight.value;
    }
  }
});

const rowsStyle = computed(() => {
  let justifyContent = "flex-start"; // Default para 'top'
  if (rowType.value === "center") {
    justifyContent = "center";
  } else if (rowType.value === "bottom") {
    justifyContent = "flex-end";
  }

  // Inicializa positionTop y positionBottom para aplicar el offset correctamente
  let positionTop = "0px"; // Valor inicial para top
  let positionBottom = "0px"; // Valor inicial para bottom

  // Ajusta positionTop o positionBottom basado en rowType
  if (rowType.value === "bottom") {
    // Para 'bottom', aplica el offset en la parte inferior
    positionBottom = `${rowOffset.value}px`;
  } else if (rowType.value === "top") {
    // Para 'top' y 'center', aplica el offset en la parte superior
    positionTop = `${rowOffset.value}px`;
  } else {
    // Para 'stretch', aplica el offset en la parte superior y en la parte inferior
    positionTop = `${rowOffset.value}px`;
    positionBottom = `${rowOffset.value}px`;
  }

  return {
    display: "flex",
    flexDirection: "column",
    justifyContent: justifyContent,
    gap: `${rowGutter.value}px`,
    position: "absolute",
    top: 0,
    bottom: 0,
    paddingTop: positionTop,
    paddingBottom: positionBottom,
    paddingLeft: `${columnMargin.value}px`,
    paddingRight: `${columnMargin.value}px`,
  };
});
const rowStyle = computed(() => ({
  height: rowHeightComputed,
  backgroundColor: rowColor.value,
  opacity: gridState.value === 2 ? 0.6 : rowOpacity.value / 100,
}));

const columnsStyle = computed(() => {
  let justifyContent;
  let gridTemplateColumnsValue;

  switch (columnType.value) {
    case "left":
      justifyContent = "start";
      gridTemplateColumnsValue = `repeat(${columnCount.value}, ${columnWidth.value}px)`;
      break;
    case "center":
      justifyContent = "center";
      gridTemplateColumnsValue = `repeat(${columnCount.value}, ${columnWidth.value}px)`;
      break;
    case "right":
      justifyContent = "end";
      gridTemplateColumnsValue = `repeat(${columnCount.value}, ${columnWidth.value}px)`;
      break;
    case "stretch":
    default:
      justifyContent = "stretch";
      // Usa '1fr' para cada columna cuando columnType es 'stretch'
      gridTemplateColumnsValue = `repeat(${columnCount.value}, 1fr)`;
      break;
  }

  const availableHeight = `calc(100% - ${2 * rowOffset.value}px)`;

  return {
    display: "grid",
    gridTemplateColumns: gridTemplateColumnsValue,
    justifyContent: justifyContent,
    columnGap: `${columnGutter.value}px`,
    position: "absolute",
    top: 0,
    bottom: 0,
    left: 0,
    right: 0,
    paddingTop: `${rowOffset.value}px`,
    paddingBottom: `${rowOffset.value}px`,
    paddingLeft: `${columnMargin.value}px`,
    paddingRight: `${columnMargin.value}px`,
    height: availableHeight,
    pointerEvents: "none",
  };
});

const columnStyle = computed(() => ({
  backgroundColor: columnColor.value,
  opacity: gridState.value === 2 ? 0.6 : columnOpacity.value / 100, // 50% para el estado 2, 10% para el estado 1
}));

/* Baseline */

// Calcula el número de líneas de base
const baselineCount = computed(() => {
  const viewportHeight = window.innerHeight;
  const availableHeight = viewportHeight - rowOffset.value;
  if (baseline.value > 0) {
    return Math.ceil(availableHeight / baseline.value);
  }
  return 0;
});

// Estilos para las líneas de base
const baselineStyle = computed(() => ({
  height: "1px", // Cada línea de base tiene 1px de altura
  backgroundColor: baselineColor.value,
  opacity: gridState.value === 2 ? 0.6 : baselineOpacity.value / 100, // 50% para el estado 2, 10% para el estado 1
}));
// Estilos para el contenedor de las líneas de base
const baselinStyle = computed(() => ({
  display: "flex",
  flexDirection: "column",
  gap: `${baseline.value - 1}px`,
  position: "absolute",
  top: `${rowOffset.value}px`, // Solo se aplica el rowOffset al top
  left: "0",
  right: "0",
  bottom: "0", // Se extiende hasta el final del viewport
  pointerEvents: "none",
}));

watch(
  () => rowType.value,
  (newValue) => {
    if (newValue === "stretch") {
      rowHeight.value = "auto";
    }
  }
);
watch(
  () => columnType.value,
  (newValue) => {
    if (newValue === "stretch") {
      columnWidth.value = "auto";
    }
  }
);

// local storage watchers
watch(columnCount, (newValue) => {
  if (isClient) {
    localStorage.setItem("columnCount", newValue);
  }
});

watch(rowCount, (newValue) => {
  if (isClient) {
    localStorage.setItem("rowCount", newValue);
  }
});

watch(columnColor, (newValue) => {
  if (isClient) {
    localStorage.setItem("columnColor", newValue.toString());
  }
});

watch(rowColor, (newValue) => {
  if (isClient) {
    localStorage.setItem("rowColor", newValue.toString());
  }
});

watch(columnOpacity, (newValue) => {
  if (isClient) {
    localStorage.setItem("columnOpacity", newValue);
  }
});

watch(rowOpacity, (newValue) => {
  if (isClient) {
    localStorage.setItem("rowOpacity", newValue);
  }
});

watch(columnWidth, (newValue) => {
  if (isClient) {
    localStorage.setItem("columnWidth", newValue.toString());
  }
});

watch(rowHeight, (newValue) => {
  if (isClient) {
    localStorage.setItem("rowHeight", newValue.toString());
  }
});

watch(columnMargin, (newValue) => {
  if (isClient) {
    localStorage.setItem("columnMargin", newValue);
  }
});

watch(rowGutter, (newValue) => {
  if (isClient) {
    localStorage.setItem("rowGutter", newValue);
  }
});

watch(columnGutter, (newValue) => {
  if (isClient) {
    localStorage.setItem("columnGutter", newValue);
  }
});

watch(columnType, (newValue) => {
  if (isClient) {
    localStorage.setItem("columnType", newValue.toString());
  }
});
watch(rowType, (newValue) => {
  if (isClient) {
    localStorage.setItem("rowType", newValue.toString());
  }
});
watch(baselineColor, (newValue) => {
  if (isClient) {
    localStorage.setItem("baselineColor", newValue.toString());
  }
});

watch(baselineOpacity, (newValue) => {
  if (isClient) {
    localStorage.setItem("baselineOpacity", newValue);
  }
});

watch(baseline, (newValue) => {
  if (isClient) {
    localStorage.setItem("baseline", newValue);
  }
});

onMounted(() => {
  if (isClient) {
    columnCount.value = parseInt(localStorage.getItem("columnCount"), 10) || 12;
    console.log("Valor en localStorage para rowCount:", localStorage.getItem("rowCount"));
    rowCount.value = parseInt(localStorage.getItem("rowCount"), 10) || 0;
    console.log("Valor inicializado para rowCount:", rowCount.value);
    columnColor.value = localStorage.getItem("columnColor") || "#FF0000";
    rowColor.value = localStorage.getItem("rowColor") || "#FF0000";
    columnOpacity.value =
      parseInt(localStorage.getItem("columnOpacity"), 10) || 10;
    rowOpacity.value = parseInt(localStorage.getItem("rowOpacity"), 10) || 10;
    columnWidth.value = localStorage.getItem("columnWidth") || "auto";
    rowHeight.value = localStorage.getItem("rowHeight") || "auto";
    columnMargin.value =
      parseInt(localStorage.getItem("columnMargin"), 10) || 0;
    rowOffset.value = parseInt(localStorage.getItem("rowOffset"), 10) || 0;
    columnGutter.value =
      parseInt(localStorage.getItem("columnGutter"), 10) || 8;
    rowGutter.value = parseInt(localStorage.getItem("rowGutter"), 10) || 8;
    columnType.value = localStorage.getItem("columnType") || "stretch";
    rowType.value = localStorage.getItem("rowType") || "top";
    baselineColor.value = localStorage.getItem("baselineColor") || "#FF0000";
    baselineOpacity.value =
      parseInt(localStorage.getItem("baselineOpacity"), 10) || 10;
    baseline.value = localStorage.getItem("baseline") || null;
    window.addEventListener("keydown", toggleGridVisibility);
  }
});
onUnmounted(() => {
  window.removeEventListener("keydown", toggleGridVisibility);
});
</script>

<style scoped>
.grid-overlay {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 999;
  pointer-events: none;
  font-family: Helvetica, Arial, sans-serif;
  font-size: 16px;
}

.grid-rows {
  height: 100%;
  width: 100%;
  /* Asegúrate de que el contenedor de las filas tenga altura completa */
}

.grid-columns {
  height: 100%;
  /* Asegúrate de que el contenedor de las columnas tenga ancho completo */
}

.grid-overlay__inner {
  position: relative;
  width: 100%;
  height: 100%;
}

.grid-controls {
  pointer-events: auto;
  position: absolute;
  top: 2em;
  right: 2em;
  z-index: 999;
  background-color: #161616;
  min-width: 200px;
  padding: 0.5em;
  color-scheme: light dark;
}

.grid-controls__header {
  display: flex;
  justify-content: space-between;
  padding-bottom: 0.5em;
  border-bottom: 1px solid #333;
}

.grid-controls {
  color: #e6e6e6;
}

.control-group {
  display: flex;
  flex-direction: column;
  text-transform: capitalize;
}

.grid-controls input {
  max-width: 100px;
}

.capitalize {
  text-transform: capitalize;
}

.input-percentage-wrapper,
.basline-px-wrapper {
  display: flex;
  align-items: center;
}

.grid-controls input:disabled {
  opacity: 0.4;
}

.grid-controls .row {
  display: flex;
  width: 100%;
  gap: 1em;
  padding: 1em;
}
</style>
