<script lang="ts" setup>
import { computed, toRef } from 'vue'
import type { TeleportProps } from 'vue'
import { useEdge, useVueFlow } from '../../composables'
import { getEdgeZIndex } from '../../utils'

const { viewportRef, findNode, elevateEdgesOnSelect } = useVueFlow()
const { edge } = useEdge()

const teleportTarget = toRef(() => viewportRef.value?.getElementsByClassName('vue-flow__edge-labels')[0] as TeleportProps['to'])

// labels teleport out of the edge svg; same z as getEdgeZIndex so they ride the edge's stacking band
const labelLayerStyle = computed(() => ({
  position: 'absolute' as const,
  inset: 0,
  pointerEvents: 'none' as const,
  zIndex: getEdgeZIndex(edge, findNode, elevateEdgesOnSelect.value),
}))
</script>

<script lang="ts">
export default {
  name: 'EdgeLabelRenderer',
  compatConfig: { MODE: 3 },
}
</script>

<template>
  <svg>
    <foreignObject height="0" width="0">
      <Teleport :to="teleportTarget" :disabled="!teleportTarget">
        <div :style="labelLayerStyle">
          <slot />
        </div>
      </Teleport>
    </foreignObject>
  </svg>
</template>
