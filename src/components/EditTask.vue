<template>
    <div class="background flex items-center justify-center">
        <div class="w-1/3 p-5 form-container">
            <form  class="w-full flex flex-col" @submit.prevent="">
                <input type="text" placeholder="Tytuł..." class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200 font-bold" :class="{'border-red-700' : validation.title}" v-model="actualData.title">
                <span class="text-red-700  text-right text-sm font-bold" v-show="validation.title">Pole tytuł jest wymagane!</span>
                <input type="text" placeholder="krótki opis" class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200" v-model="actualData.description">
                <div class="flex mx-auto">
                    <div @click="openHideStatus" title="Ustaw typ zadania" class=" cursor-pointer flex items-center align-center m-3 mr-5 text-3xl text-gray-500 relative"> <StatusList v-on:statusClick="saveStatus" :show="showStatusTypes" /> <i v-if="!showStatusTypes" :class="actualData.icon"></i> 
                    <i v-else class="fas fa-times text-red-600 cursor-pointer"></i>  </div>
                    <div @click="setPrio" title="Ustaw priorytet" class="cursor-pointer flex items-center align-center m-3 mr-5 text-3xl text-gray-500">  <i class="fab fa-product-hunt" :class="{'text-red-500' : actualData.prio}"></i> </div>
                    <div @click="setTime" title="Ustaw lub wyłącz przypomnienie" class="cursor-pointer flex items-center align-center m-3 mr-5 text-3xl text-orange-500">  <i class="far fa-clock" :class="{'text-gray-500' : setAlarm}"></i> </div>
                    

 
                    <input type="date" class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200 font-bold" :class="{'text-gray-500' : setAlarm}" v-model="actualData.date" :disabled="setAlarm">
                    <input type="time" class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200" :class="{'text-gray-500' : setAlarm}" v-model="actualData.time" :disabled="setAlarm">
                </div>
                <div class="w-full ">
                <button type="submit" class="border  ml-5 mr-10  mr-2 my-4 py-2 px-10 rounded-md text-white font-bold addButton">Aktualizuj zadanie</button>
                 <button @click="cancel"  class="border my-4 py-2 px-2 rounded-md text-white font-bold addButton"> Anuluj zmiany</button>
                 </div>
            </form>
        </div>
    </div>
</template>

<script>
import StatusList from "@/components/StatusList.vue";
export default {
  components: {
    StatusList
  },
  props: {
    taskData: {
      required: true
    },
    dateActive: {
      type: Object
    }
  },
  data() {
    return {
      //   title: "",
      //   description: "",
      //   date: "",
      //   time: "",
      //   prio: false,
      //   status: 1,
      actualData: JSON.parse(JSON.stringify(this.taskData)),
      validation: false,
      showStatusTypes: false,
      setAlarm: true
    };
  },
  mounted() {
    if (this.actualData.date && this.actualData.time) {
      this.setAlarm = false;
    }
  },
  methods: {
    saveStatus(status) {
      this.showStatusTypes = true;
      if (status.status.id === 0) {
        this.actualData.statusId = 0;
        this.actualData.icon = "fas fa-list-ul";
      } else {
        this.actualData.icon = status.status.icon;
        this.actualData.statusId = status.status.id;
      }
    },
    openHideStatus() {
      this.showStatusTypes = !this.showStatusTypes;
    },
    setPrio() {
      this.actualData.prio = !this.actualData.prio;
    },
    setTime() {
      if (!this.setAlarm) {
        this.setAlarm = true;
        this.actualData.date = null;
        this.actualData.time = null;
      } else {
        this.setAlarm = false;
        this.actualData.date = this.dateActive.date;
        this.actualData.time = this.dateActive.time;
      }
    },
    cancel() {
      this.$emit("cancel");
    }
  }
};
</script>

<style lang="css" scoped>
.background {
  position: fixed;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.952);
  z-index: 999;
}

input {
  margin-bottom: 3px;
}
</style>