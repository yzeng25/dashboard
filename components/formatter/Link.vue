<script>
import { get } from '@/utils/object';

const defaultOptions = {
  rel:    'nofollow noopener noreferrer',
  target: '_blank'
};

export default {
  props: {
    row: {
      type:     Object,
      required: true,
    },

    value: {
      type:     [Object, String],
      required: true
    },

    to: {
      type:    Object,
      default: null
    },

    urlKey: {
      type:    String,
      default: null,
    },

    labelKey: {
      type:    String,
      default: null,
    },

    options: {
      type:    [Object, String],
      default: null,
    },

    beforeIcon: {
      type:    String,
      default: null,
    },

    afterIcon: {
      type:    String,
      default: null,
    },

    beforeIconKey: {
      type:    String,
      default: null,
    },

    afterIconKey: {
      type:    String,
      default: null,
    },
  },

  computed: {
    href() {
      if ( this.urlKey ) {
        return get(this.row, this.urlKey);
      }

      if ((this.options === 'internal' || this.options?.internal) && this.to) {
        const defaultParams = this.$route.params;
        const toParams = this.to.params || {};

        return {
          ...this.to,
          params: {
            id: this.value, ...defaultParams, ...toParams
          }
        };
      }

      return this.value?.url;
    },

    rel() {
      if ( this.options && typeof this.options === 'object' ) {
        return this.options.rel;
      } else if ( this.value && typeof this.value === 'object' && this.value.rel !== undefined) {
        return this.value.rel;
      }

      return defaultOptions.rel;
    },

    target() {
      if ( this.options && typeof this.options === 'object' ) {
        return this.options.target;
      } else if ( this.value && typeof this.value === 'object' && this.value.target !== undefined) {
        return this.value.target;
      }

      return defaultOptions.target;
    },

    label() {
      if ( this.labelKey ) {
        return get(this.row, this.labelKey);
      } else if ( typeof this.value === 'string' ) {
        return this.value;
      }

      return this.value?.text || this.href;
    },

    beforeIconClass() {
      if ( this.beforeIconKey ) {
        return get(this.row, this.beforeIconKey);
      }

      return this.beforeIcon;
    },

    afterIconClass() {
      if ( this.afterIconKey ) {
        return get(this.row, this.afterIconKey);
      }

      return this.afterIcon;
    }
  }
};
</script>

<template>
  <nuxt-link v-if="options === 'internal' && href" :to="href">
    <i v-if="beforeIconClass" :class="beforeIconClass" style="position: relative; top: -2px;" />
    {{ label }}
    <i v-if="afterIconClass" :class="afterIconClass" style="position: relative; top: -2px;" />
  </nuxt-link>
  <a v-else-if="href" :href="href" :rel="rel" :target="target">
    <i v-if="beforeIconClass" :class="beforeIconClass" style="position: relative; top: -2px;" />
    {{ label }}
    <i v-if="afterIconClass" :class="afterIconClass" style="position: relative; top: -2px;" />
  </a>
  <span v-else>{{ label }}</span>
</template>
