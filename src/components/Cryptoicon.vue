<template>
  <svg
    :width="size"
    :height="size"
    :class="`cryptoicon--${symbol}`"
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 32 32"
    @click="$emit('click')"
    v-html="icon" />
</template>

<script>
export default {
  name: 'Cryptoicon',
  props: {
    symbol: {
      type: String,
      default: null
    },
    color: {
      type: String,
      default: null
    },
    size: {
      type: [String, Number],
      default: '24'
    },
    generic: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      lookupSymbol: new Map([['BCHSV', 'BSV'], ['BCHABC', 'BAB']])
    };
  },
  computed: {
    lSymbol() {
      return this.symbol && this.symbol.toLowerCase();
    },
    uSymbol() {
      return this.symbol && this.symbol.toUpperCase();
    },
    icon() {
      const symbol = this.lookupSymbol.has(this.uSymbol)
        ? this.lookupSymbol.get(this.uSymbol)
        : this.symbol;

      if (symbol) {
        let icon = this.$options.lib.find(i => i.symbol === symbol.toLowerCase());
        if (icon) {
          return !this.color ? icon.colorIcon() : icon.plainIcon(this.color);
        }
      }
      
      let genericIcon = this.$options.lib.find(i => i.symbol == 'generic');
      return !this.color ? genericIcon.colorIcon() : genericIcon.plainIcon(this.color);      
    }
  },
  lib: [],
  add(icons) {
    if (Array.isArray(icons)) {
      this.lib = icons;
    } else {
      this.lib.push(icons);
    }
  }
};
</script>
