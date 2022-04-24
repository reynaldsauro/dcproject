<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Order Items</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="item"
        placeholder="Enter item"
        class="w-100 form-control"
      />
      <button class="btn btn-warning rounded-0" @click="submitItem">
        SUBMIT
      </button>
    </div>

    <!-- Item table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Items</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in items" :key="index">
          <td>
            <span :class="{ 'line-through': item.status === 'delivered' }">
              {{ item.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': item.status === 'pending',
                'text-success': item.status === 'shipped',
                'text-warning': item.status === 'delivered',
              }"
            >
              {{ capitalizeFirstChar(item.status) }}
            </span>
          </td>
          <td class="text-center">
            <div @click="deleteItem(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editItem(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      item: "",
      editedItem: null,
      statuses: ["pending", "shipped", "delivered"],

      /* Status could be: 'pending' / 'shipped' / 'delivered' */
      items: [
        {
          name: "Chocolate",
          status: "pending",
        },
        {
          name: "1/4 sugar",
          status: "shipped",
        },
        {
          name: "50kg rice",
          status: "delivered",
        },
      ],
    };
  },

  methods: {
    /**
     * Capitalize first character
     */
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    /**
     * Change status of item by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.items[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.items[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes item by index
     */
    deleteItem(index) {
      this.items.splice(index, 1);
    },

    /**
     * Edit item
     */
    editItem(index) {
      this.item = this.items[index].name;
      this.editedItem = index;
    },

    /**
     * Add / Update items
     */
    submitItem() {
      if (this.item.length === 0) return;

      /* We need to update the item */
      if (this.editedItem != null) {
        this.items[this.editedItem].name = this.item;
        this.editedItem = null;
      } else {
        /* We need to add new item */
        this.items.push({
          name: this.item,
          status: "pending",
        });
      }

      this.item = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}
</style>
