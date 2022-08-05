<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />

      </q-toolbar>

      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">Todo</div>
        <div class="text-subtitle1">{{ todaysDate }}</div>
      </div>

      <q-img src="../../public/mountains.jpg" class="header-image absolute-top" />

    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above :width="250" :breakpoint="600">
      <q-scroll-area style="height: calc(100% - 192px); margin-top: 192px; border-right: 1px solid #ddd">
        <q-list padding>
          <q-item to="/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section>
              Todo
            </q-item-section>
          </q-item>

          <q-item to="/help" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section>
              Help
            </q-item-section>
          </q-item>

        </q-list>
      </q-scroll-area>

      <q-img class="absolute-top" src="../../public/mountains.jpg" style="height: 192px">
        <div class="absolute-bottom bg-transparent">

          <q-avatar size="56px" class="q-mb-sm">
            <img src="https://avatars.githubusercontent.com/u/87587042?v=4">
          </q-avatar>

          <div class="text-weight-bold"></div>
          <div class="col">

            <!-- name -->
            <div class="cursor-pointer" style="width: 100px">
              {{ inputName }}
              <q-popup-edit v-model="inputName" class="bg-white text-white" v-slot="scope">
                <q-input dark color="light-blue-14" input-class="text-light-blue-14" v-model="scope.value" dense
                  autofocus counter @keyup.enter="scope.set">
                  <template v-slot:append>
                    <q-icon name="edit" color="light-blue-14" />
                  </template>
                </q-input>
              </q-popup-edit>
            </div>
            <!-- fim name -->

            <!-- name -->
            <div class="cursor-pointer" style="width: 100px">
              {{ inputId }}
              <q-popup-edit v-model="inputId" class="bg-white text-white" v-slot="scope">
                <q-input dark color="light-blue-14" input-class="text-light-blue-14" v-model="scope.value" dense
                  autofocus counter @keyup.enter="[editeItens('id'), scope.set]">
                  <template v-slot:append>
                    <q-btn size="xs" round color="light-blue-14" icon="edit" />
                  </template>
                </q-input>
              </q-popup-edit>
            </div>
            <!-- fim name -->

          </div>

        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <keep-alive>
        <router-view />
      </keep-alive>
    </q-page-container>

  </q-layout>


</template>

<script>
import { defineComponent, ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import { date } from 'quasar'


export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  data: () => ({
    name: localStorage.getItem('name'),
    id: localStorage.getItem('id')
  }),

  mounted() {
    const name = localStorage.getItem('name')
    const id = localStorage.getItem('id')
    if (name) this.inputName = name
    if (id) this.inputId = id
  },

  setup() {
    const leftDrawerOpen = ref(false)
    const modal = ref(false)
    const inputName = ref('Nome')
    const inputId = ref('ID')

    function editeItens (data) {
      switch (data) {
        case 'name':
          localStorage.setItem('name', inputName.value)
          break
        case 'id':
          localStorage.setItem('id', inputId.value)
          break
      }
    }

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value
      },
      modal,
      inputName, inputId,

    }
  },

  computed: {
    todaysDate: () => {
      const timeStamp = Date.now()
      const formattedString = date.formatDate(timeStamp, 'dddd D MMMM')
      return formattedString
    }
  }
})
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.2;
  filter: grayscale(100%);
}

.btn {
  cursor: pointer;
}
</style>
