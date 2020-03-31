<template>
  <nav class="chec-navigation" :class="{ 'chec-navigation--expanded': expanded }">
    <ChecLogo class="chec-navigation__logo" />
    <ul class="chec-navigation__links-list">
      <li
        v-for="({ meta, path }, i) in navItems"
        :key="i"
      >
        <router-link :to="path" active-class="chec-navigation__link--active">
          <div class="chec-navigation__icon-wrapper">
            <ChecNavIcon
              :icon="meta.navItem.iconName"
            />
          </div>
          <span>
            {{
              meta.navItem.label
            }}
          </span>
        </router-link>
      </li>
    </ul>
  </nav>
</template>
<script>
import ChecLogo from '@/assets/svgs/chec-logo.svg';
import ChecNavIcon from '@/components/ChecNavIcon.vue';

export default {
  name: 'ChecNavigation',
  components: {
    ChecLogo,
    ChecNavIcon,
  },
  props: {
    /**
     * Expands the nav by default
     */
    expanded: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    navItems() {
      if (!this.$router.options) {
        throw new Error('Could not locate vue-router instance from within <ChecNavigation>');
      }
      return this.$router.options.routes.filter(route => route.meta && route.meta.navItem);
    },
  },
};
</script>
<style scoped lang="scss">
  .chec-navigation {
    @apply fixed top-0 left-0 w-20 h-screen shadow-sm p-4 flex
    flex-col transition-all duration-300 ease-in-out;
    &__logo {
      @apply fill-current text-gray-500 w-full h-auto justify-start py-2;
      min-height: 3rem;
      max-width: 3rem;
    }
    &__links-list {
      @apply mt-20 flex-grow;
      li {
        @apply mb-2;
        > a {
          @apply relative flex items-center justify-between overflow-hidden w-full rounded;
          span {
            @apply flex-grow w-0 opacity-0 text-sm text-gray-600 duration-100;
          }
          &:hover {
            @apply bg-gray-100;
          }
        }
      }
    }
    &__link--active {
      @apply bg-gray-200;
    }
    &__icon-wrapper {
      @apply w-12 h-12 p-3 z-10;
      min-width: 3rem;
      > svg {
        @apply w-full h-full;
      }
    }
    &--expanded {
      width: 15rem;
      .chec-navigation__links-list {
        li {
          > a {
            span {
              @apply w-auto opacity-100;
            }
          }
        }
      }
    };
    &:hover {
      @extend .chec-navigation--expanded;
    }
  }
</style>