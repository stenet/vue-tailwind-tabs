<script lang="ts">
import { computed, h, ref, onMounted, onUnmounted } from 'vue';

export default {
  props: {
    selectedIndex: {
      type: Number,
      required: false,
    },
  },
  emits: ['update:selectedIndex'],
  setup(props, context) {
    const itemRefs = ref({});
    const tabRef = ref();
    const windowWidth = ref();

    const checkWindowResize = () => {
      windowWidth.value = window.innerWidth;
    };

    const selected =
      props.selectedIndex == void 0
        ? ref(0)
        : computed({
            get() {
              return props.selectedIndex;
            },
            set(newValue) {
              context.emit('update:selectedIndex', newValue);
            },
          });

    onMounted(() => {
      window.addEventListener('resize', checkWindowResize);
    });
    onUnmounted(() => {
      window.removeEventListener('resize', checkWindowResize);
    });

    return () => {
      const item =
        itemRefs.value[selected.value] != void 0
          ? itemRefs.value[selected.value]
          : null;

      const tabRects = tabRef.value ? tabRef.value.getClientRects() : null;

      const tabRect = tabRects?.length > 0 ? tabRects[0] : null;

      const itemRects = item ? item.getClientRects() : null;

      const itemRect = itemRects?.length > 0 ? itemRects[0] : null;

      return [
        h(
          'div',
          {
            ref: (el) => (tabRef.value = el),
            class:
              'relative inline-flex flex-wrap gap-1 bg-neutral-200 rounded p-1',
          },
          {
            default: () => [
              ...context.slots.default?.().map((r, i) =>
                h(
                  'div',
                  {
                    ref: (el) => (itemRefs.value[i] = el),
                    class:
                      'z-10 outline-neutral-600 rounded outline-neutral-200 hover:bg-white/50',
                    tabIndex: 0,
                    onClick: () => {
                      selected.value = i;
                    },
                    onKeypress: (ev) => {
                      console.log('keypress');
                      if ([13, 32].includes(ev.keyCode)) {
                        selected.value = i;
                      }
                    },
                  },
                  h(
                    'div',
                    {
                      class: 'px-6 py-1 text-sm rounded cursor-pointer',
                    },
                    r.props.text
                  )
                )
              ),
              itemRect && tabRect
                ? h(
                    'div',
                    {
                      class:
                        'absolute rounded bg-white transition-all duration-300 shadow',
                      style: `left: ${itemRect.left - tabRect.left}px; top: ${
                        itemRect.top - tabRect.top
                      }px; width: ${itemRect.width}px; height: ${
                        itemRect.height
                      }px`,
                      'data-w': windowWidth.value,
                    },
                    ''
                  )
                : null,
            ],
          }
        ),
        h(
          'div',
          {
            class: 'mt-4',
          },
          context.slots.default?.().filter((r, i) => i == selected.value)
        ),
      ];
    };
  },
};
</script>
