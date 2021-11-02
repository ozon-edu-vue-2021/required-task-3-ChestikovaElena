<template>
  <div
    :class="['icon__wrapper', {
        'icon__wrapper--selected': isSelected
      }]"
    @click="iconClickHandler"
  >
    <svg
      width="24"
      height="24"
      viewBox="0 0 24 24"
      xmlns="http://www.w3.org/2000/svg"
      class="icon"
      v-html="types[iconType]"
    >
    </svg>
  </div>
</template>

<script>
export default {
  name: 'Icon',
  props: {
    iconType: {
      type: String,
      default: 'home'
    },
    selectedIcon: {
      type: String,
      default: 'home'
    }
  },
  computed: {
    isSelected() {
      return this.iconType === this.selectedIcon
    }
  },
  data: () => ({
    types: {
      home:
        `<path d="M0 0h24v24H0V0z" fill="none"/><path d="M12 5.69l5 4.5V18h-2v-6H9v6H7v-7.81l5-4.5M12 3L2 12h3v8h6v-6h2v6h6v-8h3L12 3z"/>`,
      email:
        `<path d="M0 0h24v24H0V0z" fill="none"/><path d="M22 6c0-1.1-.9-2-2-2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6zm-2 0l-8 5-8-5h16zm0 12H4V8l8 5 8-5v10z"/>`,
      more:
        `</g><g><g><rect height="2" width="9" x="13" y="7"/><rect height="2" width="9" x="13" y="15"/><rect height="2" width="6" x="16" y="11"/><polygon points="13,12 8,7 8,11 2,11 2,13 8,13 8,17"/></g></g>`,
      link:
        `<path d="M0 0h24v24H0V0z" fill="none"/><path d="M17 7h-4v2h4c1.65 0 3 1.35 3 3s-1.35 3-3 3h-4v2h4c2.76 0 5-2.24 5-5s-2.24-5-5-5zm-6 8H7c-1.65 0-3-1.35-3-3s1.35-3 3-3h4V7H7c-2.76 0-5 2.24-5 5s2.24 5 5 5h4v-2zm-3-4h8v2H8z"/>`
    }
  }),
  methods: {
    iconClickHandler() {
      this.$emit('update:selectedIcon', this.iconType);
    }
  }
}
</script>

<style scoped>
.icon__wrapper {
  width: 30px;
  height: 30px;
  border-radius: 6px;
  background-color: lightgrey;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon__wrapper--selected {
  background-color: grey;
}

.icon__wrapper--selected > .icon {
  fill: white;
}

.icon {
  fill: blue;
  cursor: pointer;
}

</style>
