<template>
  <li
    @click="showSubItems(item)"
    class="menu-item"
    :class="{ 'menu-item-dropdown': item.subItems }"
  >
    <router-link
      :to="item.href"
      :target="item.href === 'onepage' ? '_blank' : null"
    >
      <span ref="route-name">
        {{ item.textContent }}
      </span>
      <span v-if="item.subItems" class="material-symbols-outlined">
        keyboard_arrow_right
      </span>
    </router-link>
    <ul
      class="menu-right-nav-container menu-right-subNav-container"
      :class="{ 'active-menu-right-subNav-container': isSubItem }"
    >
      <li
        v-for="(subItem, subIndex) of item.subItems"
        class="menu-item menu-sub-item"
        :class="{ 'menu-sub-item-visible': isSubItem }"
        :key="subIndex"
        @click="changeGridLayout(subItem)"
      >
        <router-link :to="subItem.href">
          <span class="sub-content">
            {{ subItem.textContent }}
          </span>
          <span
            v-if="subItem.subItems && !isSubItem"
            class="material-symbols-outlined"
          >
            keyboard_arrow_right
          </span>
          <span
            v-if="subItem.subItems && isSubItem"
            class="material-symbols-outlined"
          >
            keyboard_arrow_down
          </span>
        </router-link>
      </li>
    </ul>
  </li>
</template>

<script>
export default {
  name: "NavigationList",
  data() {
    return {
      isSubItem: false,
    };
  },
  props: {
    item: Object,
  },
  mounted() {
    this.activeRoute();
  },
  watch: {
    $route: function () {
      this.activeRoute();
    },
  },
  methods: {
    activeRoute() {
      const route = this.$route.name;
      const item = this.$refs["route-name"];
      item.classList.remove("active-route");
      if (route === item.innerHTML.trim()) {
        item.classList.add("active-route");
      }
    },
    showSubItems(item) {
      if (item.subItems) {
        this.isSubItem = !this.isSubItem;
      } else {
        this.$store.state.isRightMenuActive = false;
        this.$store.state.isRightMenuNotActive = true;
      }
    },
    changeGridLayout(item) {
      this.$store.state.layoutGrid = item.layoutGrid;
      this.$store.state.isRightMenuActive = false;
      this.$store.state.isRightMenuNotActive = true;
      if (item.layoutGrid === "three-column-masonry") {
        this.setLayout();
      }
    },
    setLayout() {
      const container = document.querySelector(".three-column-masonry");
      for (let i = 0; i < container.length; i++) {
        if (i % 3 === 0) {
          container[i].style.top = `${this.firstColumnTop}px`;
          this.firstColumnTop += container[i].clientHeight;
        } else if (i % 3 === 1) {
          container[i].style.left = "30vw";
          container[i].style.top = `${this.secondColumnTop}px`;
          this.secondColumnTop += container[i].clientHeight;
        } else {
          container[i].style.left = "60vw";
          container[i].style.top = `${this.thirdColumnTop}px`;
          this.thirdColumnTop += container[i].clientHeight;
        }
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.material-symbols-outlined {
  font-size: 1.2rem;
  transition: all 600ms ease-in-out;
}
li {
  display: block;
  align-items: center;
  justify-content: space-between;
  list-style: none;
  background-color: transparent;
  &:hover {
    span {
      color: #fafafc;
    }
    .material-symbols-outlined {
      transform: rotate(90deg);
    }
  }
  span {
    text-transform: uppercase;
    display: inline-flex;
    vertical-align: middle;
    color: #8c8c8e;
    transition: color 600ms ease-in-out;
  }
}
.menu-right-subNav-container {
  display: block;
  position: relative;
  transition: max-height 600ms ease-in-out;
  max-height: 0;
  padding-left: 0;
  background-color: #20202a;
  -webkit-filter: brightness(95%);
  filter: brightness(95%);
  box-shadow: inset 0 3px 8px 0 rgb(15 15 20 / 20%);
}
.active-menu-right-subNav-container {
  max-height: 500px;
}
a {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 7px 30px;
  line-height: 1.2;
}
.menu-item {
  font-size: 11px;
}
.menu-sub-item {
  font-size: 10px;
  opacity: 0;
  pointer-events: none;
  transition: opacity 600ms ease-in-out;
  &:first-child {
    padding-top: 15px;
  }
  &:last-child {
    padding-bottom: 15px;
  }
}
.menu-sub-item-visible {
  opacity: 1;
  pointer-events: all;
}
.sub-content {
  text-transform: uppercase;
}
</style>
