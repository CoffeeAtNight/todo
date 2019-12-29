<template>
    <div class="">
        <ToDoList :date="time" />
        <LeftMenu />
    </div>
</template>

<script>
import ToDoList from "@/components/ToDoList.vue";
import LeftMenu from "@/components/LeftMenu.vue";
export default {
  components: {
    ToDoList,
    LeftMenu
  },
  data() {
    return {
      time: {
        date: null,
        time: null
      },
      dataInterval: null
    };
  },
  mounted() {
    let checkDate = () => {
      let date = new Date();
      let month = date.getMonth() + 1;
      this.time.date = date.getFullYear() + "-" + month + "-" + date.getDate();

      let hours = parseInt(date.getHours());
      let minutes = parseInt(date.getMinutes());

      hours = hours > 9 ? hours : "0" + hours;
      minutes = minutes > 9 ? minutes : "0" + minutes;

      this.time.time = hours + ":" + minutes;
      //   console.log(this.time.time);
    };

    this.dataInterval = setInterval(checkDate, 1000);
  },

  beforeDestroy() {
    clearInterval(this.dataInterval);
  }
};
</script>

<style lang="scss" scoped>
</style>