<script lang="ts">
import { h, ref, onMounted, onUnmounted } from 'vue';

export default {
  setup(props, context) {
    const selected = ref(0);
    const itemRefs = ref({});
    const tabRef = ref();
    const windowWidth = ref();

    const checkWindowResize = () => {
      windowWidth.value = window.innerWidth;
    };

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
              'relative inline-flex flex-wrap gap-2 bg-neutral-300 rounded p-1',
          },
          {
            default: () => [
              ...context.slots.default?.().map((r, i) =>
                h(
                  'div',
                  {
                    ref: (el) => (itemRefs.value[i] = el),
                    class:
                      'z-10 outline-neutral-600 rounded outline-neutral-300 hover:bg-neutral-100',
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
                      class: 'px-2 py-1 text-sm rounded cursor-pointer',
                    },
                    r.props.text
                  )
                )
              ),
              itemRect && tabRect
                ? h(
                    'div',
                    {
                      class: 'absolute rounded bg-white transition-all shadow',
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
