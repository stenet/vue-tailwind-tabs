<script lang="ts">
import { h, ref } from 'vue';

export default {
  setup(props, context) {
    const selected = ref(0);
    const itemRefs = ref({});
    const tabRef = ref();

    //TODO, wenn sich die Breite ändert, dann muss neu gerendert werden

    return () => {
      const item =
        itemRefs.value[selected.value] != void 0
          ? itemRefs.value[selected.value]
          : null;

      const tabRects = tabRef.value ? tabRef.value.getClientRects() : null;

      const tabRect = tabRects?.length > 0 ? tabRects[0] : null;

      const itemRects = item ? item.getClientRects() : null;

      const itemRect = itemRects?.length > 0 ? itemRects[0] : null;

      return h(
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
                  class: 'z-10',
                  onClick: () => {
                    selected.value = i;
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
                  },
                  ''
                )
              : null,
          ],
        }
      );
    };
  },
};
</script>
