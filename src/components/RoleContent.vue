<!--RoleContent-->
<template>
  <div class="role_box">
    <div class="title_background">
      <button
        type="button"
        title="移除角色"
        class="role_remove"
        @click="removeRole()"
      ></button>
      <h1 class="role_title">{{ parentMsg.Name }}</h1>
    </div>
    <draggable
      class="role_list"
      v-model="members"
      :group="{ name: 'memberdata', put: true }"
      :move="onMove"
      @change="onChange"
      :animation="200"
    >
      <div class="role_content" v-for="member in members" :key="member.ID">
        {{ member.Name }}
      </div>
    </draggable>
  </div>
</template>
<script>
import draggable from "vuedraggable";
export default {
  components: {
    draggable,
  },
  data() {
    return {
      members: [],
    };
  },
  methods: {
    initialMember() {
      const promi = this.axios.get("http://localhost/memberGet.php", {
        params: { SN: this.parentMsg.SN },
      });
      promi
        .then((response) => {
          if (response.data) {
            response.data.forEach((item) =>
              this.members.push({ ID: item.ID, Name: item.Name })
            );
          }
        })
        .catch(function (response) {
          console.log(response);
        });
    },
    onMove(evt) {
      var dragCheck = !evt.relatedContext.list.find(
        (item) => item.ID == evt.draggedContext.element.ID
      );
      var selfCheck = evt.relatedContext.component._uid == this._uid + 1;
      return selfCheck || dragCheck;
    },
    onChange(evt) {
      if (evt.added) {
        console.log(evt.added.element.ID);
        console.log(this.parentMsg.SN);
        this.axios.get("http://localhost/memberAdd.php", {
          params: { ID: evt.added.element.ID, SN: this.parentMsg.SN },
        });
      }
      if (evt.removed) {
        console.log(evt.removed.element.ID);
        this.axios.get("http://localhost/memberDelete.php", {
          params: { ID: evt.removed.element.ID, SN: this.parentMsg.SN },
        });
      }
    },
    removeRole() {
      var deleteCheck = confirm("確定要移除該角色嗎？\n(移除後資料將一併刪除)");
      if (deleteCheck) {
        this.$emit("remove-role", this.parentMsg);
      }
    },
  },
  props: ["parentMsg"],
  created() {
    this.initialMember();
  },
};
</script>
<style scoped>
.title_background {
  background-color: #50c8f8;
}
.role_remove {
  background-image: url("../assets/garbage-can1.png");
  background-size: contain;
  height: 25px;
  width: 25px;
  margin: 5px;
  float: right;
}
.role_remove:hover {
  background-image: url("../assets/garbage-can.png");
  background-size: contain;
  height: 25px;
  width: 25px;
}
.role_title {
  background-color: #50c8f8;
  color: white;
  text-align: left;
  margin: 0;
  overflow: hidden;
}
.role_list {
  background-color: white;
  padding: 5px;
  height: 100px;
  overflow-y: scroll;
}
.role_content {
  font-size: large;
  cursor: grab;
}
</style>
