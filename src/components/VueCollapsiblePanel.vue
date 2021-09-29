<template>
  <section
    ref="panelRef"
    class="vcp"
    :class="{
      'vcp--expanded': isExpanded,
      'vcp--expandable': hasContent,
    }"
  >
    <header
      class="vcp__header"
      @click="toggle"
    >
      <div class="vcp__header-title">
        <slot name="title" />
      </div>
      <div
        v-if="hasContent"
        class="vcp__header-icon"
      >
        <slot name="icon">
          <span v-html="toggleIcon" />
        </slot>
      </div>
    </header>

    <transition
      name="slide"
      @before-enter="collapse"
      @enter="expand"
      @before-leave="expand"
      @leave="collapse"
      @after-enter="setHeightAuto"
      @after-leave="setHeightAuto"
    >
      <div
        v-if="isInitialized"
        v-show="isExpanded"
        ref="bodyRef"
        class="vcp__body"
      >
        <div
          ref="bodyContentRef"
          class="vcp__body-content"
        >
          <slot name="content" />
        </div>
      </div>
    </transition>
  </section>
</template>

<script lang="ts">
import { v4 as uuid } from 'uuid'
import {
  computed,
  defineComponent,
  ref,
  onMounted,
  watchEffect,
} from 'vue'
import { toggleIcon } from '@/components/vue-collapsible-panel.constant'
import { useCollapsiblePanelStore } from '@/components/composables/vue-collapsible-panel.store'

export default defineComponent({
  name: 'VueCollapsiblePanel',
  props: {
    expanded: {
      type: Boolean,
      default: true,
    },
    hasContent: {
      type: Boolean,
      default: true,
    },
    initialize: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const idPanel = `panel-${uuid()}`
    const panelRef = ref<HTMLElement>()
    const bodyRef = ref<HTMLElement>()
    const bodyContentRef = ref<HTMLElement>()
    const isInitialized = ref(props.initialize)
    const {
      panelExpanded,
      togglePanelExpandedStatus,
      setPanelExpandedStatus,
    } = useCollapsiblePanelStore()

    const idGroup = computed(() => {
      return panelRef.value?.parentElement?.getAttribute('data-id-group') || ''
    })

    const isExpanded = computed(
      () => panelExpanded(idGroup.value, idPanel).value && props.hasContent,
    )

    if (!isInitialized.value) {
      const isInitializedWatchStop = watchEffect(() => {
        if (isExpanded.value) {
          isInitialized.value = true
          isInitializedWatchStop()
        }
      })
    }

    const toggle = (): void => {
      if (!props.hasContent) return

      togglePanelExpandedStatus(idGroup.value, idPanel)
    }

    const collapse = (element: HTMLElement): void => {
      element.style.height = '0'
    }

    const expand = (element: HTMLElement): void => {
      element.style.height = `${element.scrollHeight}px`
    }

    const setHeightAuto = (element: HTMLElement): void => {
      element.style.height = 'auto'
    }

    onMounted(() => {
      setPanelExpandedStatus(idGroup.value, idPanel, props.expanded)
    })

    return {
      panelRef,
      bodyRef,
      bodyContentRef,
      isExpanded,
      isInitialized,
      collapse,
      expand,
      setHeightAuto,
      toggle,
      toggleIcon,
    }
  },
})
</script>

<style lang="scss" scoped>
  .vcp {
    $root: &;
    $timing-function: cubic-bezier(.5, .25, 0, 1);

    &__header {
      display: flex;
      padding: 12px;
      pointer-events: none;

      #{$root}--expandable & {
        pointer-events: auto;
        cursor: pointer;
      }
    }

    &__header-title {
      display: flex;
      align-items: center;
      flex: 1;
    }

    &__header-icon {
      display: flex;
      align-items: center;
      transition: transform .3s $timing-function;

      #{$root}--expanded & {
        transform-origin: center;
        transform: rotate(180deg);
      }

      > span {
        display: flex;
        height: 24px;
        width: 24px;
      }

      ::v-deep(svg) {
        height: 100%;
        width: 100%;
      }
    }

    &__body {
      overflow: hidden;
      transition: all .3s $timing-function;
    }

    &__body-content {
      padding: 12px;
    }
  }

  .slide-enter-from,
  .slide-leave-to {
    opacity: .25;
  }
</style>
