<template>
<div>
        <div class="menuContainer text-gray-500" :class="{'active' : menuOpen}" @click="openHideMenu">
            <ul>
              
                <li class="menuItem first"><i class="far fa-check-circle"></i> <p>Zobacz gotowe zadania</p></li>
                <li class="menuItem second"><i class="far fa-clock"></i>  <p>Włącz/wyłącz zegar</p></li>
                <li class="menuItem third"><i class="fas fa-file-download"></i>  <p>Zaimportuj listę</p></li>
                <li class="menuItem fourth"><i class="fas fa-file-upload"></i>  <p>Zaimportuj listę do txt</p></li>
                <li class="menuItem five"><i class="far fa-images"></i> <p>Ustaw tapetę</p></li>
            </ul>
               <i class="far fa-times-circle text-red-500"></i>
        </div>
        <div id="openButton" v-if="!menuOpen" @click="openHideMenu">
            >>
        </div>
        </div>
</template>

<script>
export default {
  data() {
    return {
      menuOpen: false
    };
  },
  methods: {
    openHideMenu() {
      this.menuOpen = !this.menuOpen;
    }
  }
};
</script>

<style lang="css" scoped>
.menuContainer {
  width: 50px;
  height: 50px;
  position: fixed;
  top: 50%;
  left: 0;
  transform: translate(-100%, -50%);
  transition: 0.2s;
}

.menuContainer.active {
  transform: translate(0%, -50%);
}

.menuContainer i {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 30px;
  opacity: 0;
  transition: 0.2s;
  cursor: pointer;
}

.menuContainer.active i {
  opacity: 1;
}
#openButton {
  position: fixed;
  top: 50%;
  left: 0;
  height: 50px;
  width: 25px;
  transform: scale(1) translate(0, -50%);
  border: solid 1px white;
  border-left: none;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
  line-height: 50px;
  background-color: rgba(0, 0, 0, 0.582);
  user-select: none;
  cursor: pointer;
  color: rgb(161, 161, 161);
  font: bold;
  transition: 0.2s;
}
#openButton:hover {
  transform: scale(1.1) translate(0, -50%);
}

.menuItem {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  height: 40px;
  width: 40px;
  /* background-color: royalblue; */
  border-radius: 100%;
  opacity: 0;
}

.active > ul > .menuItem {
  animation-name: opacity;
  animation-delay: 1s;
  animation-duration: 0.3s;
  animation-fill-mode: both;
  /* background-color: red; */
}

.active > ul > .menuItem.first {
  animation-name: firstIn, opacity;
  animation-delay: 0.1s;
  animation-duration: 0.3s;
  /* animation-fill-mode: both; */
}
.active > ul > .menuItem.second {
  animation-name: secondIn, opacity;
  animation-delay: 0.1s;
  animation-duration: 0.3s;
  /* animation-fill-mode: both; */
}

.active > ul > .menuItem.third {
  animation-name: thirdIn, opacity;
  animation-delay: 0.1s;
  animation-duration: 0.3s;
  /* animation-fill-mode: both; */
}

.active > ul > .menuItem.fourth {
  animation-name: fourthIn, opacity;
  animation-delay: 0.1s;
  animation-duration: 0.3s;
  /* animation-fill-mode: both; */
}

.active > ul > .menuItem.five {
  animation-name: fiveIn, opacity;
  animation-delay: 0.1s;
  animation-duration: 0.3s;
  /* animation-fill-mode: both; */
}

i:hover {
  transform: translate(-50%, -50%) scale(1.2);
  color: white;
}

li:hover p {
  background-color: rgba(0, 0, 0, 0.685);
  user-select: none;
}

li p {
  position: absolute;
  left: 103%;
  top: 50%;
  transform: translateY(-50%);
  width: 200px;
  font-weight: bold;
  letter-spacing: 1px;
  color: rgba(245, 245, 245, 0.795);
}

@keyframes opacity {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}

@keyframes firstIn {
  0% {
    transform: translate(-50%, -50%);
  }

  100% {
    transform: translate(-50%, -140px);
  }
}
@keyframes secondIn {
  0% {
    transform: translate(-50%, -50%);
  }

  100% {
    transform: translate(80px, -90px);
  }
}

@keyframes thirdIn {
  0% {
    transform: translate(-50%, -50%);
  }

  100% {
    transform: translate(100px, -50%);
  }
}

@keyframes fourthIn {
  0% {
    transform: translate(-50%, -50%);
  }

  100% {
    transform: translate(70px, 55px);
  }
}

@keyframes fiveIn {
  0% {
    transform: translate(-50%, -50%);
  }

  100% {
    transform: translate(-50%, 100px);
  }
}
</style>