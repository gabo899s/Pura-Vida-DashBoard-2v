<template>
  <div class="visual-builder">
    <header class="builder-header">
      <h1>🧱 Constructor Pura Vida RP</h1>
      <button @click="saveWorkflow" class="save-btn">💾 Guardar</button>
    </header>

    <div class="builder-content">
      <div class="blocks-palette">
        <h3>📦 Bloques</h3>
        <input v-model="searchTerm" placeholder="🔍 Buscar..." class="search-input">
        <div class="blocks-list">
          <div
            v-for="block in blocks"
            :key="block.type"
            class="block-item"
            draggable="true"
            @dragstart="onDragStart(block)"
          >
            <div class="block-icon">{{ block.icon }}</div>
            <div class="block-info">
              <div class="block-name">{{ block.name }}</div>
              <div class="block-desc">{{ block.description }}</div>
            </div>
          </div>
        </div>
      </div>

      <div class="workspace" @dragover.prevent @drop="onDrop">
        <div
          v-for="block in workflow"
          :key="block.id"
          class="placed-block"
          :style="{ left: block.x + 'px', top: block.y + 'px' }"
        >
          <div class="block-header">
            {{ block.icon }} {{ block.name }}
          </div>
          <div class="block-content">
            {{ block.description }}
          </div>
        </div>

        <div v-if="workflow.length === 0" class="empty-workspace">
          👆 Arrastra bloques aquí
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'VisualBuilder',
  setup() {
    const searchTerm = ref('')
    const workflow = ref([])
    const draggedBlock = ref(null)

    const blocks = ref([
      { type: 'button', name: 'Botón', icon: '🖱️', description: 'Botón clickeable' },
      { type: 'page', name: 'Página', icon: '📄', description: 'Navegar a página' },
      { type: 'modal', name: 'Modal', icon: '🪟', description: 'Ventana emergente' },
      { type: 'pdf', name: 'PDF', icon: '📄', description: 'Generar documento' }
    ])

    const onDragStart = (block) => {
      draggedBlock.value = block
    }

    const onDrop = (event) => {
      if (!draggedBlock.value) return

      const rect = event.currentTarget.getBoundingClientRect()
      const x = event.clientX - rect.left
      const y = event.clientY - rect.top

      workflow.value.push({
        id: Date.now().toString(),
        ...draggedBlock.value,
        x: x,
        y: y
      })

      draggedBlock.value = null
    }

    const saveWorkflow = () => {
      alert('✅ Workflow guardado!')
      console.log('Workflow:', workflow.value)
    }

    return {
      searchTerm,
      workflow,
      blocks,
      onDragStart,
      onDrop,
      saveWorkflow
    }
  }
}
</script>

<style scoped>
.visual-builder {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background: #F6EFD4;
}

.builder-header {
  background: #A3D2CA;
  color: white;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.save-btn {
  background: #BFD8B8;
  color: #444B54;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
  font-weight: bold;
}

.builder-content {
  flex: 1;
  display: flex;
  overflow: hidden;
}

.blocks-palette {
  width: 300px;
  background: white;
  padding: 1rem;
  border-right: 2px solid #CEDDE0;
}

.search-input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #CEDDE0;
  border-radius: 0.5rem;
  margin-bottom: 1rem;
}

.blocks-list {
  overflow-y: auto;
}

.block-item {
  background: #F9F9F9;
  border: 1px solid #CEDDE0;
  border-radius: 0.5rem;
  padding: 1rem;
  margin-bottom: 0.5rem;
  cursor: grab;
  display: flex;
  align-items: center;
}

.block-icon {
  font-size: 1.5rem;
  margin-right: 0.75rem;
}

.block-name {
  font-weight: bold;
  color: #444B54;
}

.block-desc {
  font-size: 0.8rem;
  color: #666;
}

.workspace {
  flex: 1;
  background: #E8F4F8;
  position: relative;
  overflow: auto;
}

.placed-block {
  position: absolute;
  background: white;
  border: 2px solid #A3D2CA;
  border-radius: 0.75rem;
  padding: 1rem;
  min-width: 180px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.empty-workspace {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: #666;
  font-size: 1.2rem;
}
</style>
