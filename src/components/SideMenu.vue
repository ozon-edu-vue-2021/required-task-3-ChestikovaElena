<template>
  <div class="menu">
    <div class="toolbar">
      <div class="toolbar__header">
        <template v-if="!isUserOpenned">
          <h3>Информация</h3>
        </template>
        <template v-else>
          <div class="action">
            <div class="arrow" @click="closeProfile"></div>
          </div>
          <h3>Профиль</h3>
        </template>
      </div>
      <div class="toolbar__actions"></div>
    </div>
    <div class="content">
      <div v-if="!isUserOpenned" class="legend"> 
        <div class="legend__data">
          <div v-if="legend.length > 0" class="legend__items">
            <Draggable v-model="legend">
              <LegendItem
                v-for="(item, index) in legend"
                :key="index"
                :color="item.color"
                :text="item.text"
                :counter="item.counter"
                class="legend__item"
              />
            </Draggable>
          </div>
          <span v-else class="legend--empty"> Список пуст </span>
        </div>
        <div class="legend__chart">
          <Chart :legend="legend" :tables="tables"/>
        </div>
      </div>
      <div v-show="isUserOpenned" class="profile">
        <div v-if="!person" class="profile__empty">Место пустое</div>

        <PersonCard v-else :person="person" :subdivision="subdivision"/>
      </div>
    </div>
  </div>
</template>

<script>
import LegendItem from "./SideMenu/LegendItem.vue";
import PersonCard from "./SideMenu/PersonCard.vue";
import Chart from "./SideMenu/Chart.vue";
import Draggable from "vuedraggable";
import getEmptyArray from "@/utils/consts.js";

export default {
  props: {
    isUserOpenned: {
      type: Boolean,
      default: false,
    },
    person: {
      type: Object,
      default: null,
    },
    tables: {
      type: Array,
      default: getEmptyArray,
    },
    legend: {
      type: Array,
      default: getEmptyArray,
    },
  },
  components: {
    LegendItem,
    PersonCard,
    Draggable,
    Chart,
  },
  computed: {
    subdivision() {
      let subdivision;
      if (this.person) {
        const personTable = this.tables.find(({ _id }) => _id === this.person.tableId);
        subdivision = this.legend.find(({ group_id }) => group_id === personTable.group_id)?.text ?? "";
      }
      return subdivision;
    }
  },
  methods: {
    closeProfile() {
      this.$emit("update:isUserOpenned", false);
    },
  },
};
</script>

<style scoped>
.menu {
  border-left: 1px solid #ccd8e4;
  padding: 24px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.toolbar .toolbar__actions button {
  font-size: 0.76rem;
  text-transform: uppercase;
  letter-spacing: 0.08rem;
  padding: 2px 6px;
}

.toolbar__header {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}

.toolbar__header .action {
  cursor: pointer;
  margin-right: 14px;
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.toolbar__header .action .arrow {
  width: 10px;
  height: 10px;
  border-top: 2px solid blue;
  border-right: 2px solid blue;
  transform: rotate(-135deg);
}

h3 {
  margin: 0;
}

.content {
  flex: 1;
}

.content .legend {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
}

.content .legend .legend__data {
  display: flex;
  height: 100%;
}

.content .legend .legend__items {
  flex: 1;
  width: 100%;
}

.content .legend .legend__items .legend__item:not(:first-child) {
  margin-top: 16px;
}

.content .legend .legend__items .legend__item {
  cursor: pointer;
}

.content .legend .legend__items .legend__item.sortable-chosen {
  opacity: 25%;
}

.content .legend .legend--empty {
  align-self: center;
  width: 100%;
  text-align: center;
}

.profile {
  padding-top: 20px;
}
</style>
