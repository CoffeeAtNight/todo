<template>
  <div
    class="container flex  w-1/3 h-full shadow-lg flex-col"
    :class="{ active: openMenu }"
  >
    <p
      class="text-white font-bold w-full h-20 flex items-center mb-6 p-5 shadow-xl tracking-wider"
    >
      Moje zadania do wykonania
    </p>
    <button class="exit font-bold text-gray-600 text-lg" @click="menuShow">
      {{ buttonValue }}
    </button>

    <h3 class="text-center">Stwórz własne zadanie</h3>

    <div class="flex flex-col  mx-4 border-b">
      <form @submit.prevent="createTaks" class="w-full flex flex-col">
        <input
          type="text"
          placeholder="Tytuł..."
          class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200 font-bold"
          :class="{ 'border-red-700': validation.title }"
          v-model="title"
        />
        <span
          class="text-red-700  text-right text-sm font-bold"
          v-show="validation.title"
          >Pole tytuł jest wymagane!</span
        >
        <input
          type="text"
          placeholder="krótki opis"
          class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200"
          v-model="description"
        />
        <div class="flex mx-auto">
          <div
            @click="openHideStatus"
            title="Ustaw typ zadania"
            class=" cursor-pointer flex items-center align-center m-3 mr-5 text-3xl text-gray-500 relative"
          >
            <StatusList v-on:statusClick="saveStatus" :show="showStatusTypes" />
            <i v-if="!showStatusTypes" :class="statusIcon"></i>
            <i v-else class="fas fa-times text-red-600 cursor-pointer"></i>
          </div>
          <div
            @click="setPrio"
            title="Ustaw priorytet (na liście czerwone)"
            class="cursor-pointer flex items-center align-center m-3 mr-5 text-3xl text-gray-500"
          >
            <i
              class="fab fa-product-hunt"
              :class="{ 'text-red-500': prio }"
            ></i>
          </div>
          <div
            @click="turnOnAlarm"
            title="Ustaw lub wyłącz przypomnienie"
            class="cursor-pointer flex items-center align-center m-3 mr-5 text-3xl text-orange-500"
          >
            <i class="far fa-clock" :class="{ 'text-gray-500': setAlarm }"></i>
          </div>

          <input
            type="date"
            class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200 font-bold"
            :class="{ 'text-gray-500': setAlarm }"
            v-model="activeDate"
            :disabled="setAlarm"
          />
          <input
            type="time"
            class="mb-3 bg-transparent border-b py-2 px-2 border-gray-200 text-gray-200"
            :class="{ 'text-gray-500': setAlarm }"
            v-model="activeTime"
            :disabled="setAlarm"
          />
        </div>

        <button
          type="submit"
          class="border mx-20 my-4 py-2 rounded-md text-white font-bold addButton"
        >
          Dodaj zadanie
        </button>
      </form>
    </div>
    <div class="mx-5 h-full overflow-y-scroll p-3 rounded-md mt-2">
      <TaskContainer
        v-on:deleteTask="deleteTask"
        v-for="task in taskList"
        :key="task.id"
        :taskData="task"
        :date="date"
      />
    </div>
  </div>
</template>

<script>
import TaskContainer from "@/components/TaskContainer.vue";
import StatusList from "@/components/StatusList.vue";
export default {
  props: {
    date: {
      required: true,
      type: Object
    }
  },
  components: {
    TaskContainer,
    StatusList
  },
  data() {
    return {
      title: "",
      description: "",
      statusId: 0,
      statusIcon: "fas fa-list-ul",
      prio: false,
      openMenu: true,
      showStatusTypes: false,
      setAlarm: true,
      activeDate: null,
      activeTime: null,
      taskList: [],
      maxId: 0,
      validation: {
        title: false
      }
    };
  },
  mounted() {
    if (localStorage.getItem("toDoData789")) {
      let dataFromStorage = JSON.parse(localStorage.getItem("toDoData789"));
      //   console.log(dataFromStorage);
      this.taskList = dataFromStorage.taskList;
      this.maxId = dataFromStorage.maxId;
    } else {
      let objectToSave = {
        maxId: 0,
        taskList: []
      };
      localStorage.setItem("toDoData789", JSON.stringify(objectToSave));
    }
  },
  methods: {
    deleteTask(id) {
      if (confirm("Czy usunąć pozycje?")) {
        let dataFromStorage = JSON.parse(localStorage.getItem("toDoData789"));
        dataFromStorage.taskList = this.taskList.filter(item => item.id !== id);
        this.taskList = dataFromStorage.taskList;
        localStorage.setItem("toDoData789", JSON.stringify(dataFromStorage));
      } else {
        alert("nothing");
      }
    },
    saveStatus(status) {
      if (status.status.id === 0) {
        this.statusId = 0;
        this.statusIcon = "fas fa-list-ul";
      } else {
        this.statusIcon = status.status.icon;
        this.statusId = status.status.id;
      }
    },
    menuShow() {
      this.openMenu = !this.openMenu;
    },
    openHideStatus() {
      this.showStatusTypes = !this.showStatusTypes;
    },
    turnOnAlarm() {
      if (this.setAlarm) {
        this.setAlarm = !this.setAlarm;
        this.activeDate = this.date.date;
        this.activeTime = this.date.time;
      } else {
        this.activeDate = null;
        this.activeTime = null;
        this.setAlarm = !this.setAlarm;
      }
    },
    setPrio() {
      this.prio = !this.prio;
    },
    createTaks() {
      if (this.title) {
        this.validation.title = false;
        let actualTime = this.date;

        let statusId = this.statusId;
        let icon = statusId === 0 ? "fas fa-circle" : this.statusIcon;

        let task = {
          id: ++this.maxId,
          title: this.title,
          prio: this.prio,
          description: this.description,
          date: this.activeDate,
          time: this.activeTime,
          addDate: actualTime.date,
          addTime: actualTime.time,
          statusId,
          icon
        };

        let taskId = this.maxId;
        this.taskList.push(task);

        let taskList = this.taskList;

        let dataToSave = {
          maxId: taskId,
          taskList
        };

        localStorage.setItem("toDoData789", JSON.stringify(dataToSave));

        this.title = "";
        this.description = "";
        this.activeDate = null;
        this.activeTime = null;
        this.setAlarm = true;
        this.prio = false;
        this.statusId = 0;
        this.statusIcon = "fas fa-list-ul";
      } else {
        this.validation.title = true;
      }
    }
  },
  computed: {
    buttonValue() {
      if (this.openMenu) return "X";
      return "<<";
    }
  }
};
</script>

<style lang="css" scoped>
.container {
  position: fixed;
  top: 0;
  right: -33.33333%;
  transition: 0.2s linear;
  background-color: rgba(0, 0, 0, 0.383);
}

.container p {
  background-color: rgba(0, 0, 0, 0.685);
}

.container.active {
  top: 0;
  right: 0;
}

.exit {
  position: absolute;
  width: 30px;
  height: 30px;
  left: -30px;
  top: 0;
}

.exit,
input:focus {
  outline: none;
  border: none;
}

.addButton {
  transform: scale(0.9);
  transition: 0.5s;
}

.addButton:hover {
  transform: scale(1);
  background-color: rgb(115, 177, 22);
  border-radius: 20px;
}

input {
  transition: 0.1s linear;
  transform: scale(0.9);
}

input:focus {
  transform: scale(1);
  background-color: rgba(0, 0, 0, 0.466);
  color: white;
}
i {
  transition: 0.2s;
}
i:hover {
  color: white;
  transform: scale(1.1);
}
</style>
