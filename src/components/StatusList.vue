<template>
    <div class="containerOfStatus" v-if="show">
        <ul>
            <li @click="sendData(status)" v-for="status in statusTypes" :key="status.id " class="flex items-center px-3 py-2"><i :class="status.icon" class="mr-3"></i> <p>{{status.name}}</p> </li>
        </ul>
    </div>
</template>

<script>
export default {
  props: {
    show: {
      dafault: false,
      type: Boolean
    }
  },
  methods: {
    sendData(status) {
      this.$emit("statusClick", { status });
    }
  },
  data() {
    return {
      statusTypes: [
        {
          id: 0,
          name: "Domyślny",
          icon: "fas fa-circle"
        },
        {
          id: 1,
          name: "oddzwoń",
          icon: "fas fa-phone-alt"
        },
        {
          id: 2,
          name: "obserwuj",
          icon: "far fa-eye"
        },
        {
          id: 3,
          name: "mail",
          icon: "far fa-envelope"
        },
        {
          id: 4,
          name: "SMS",
          icon: "fas fa-sms"
        },
        {
          id: 5,
          name: "czy wyjdzie?",
          icon: "fas fa-shipping-fast"
        },
        { id: 6, name: "wycena", icon: "fas fa-comments-dollar" }
      ]
    };
  }
};
</script>

<style lang="css" scoped>
.containerOfStatus {
  position: absolute;
  left: -21vw;
  top: 0;
  width: 20vw;
  background-color: white;
  border-radius: 20px;
  max-height: 40vh;
  overflow-y: scroll;
  z-index: 99;
}
li {
  transition: 0.2s;
  cursor: pointer;
}
li:hover {
  background-color: royalblue;
  color: white;
}

li:hover i {
  color: gold;
}
</style>