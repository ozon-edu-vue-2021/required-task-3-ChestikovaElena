<template>
  <div id="app" @click="hideCard">
    <div class="office">
      <Map
        @update:isUserOpenned="openCard"
        :tables="tables"
        :legend="legend"
        :people="people"
      />
      <SideMenu
        :person="selectedPerson"
        :isUserOpenned="isUserOpenned"
        :tables="tables"
        :legend="legend"
        @update:isUserOpenned="openCard"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import tables from "@/assets/data/tables_v2.json";
import legend from "@/assets/data/legend_v2.json";
import people from "@/assets/data/people.json";


export default {
  name: "App",
  data() {
    return {
      isUserOpenned: false,
      selectedPerson: null,
      tables: [],
      legend: [],
      people: [],
    }
  },
  components: {
    Map,
    SideMenu,
  },
  created() {
    this.loadLegendTablesPeople();
  },
  methods: {
    openCard(isUserOpenned, person = null) {
      this.isUserOpenned = isUserOpenned;
      this.selectedPerson = person;
    },
    hideCard($event) {
      const clickedElement = $event.target;
      if (!clickedElement.closest(".employee-place")
        && !clickedElement.closest(".menu")
        || clickedElement.closest(".action")
        ) {
        this.openCard(false);
      }
    },
    loadLegendTablesPeople() {
      this.tables = tables;
      this.people = people;

      legend.map((item) => {
          const counterFromTables = tables.filter(({ group_id }) => group_id === item.group_id)?.length ?? 0;
          if (counterFromTables !== item.counter && counterFromTables !== 0) {
            item.counter = counterFromTables;
          };
        });
        
        this.legend = legend;
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 320px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
