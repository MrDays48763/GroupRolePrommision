<!--GroupRegion-->
<template>
  <div class="grp_main">
    <div class="wrapping">
      <input class="search_bar" type="text" placeholder="Search group" v-model="query" />
      <button
        type="button"
        title="新增群組"
        class="add_new_grp"
        @click="AddGroup()"
      ></button>
    </div>
    <div class="grp_box_region">
      <GroupContent
        v-for="group in searchList"
        :key="group.SN"
        :parent-msg="group"
        @remove-group="deleteGroup"
      />
    </div>
  </div>
</template>
<script>
import GroupContent from "./GroupContent.vue";
export default {
  components: { GroupContent },
  data() {
    return {
      groups: [],
      query: "",
    };
  },
  methods: {
    initialGroup() {
      const promi = this.axios.get("http://localhost/groupGet.php");
      promi
        .then((response) => {
          if (response.data) {
            response.data.forEach((item) =>
              this.groups.push({ SN: item.SN, Name: item.Name })
            );
          }
        })
        .catch(function (response) {
          console.log(response);
        });
    },
    AddGroup() {
      var groupName = prompt("請輸入新群組名稱").trim();
      if (groupName) {
        const promi = this.axios.get("http://localhost/groupAdd.php", {
          params: { Name: groupName },
        });
        promi
          .then((response) =>
            response.data.forEach((item) =>
              this.groups.push({ SN: item.SN, Name: item.Name })
            )
          )
          .catch(function (response) {
            console.log(response);
          });
      } else {
        alert("至少輸入一個字。");
      }
    },
    deleteGroup(childData) {
      var grpID = this.groups.findIndex((item) => item == childData);
      this.groups.splice(grpID, 1);
      this.axios.get("http://localhost/groupDelete.php", {
        params: { SN: childData.SN },
      });
    },
  },
  computed: {
    searchList() {
      return this.groups.filter((item) => item.Name.toLowerCase().includes(this.query));
    },
  },
  created() {
    this.initialGroup();
  },
};
</script>
<style scoped>
.wrapping {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  height: 35px;
  width: 345px;
  gap: 10px;
  margin: 0 auto;
}
.search_bar {
  grid-column: 1/4;
  border-radius: 35px;
}
.search_bar {
  font-size: large;
}
.add_new_grp {
  background-image: url("../assets/plus.png");
  background-size: contain;
  grid-column: 4;
  width: 35px;
}
.add_new_grp:hover {
  background-image: url("../assets/add.png");
  background-size: contain;
  grid-column: 4;
  width: 35px;
}
.grp_box_region {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  gap: 50px;
  margin-top: 20px;
}
</style>
