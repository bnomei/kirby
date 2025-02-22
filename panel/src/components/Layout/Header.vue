<template>
  <header :data-editable="editable" class="k-header">
    <k-headline tag="h1" size="huge">
      <span v-if="editable && $listeners.edit" class="k-headline-editable" @click="$emit('edit')">
        <!-- @slot headline -->
        <slot />
        <k-icon type="edit" />
      </span>
      <slot v-else />
    </k-headline>
    <k-bar v-if="$slots.left || $slots.right" class="k-header-buttons">
      <template #left>
        <!-- @slot buttons on the left -->
        <slot name="left" />
      </template>
      <template #right>
        <!-- @slot buttons on the right -->
        <slot name="right" />
      </template>
    </k-bar>

    <k-tabs :tab="tab" :tabs="tabsWithBadges" theme="notice" />
  </header>
</template>

<script>
/**
 * The Header component is a composition of a big fat headline plus two optional slots for buttons — directly below the headline and on the right. The Header is a fundamental part of any main Panel view. While we use the left slot for option buttons, the right slot is mainly used for prev/next navigation between items such as pages or users.
 * @internal
 */
export default {
  props: {
    /**
     * Whether the headline is editable
     */
    editable: Boolean,
    tab: String,
    tabs: {
      type: Array,
      default() {
        return []
      }
    }
  },
  computed:  {
    tabsWithBadges() {
      const changed = Object.keys(this.$store.getters['content/changes']());

      return this.tabs.map(tab => {

        // collect all fields per tab
        let fields = [];
        Object.values(tab.columns).forEach(column => {
          Object.values(column.sections).forEach(section => {
            if (section.type === "fields") {
              Object.keys(section.fields).forEach(field => {
                fields.push(field);
              })
            }
          })
        });

        // get count of changed fields in this tab
        tab.badge = fields.filter(field => changed.includes(field.toLowerCase())).length;

        return tab;
      });
    }
  }
}
</script>

<style>
.k-header {
  padding-top: 4vh;
  margin-bottom: 2rem;
  border-bottom: 1px solid var(--color-border);
}
.k-header .k-headline {
  min-height: 1.25em;
  margin-bottom: .5rem;
  word-wrap: break-word;
}
.k-header .k-header-buttons {
  margin-top: -.5rem;
  height: 3.25rem;
}
.k-header .k-headline-editable {
  cursor: pointer;
}
.k-header .k-headline-editable .k-icon {
  color: var(--color-gray-500);
  opacity: 0;
  transition: opacity .3s;
  display: inline-block;
  margin-inline-start: .5rem
}
.k-header .k-headline-editable:hover .k-icon {
  opacity: 1;
}
</style>
