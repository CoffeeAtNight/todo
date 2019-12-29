<template>
    <div class="bg-gray-100  mb-2 flex bordered rounded-l-lg h-32 " :class="{'bg-red-500' : taskData.prio, 'bg-green-600' : !taskData.prio}">
        <div class="flex items-center align-center mx-2 border-l-xs"><i :class="taskData.icon" class="text-gray-300"></i></div>
                
        <div class="bg-white px-2 py-1 flex-grow relative">
            <p class="font-bold">{{taskData.title}}</p>
            <p>{{taskData.description}}</p>
            <p class="text-sm">Dodano: {{taskData.addDate + " " + taskData.addTime}}</p>
            <!-- {{taskData}} -->
            <div class="bg-orange-500 text-center flex timer items-center pl-2" v-if="showTimer">
            <i class="fas fa-stopwatch text-white mr-2"></i> 
            <p v-if="!endOfTime" class="text-white">{{ days }} dni {{ hours }} : {{minutes}} : {{seconds}}</p> 
            <p v-else class="text-white">KONIEC CZASU!</p>
            
            </div>
        </div>
        <div class="flex flex-col align-between justify-around pr-2 bg-gray-200 shadow-lg text-2xl px-2">
            <button @click="deleteTask(taskData.id)"><i class="fas fa-times text-red-700"></i></button>
            <button @click="() => this.inEdit = !this.inEdit"><i class="fas fa-edit text-blue-700"></i></button>
            <button><i class="far fa-check-circle text-green-700"></i></button>
        </div>
        <EditTask :taskData="taskData" :dateActive="date" v-if="inEdit" v-on:cancel="() => this.inEdit = !this.inEdit" />
            </div>
</template>

<script>
import EditTask from "@/components/EditTask.vue";
export default {
  props: {
    taskData: {
      required: true,
      type: Object
    },
    date: {
      type: Object
    }
  },
  components: {
    EditTask
  },
  data() {
    return {
      timeInterval: null,
      showTimer: false,
      timeToEnd: null,
      endOfTime: false,
      days: null,
      hours: null,
      minutes: null,
      seconds: null,
      inEdit: false
    };
  },
  mounted() {
    if (this.taskData.date && this.taskData.time) {
      let stringDate = this.taskData.date + " " + this.taskData.time;
      this.timeToEnd = new Date(stringDate).getTime();
      this.showTimer = true;
      this.timeInterval = setInterval(() => {
        let actualTime = new Date().getTime();
        if (actualTime >= this.timeToEnd) {
          this.endOfTime = true;
          clearInterval(this.timeInterval);
          return;
        }
        // let time = Math.floor((this.timeToEnd - actualTime) / 1000);
        let days = Math.floor(
          this.timeToEnd / (1000 * 60 * 60 * 24) -
            actualTime / (1000 * 60 * 60 * 24)
        );
        let hours = Math.floor(
          (this.timeToEnd / (1000 * 60 * 60) - actualTime / (1000 * 60 * 60)) %
            24
        );
        let minutes = Math.floor(
          (this.timeToEnd / (1000 * 60) - actualTime / (1000 * 60)) % 60
        );

        let seconds = Math.floor(
          (this.timeToEnd / 1000 - actualTime / 1000) % 60
        );

        if (days < 10) {
          days = "0" + days;
        }
        if (hours < 10) {
          hours = "0" + hours;
        }
        if (minutes < 10) {
          minutes = "0" + minutes;
        }
        if (seconds < 10) {
          seconds = "0" + seconds;
        }

        this.days = days;
        this.hours = hours;
        this.minutes = minutes;
        this.seconds = seconds;

        console.log(days + " " + hours + " " + minutes + " " + seconds);
      }, 1000);
    }
  },
  beforeDestroy() {
    clearInterval(this.timeInterval);
  },
  methods: {
    deleteTask(id) {
      this.$emit("deleteTask", id);
    }
  }
};
</script>

<style lang="css" scoped>
.timer {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 100%;
}
</style>