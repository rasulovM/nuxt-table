<template>
  <div class="container table__container">
    <div
      class="table__overlay"
      v-if="showPopup"
      @click="showPopup = false"
    ></div>
    <popup
      v-if="showPopup"
      :addNewTagline="addNewTagline"
      @closePopup="closePopup"
    />
    <h1 class="title">Table</h1>
    <v-simple-table dense class="table pa-3">
      <thead>
        <tr>
          <th></th>
          <th
            class="table__title pl-6 pa-0"
            v-for="(tagline, indx) in taglines"
            :key="indx"
          >
            <span class="table__title-text"> {{ tagline }}</span>
            <v-icon
              color="grey"
              class="table__title-icon"
              @click="removeColumn(indx)"
              small
              >mdi-close</v-icon
            >
          </th>
          <th></th>
        </tr>
      </thead>
      <draggable
        v-model="items[id]"
        tag="tbody"
        @end="changePos"
        handle=".table__move-row"
        class="table__body"
      >
        <tr v-for="(item, indx1) in items" class="table__row pl-3" :key="indx1">
          <td>
            <v-icon class="table__move-row" small color="black"
              >mdi-arrow-all</v-icon
            >
          </td>
          <td
            tag="td"
            class="table__cell pa-0"
            v-for="(column, indx2) in taglines"
            :key="indx2"
          >
            <cell
              @updateCell="updateCell"
              :title="item[indx2]"
              :indxRow="indx1"
              :indxCol="indx2"
            />
          </td>
          <td class="table__cell">
            <v-btn color="green" outlined small @click="removeRow(item)">
              Удалить
              <v-icon small color="green" class="ml-2 table__icon"
                >mdi-delete</v-icon
              >
            </v-btn>
          </td>
        </tr>
      </draggable>
    </v-simple-table>
    <div class="table__btns">
      <v-btn color="warning" dark normal @click="showPopup = true"
        >Добавить колонку<v-icon normal class="ml-2">mdi-plus</v-icon></v-btn
      >
      <v-btn color="primary" normal @click="addRow()"
        >Добавить строку<v-icon normal class="ml-2">mdi-plus</v-icon></v-btn
      >
    </div>
  </div>
</template>

<script>
import Cell from "./Cell";
import Popup from "./Popup.vue";
import draggable from "vuedraggable";
export default {
  components: {
    Popup,
    Cell,
    draggable,
  },
  name: "Table",
  data() {
    return {
      value: "",
      itemCell: "",
      showPopup: false,
      items: [
        [1, "Valeria", "Williams", "26", "Female"],
        [2, "Brad", "Douglas", "22", "Male"],
        [3, "Emily", "Tucker", "20", "Female"],
        [4, "Antony", "Richardson", "24", "Male"],
        [5, "Garry", "Barnes", "22", "Male"],
        [6, "Dexter", "Cole", "27", "Male"],
      ],
      taglines: ["#", "First Name", "Last Name", "Age", "Gender"],
      id: "",
    };
  },
  methods: {
    changePos(e) {
      this.id = e.to.rowIndex;
    },
    removeRow(item) {
      let index = "";
      this.items.forEach((i, indx) => {
        if (i === item) {
          index = indx;
        }
      });
      const arr = [...this.items];
      this.items = arr.filter((_, indx2) => index !== indx2);
    },
    removeColumn(indx) {
      this.items = this.items.map((i, index) =>
        i.filter((k, index) => index !== indx)
      );
      this.taglines = this.taglines.filter((_, index) => indx != index);
    },
    updateCell(newVal, oldVal, indxRow, indxCol) {
      if (!oldVal) {
        return (this.items[indxRow][indxCol] = newVal);
      }
      this.items.forEach((i, indx1) => {
        i.forEach((k, indx2) => {
          if (k === oldVal) {
            this.items[indx1][indx2] = newVal;
          }
        });
      });
    },
    addRow() {
      const arr = this.taglines.map((i) => "");
      this.items.push(arr);
    },
    addNewTagline(title) {
      this.taglines = [...this.taglines, title];
      this.items.map((i) => i.push(""));
      this.closePopup();
    },
    closePopup() {
      this.showPopup = false;
    },
  },
};
</script>

<style lang="scss">
.table {
  max-width: 1200px;
  overflow-x: auto;
  height: 100%;
  flex: 1;
  &__move-row {
    cursor: move;
  }
  &__title {
    transition: all 0.3s ease;
    &-icon {
      margin-top: -10px;
      margin-left: 15px;
      opacity: 0;
      visibility: hidden;
      cursor: pointer;
    }
    &:hover {
      .table__title-icon {
        opacity: 1;
        visibility: visible;
      }
    }
  }
  &__container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    padding-bottom: 40px;
  }
  &__body {
    height: 100%;
  }
  &__cell {
    padding-top: 0 !important;
    padding-bottom: 0 !important;
    min-width: 150px;
    &:first-child {
      min-width: 50px;
      width: 80px;
    }
  }
  &__overlay {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0%;
    background: rgba(0, 0, 0, 0.52);
    z-index: 888;
  }
}
</style>
