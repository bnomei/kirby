<template>
  <k-field :input="_uid" v-bind="$props" class="k-time-field">
    <k-input
      :id="_uid"
      ref="input"
      v-bind="$props"
      theme="field"
      type="time"
      v-on="listeners"
    />
  </k-field>
</template>

<script>
import { props as Field } from "../Field.vue";
import { props as Input } from "../Input.vue";
import { props as TimeInput } from "../Input/TimeInput.vue";

/**
 * Have a look at `<k-field>`, `<k-input>` and `<k-time-input>` 
 * for additional information.
 * @example <k-time-field v-model="time" name="time" label="Time" />
 */
export default {
  mixins: [
    Field,
    Input,
    TimeInput
  ],
  inheritAttrs: false,
  props: {
    icon: {
      type: String,
      default: "clock"
    }
  },
  computed: {
    listeners() {
      return {
        ...this.$listeners,
        update: input => this.$emit("input", input),
        input: () => {}
      };
    }
  },
  methods: {
    focus() {
      this.$refs.input.focus();
    }
  }
}
</script>
