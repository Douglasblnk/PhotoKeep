<script setup>
import { onClickOutside } from '@vueuse/core';

const props = defineProps({
  modelValue: Boolean,
});

const emit = defineEmits(['close']);

const target = ref(null);

const secondRender = ref(false);

const closeModal = () => {
  secondRender.value = false;

  setTimeout(() => {
    emit('close');
  }, 100);
};

onClickOutside(target, closeModal);

watch(
  () => props.modelValue,
  (value) => {
    if (value) {
      document.body.style.overflow = 'hidden';

      return setTimeout(() => {
        secondRender.value = true;
      }, 100);
    }

    else {
      document.body.style.overflow = 'auto';

      secondRender.value = false;
    }
  },
);

defineExpose({ closeModal });
</script>

<template>
  <Teleport to="body">
    <Transition name="fade">
      <div
        v-if="modelValue"
        w:pos="fixed top-0 left-0"
        w:backdrop="~ blur-lg"
        w:bg="black opacity-30"
        w:w="screen"
        w:h="screen"
        w:flex="~"
        w:justify="center"
        w:align="items-center"
        w:z="fullscreen"
      >
        <Transition name="modal">
          <div
            v-if="secondRender"
            ref="target"
          >
            <slot />
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>
