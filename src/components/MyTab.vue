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

      const itemCount = context.slots.default?.().length ?? 0;

      return [
        h(
          'div',
          {
            ref: (el) => (tabRef.value = el),
            onKeydown: (ev) => {
              switch (ev.keyCode) {
                case 37:
                case 39: {
                  let newIndex =
                    ev.keyCode === 37 ? selected.value - 1 : selected.value + 1;

                  if (newIndex < 0) {
                    newIndex = itemCount - 1;
                  } else if (newIndex >= itemCount) {
                    newIndex = 0;
                  }

                  selected.value = newIndex;
                  itemRefs.value[newIndex].focus();
                  break;
                }
              }
            },
            class:
              'relative inline-flex flex-wrap gap-1 bg-neutral-200 rounded p-0.5',
          },
          {
            default: () => [
              ...context.slots.default?.().map((r, i) =>
                h(
                  'div',
                  {
                    ref: (el) => (itemRefs.value[i] = el),
                    class:
                      'z-10 outline-neutral-600 rounded outline-0 hover:bg-white/50 focus:bg-white/50',
                    tabIndex: 0,
                    onClick: () => {
                      selected.value = i;
                    },
                    onKeypress: (ev) => {
                      switch (ev.keyCode) {
                        case 13:
                        case 32: {
                          selected.value = i;
                          break;
                        }
                      }
                    },
                  },
                  h(
                    'div',
                    {
                      class: 'px-6 py-2 text-sm rounded cursor-pointer',
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
          context.slots.default?.().map((r, i) => {
            return h(r, {
              style: `${i === selected.value ? '' : 'display:none'}`,
            });
          })
        ),
      ];
    };
  },
};
</script>
