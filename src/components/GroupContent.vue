<!--GroupConent-->
<template>
  <div class="grp_box">
    <div class="title_background">
      <button
        type="button"
        title="移除群組"
        class="grp_remove"
        @click="removeGroup()"
      ></button>
      <button
        type="button"
        title="新增角色"
        class="add_new_role"
        @click="AddRole()"
      ></button>
      <h1 class="grp_title">{{ parentMsg.Name }}</h1>
    </div>
    <div class="grp_list">
      <RoleContent
        v-for="role in roles"
        :key="role.SN"
        :parent-msg="role"
        @remove-role="deleteRole"
      />
    </div>
  </div>
</template>
<script>
import RoleContent from "./RoleContent.vue";
export default {
  components: {
    RoleContent,
  },
  data() {
    return { roles: [] };
  },
  methods: {
    initialRole() {
      const promi = this.axios.get("http://localhost/roleGet.php", {
        params: { SN: this.parentMsg.SN },
      });
      promi
        .then((response) => {
          if (response.data) {
            response.data.forEach((item) =>
              this.roles.push({ SN: item.RolesSN, Name: item.RolesName })
            );
          }
        })
        .catch(function (response) {
          console.log(response);
        });
    },
    AddRole() {
      var RoleName = prompt("請輸入新角色名稱").trim();
      if (RoleName) {
        const promi = this.axios.get("http://localhost/roleAdd.php", {
          params: { Name: RoleName, groupSN: this.parentMsg.SN },
        });
        promi
          .then((response) =>
            response.data.forEach((item) =>
              this.roles.push({ SN: item.SN, Name: item.Name })
            )
          )
          .catch(function (response) {
            console.log(response);
          });
      } else {
        alert("至少輸入一個字。");
      }
    },
    removeGroup() {
      var deleteCheck = confirm("確定要移除該群組嗎？\n(移除後資料將一併刪除)");
      if (deleteCheck) {
        this.$emit("remove-group", this.parentMsg);
      }
    },
    deleteRole(childData) {
      var roleID = this.roles.findIndex((item) => item == childData);
      this.roles.splice(roleID, 1);
      this.axios.get("http://localhost/roleDelete.php", {
        params: { SN: childData.SN },
      });
    },
  },
  props: ["parentMsg"],
  created() {
    this.initialRole();
  },
};
</script>
<style scoped>
.grp_box {
  height: 300px;
  border: 3px solid black;
}
.title_background {
  background-color: black;
}
.grp_title {
  background-color: black;
  color: white;
  text-align: left;
  margin: 0;
  overflow: hidden;
}
.grp_remove {
  background-image: url("../assets/garbage-can1.png");
  background-size: contain;
  height: 25px;
  width: 25px;
  margin: 5px;
  float: right;
}
.grp_remove:hover {
  background-image: url("../assets/garbage-can.png");
  background-size: contain;
  height: 25px;
  width: 25px;
}
.add_new_role {
  background-image: url("../assets/plus.png");
  background-size: contain;
  height: 25px;
  width: 25px;
  margin: 5px;
  float: right;
}
.add_new_role:hover {
  background-image: url("../assets/add.png");
  background-size: contain;
  height: 25px;
  width: 25px;
}
.grp_list {
  height: 260px;
  overflow-y: scroll;
}
</style>
