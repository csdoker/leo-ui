<template>
  <div
    @click="handleClick"
    :class="classes">
    <template v-if="loading">

      <o-icon type="loading" :size="iconSize" rotate></o-icon>

    </template>
    <template v-else-if="icon">

      <o-icon :type="icon" :size="iconSize"></o-icon>

    </template>
    <slot></slot>
  </div>
</template>

<script>
  import oIcon from '~/components/Icon';

  export default {
    name: 'oButton',
    components: { oIcon },
    props: {
      type: {
        validator (value) {
          return ['bg', 'shadow', 'text', 'dashed', ''].includes(value);
        },
        default: ''
      },
      custom: {
        validator (value) {
          return ['primary', 'info', 'success', 'warning', 'error', ''].includes(value);
        },
        default: ''
      },
      size: {
        validator (value) {
          return ['large', 'small', 'mini', ''].includes(value);
        },
        default: ''
      },
      icon: {
        type: String,
        default: ''
      },
      iconSize: {
        type: [Number, String]
      },
      disabled: {
        type: Boolean,
        default: false
      },
      loading: {
        type: Boolean,
        default: false
      },
      active: {
        type: Boolean,
        default: false
      },
      round: {
        type: Boolean,
        default: false
      },
      long: {
        type: Boolean,
        default: false
      }
    },
    inject: {
      form: { default: null }
    },
    data () {
      return {
        prefixCls: `${ this.$PrefixCls }-button`
      }
    },
    computed: {
      classes () {
        return [
          this.prefixCls,
          this.type,
          this.form ? this.form.size : this.size,
          this.disabled && !this.type ? '' : this.custom,
          {
            long     : this.long,
            disabled : this.disabled,
            round    : this.round && this.type !== 'text',
            primary  : ['bg', 'shadow'].indexOf(this.type) > -1 && !this.custom,
            active   : this.active && !this.custom && ['bg', 'shadow'].indexOf(this.type) < 0
          }
        ]
      }
    },
    methods: {
      handleClick (e) {
        this.disabled || this.loading || this.$emit('on-click', e);
      }
    }
  }
</script>