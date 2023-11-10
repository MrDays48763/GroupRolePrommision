<!--HomeView.vue-->
<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <h1>群組角色權限管理</h1>
    <div class="main">
      <GroupRegion class="grp_reg" />
      <!-- <RoleRegion class="role_reg" /> -->
      <StaffRegion class="staff_reg" />
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
import GroupRegion from "../components/GroupRegion.vue"
// import RoleRegion from "../components/RoleRegion.vue"
import StaffRegion from "../components/StaffRegion.vue"
export default {
  name: 'HomeView',
  components: {
    GroupRegion,
    // RoleRegion,
    StaffRegion
  }
}
</script>
<style scoped>
.main {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  gap: 50px;
  max-width: 100em;
  margin:0 auto;
}
.grp_reg {
  grid-column:1/5;
}
/* .role_reg {
flex: 1;
} */
.staff_reg {
  grid-column:5;
  grid-row:1;
  position:sticky;
  top:5em;
  height:35em;
}
</style>