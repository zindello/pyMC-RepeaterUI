<script setup lang="ts">
interface Props {
  modelValue: boolean;
  message: string;
  title?: string;
  isRestarting?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Service Restart Required',
  isRestarting: false,
});

const emit = defineEmits<{
  'update:modelValue': [value: boolean];
  confirm: [];
  cancel: [];
}>();

function close() {
  emit('update:modelValue', false);
  emit('cancel');
}
</script>

<template>
  <Teleport to="body">
    <Transition
      enter-active-class="transition-opacity duration-200"
      enter-from-class="opacity-0"
      leave-active-class="transition-opacity duration-200"
      leave-to-class="opacity-0"
    >
      <div
        v-if="modelValue"
        class="fixed inset-0 z-50 flex items-center justify-center bg-black/60 backdrop-blur-sm"
        @click.self="close"
      >
        <div class="bg-white dark:bg-[var(--color-surface-elevated)] rounded-xl shadow-xl border border-stroke-subtle dark:border-stroke/20 p-6 max-w-md w-full mx-4">
          <div class="flex items-start gap-3 mb-4">
            <div class="flex-shrink-0 w-10 h-10 rounded-full bg-amber-100 dark:bg-amber-900/30 flex items-center justify-center">
              <svg class="w-5 h-5 text-amber-600 dark:text-amber-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M12 9v2m0 4h.01M10.29 3.86L1.82 18a2 2 0 001.71 3h16.94a2 2 0 001.71-3L13.71 3.86a2 2 0 00-3.42 0z" />
              </svg>
            </div>
            <div>
              <h3 class="text-base font-semibold text-content-primary dark:text-content-primary">
                {{ title }}
              </h3>
              <p class="mt-1 text-sm text-content-secondary dark:text-content-muted">
                {{ message }}
              </p>
            </div>
          </div>
          <div class="flex justify-end gap-2">
            <button
              @click="close"
              :disabled="isRestarting"
              class="px-3 sm:px-4 py-2 bg-background-mute dark:bg-white/5 hover:bg-stroke-subtle dark:hover:bg-white/10 text-content-primary dark:text-content-primary rounded-lg border border-stroke-subtle dark:border-stroke/20 transition-colors text-sm"
            >
              Cancel
            </button>
            <button
              @click="emit('confirm')"
              :disabled="isRestarting"
              class="px-3 sm:px-4 py-2 bg-primary/20 hover:bg-primary/30 text-content-primary dark:text-content-primary rounded-lg border border-primary/50 transition-colors text-sm disabled:opacity-50 disabled:cursor-not-allowed"
            >
              {{ isRestarting ? 'Restarting…' : 'Restart' }}
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>
