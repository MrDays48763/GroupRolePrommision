<template>
  <div class="staff_main">
    <input class="search_bar" type="search" placeholder="Search user" v-model="query" />
    <h1 class="staff_title">User</h1>
    <draggable
      class="staff_list"
      v-model="staff_list"
      :group="{ name: 'memberdata', pull: 'clone', put: false }"
      :sort="false"
      :move="onMove"
      :animation="200"
    >
      <div class="staff_content" v-for="item in searchList" :key="item.ID">
        {{ item.Name }}
      </div>
    </draggable>
    <h1 class="cancel_title">Cancel Block</h1>
    <draggable
      class="staff_cancel"
      v-model="staffs"
      :group="{ name: 'memberdata', put: true }"
      @add="deleteStaff"
      ><div class="cancel_content"></div
    ></draggable>
  </div>
</template>
<script>
import draggable from "vuedraggable";
export default {
  components: { draggable },
  data() {
    return {
      staff_list: [],
      query: "",
      staffs: [],
    };
  },
  methods: {
    initialUser() {
      const promi = this.axios.get("http://localhost/userGet.php");
      promi
        .then((response) =>
          response.data.forEach((item) =>
            this.staff_list.push({ ID: item.ID, Name: item.Name })
          )
        )
        .catch(function (response) {
          console.log(response);
        });
    },
    onMove(evt) {
      return !evt.relatedContext.list.find(
        (item) => item.ID == evt.draggedContext.element.ID
      );
    },
    onEnd(evt) {
      console.log(evt.item);
    },
    deleteStaff() {
      this.staffs.splice(0, 1);
    },
  },
  computed: {
    searchList() {
      return this.staff_list.filter((item) => item.Name.includes(this.query));
    },
  },
  created() {
    this.initialUser();
  },
};
</script>
<style scoped>
.search_bar {
  height: 35px;
  width: 300px;
  border-radius: 35px;
}
.search_bar::placeholder {
  font-size: large;
}
.staff_title {
  background-color: black;
  color: white;
  margin: 0;
  margin-top: 20px;
}
.staff_list {
  border: 3px black solid;
  overflow-y: scroll;
  padding: 5px;
  height: 400px;
}
.staff_content {
  border: 2px solid #aaaaaa;
  text-align: left;
  margin-bottom: 5px;
  font-size: large;
  cursor: grab;
}
.cancel_title {
  background-color: black;
  color: white;
  margin: 0;
}
.staff_cancel {
  background-image: url("../assets/garbage-can.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  height: 150px;
  border: 3px solid black;
  padding: 5px;
}
</style>
