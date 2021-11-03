<template>
  <div class="map" @click="clickHandler">
    <h3>Карта офиса</h3>
    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" />
      <TableSVG v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import MapSVG from "@/assets/images/map.svg";
import TableSVG from "@/assets/images/workPlace.svg";
import * as d3 from "d3";
import { showNotification } from "@/utils/showNotification.js"
import getEmptyArray from "@/utils/consts.js";

export default {
  props: {
    tables: {
      type: Array,
      default: getEmptyArray,
    },
    legend: {
      type: Array,
      default: getEmptyArray,
    },
    people: {
      type: Array,
      default: getEmptyArray,
    }
  },
  components: {
    MapSVG,
    TableSVG,
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      group: null,
      tableSVG: null,
    };
  },
  mounted() {
    this.svg = d3.select(this.$refs.svg);
    this.group = this.svg.select("g");
    this.tableSVG = d3.select(this.$refs.table);

    try {
      this.drawTables();
    } catch(error) {
      showNotification(error);
    }
  },
  methods: {
    drawTables() {
      const svgTablesGroup = this.group.append("g").classed("groupPlaces", true);
      this.tables.map((table) => {
        const svgTable = svgTablesGroup
          .append("g")
          .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
          .attr("id", table._id)
          .classed("employee-place", true);

        svgTable
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .attr("id", table.group_id)
          .html(this.tableSVG.html())
          .attr(
            "fill",
            this.legend.find(({ group_id }) => group_id === table.group_id)?.color ?? "transparent"
          );
      });
    },
    clickHandler($event) {
      const clickedElement = $event.target;
      if (clickedElement.closest(".employee-place")) {
        const clickedElementID = clickedElement.closest(".employee-place").id;
        const currentEmployee = this.people.find(({ tableId }) => String(tableId) === clickedElementID);
        
        this.$emit("update:isUserOpenned", true, currentEmployee);
      }
    }
  }
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}

.table:hover {
  fill: grey;
}
</style>
