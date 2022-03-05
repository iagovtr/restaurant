<template>
  <div class="category-menu">
    <ul>
      <li v-for="category in categoriesList" :key="category.id" @click="onCategoryClick(category.id)" :class="{ active: isActive(category.id) }">
        <component :is="category.icon" />
        <p>{{ category.label }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import Fondue from "../assets/icons/fondue.svg";
import Deserts from "../assets/icons/ice-cream.svg";
import Drinks from "../assets/icons/smothie.svg";

export default {
  name: "CategoryMenu",
  components: {
    Fondue,
    Deserts,
    Drinks,
  },
  data() {
    return {
      categoriesList: [
        { label: "Fondues", icon: "Fondue", id: "fondue" },
        { label: "Bebidas", icon: "Drinks", id: "drinks" },
        { label: "Açaís", icon: "Deserts", id: "deserts" },
      ],
      selectedCategory: "",
    };
  },
  mounted() {
    this.onCategoryClick("fondue");
  },
  methods: {
    onCategoryClick(id) {
      this.selectedCategory = id;
      this.$store.dispatch("changeCategory", id);
    },
    isActive(id) {
      return this.selectedCategory === id;
    },
  },
};
</script>

<style lang="less" scoped>
.category-menu {
  width: 130px;
  min-width: 130px;
  height: 100vh;
  background: white;
  display: flex;
  align-items: center;

  ul {
    list-style: none;
    padding: 0;
    width: 100%;

    li {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100px;
      cursor: pointer;

      p {
        margin-bottom: 0px;
        font-weight: 500;
        font-size: 14px;
        color: @dark-grey;
      }

      svg {
        height: 41px;
        path {
          fill: @dark-grey;
        }
      }
      &.active {
        background: @yellow;
        border-radius: 8px;

        p {
          color: black;
        }

        svg {
          path {
            fill: black;
          }
        }
      }
    }
  }

  @media @small-desktops{
    width: 100%;
    max-width: 800px;
    margin: auto;
    height: fit-content;

    ul {
      display: flex;
      justify-content: space-around;
      margin: 20px;
      overflow: scroll;

      li {
        min-width: 78px;
      }
    }
  }

  @media @smartphones {
    ul {
      justify-content: unset;
    }
  }
}
</style>