<template>
          <div class="q-pa-md" style="width: 100%">
   <q-card>
        <q-tabs
          v-model="tab"
          dense
          class="bg-grey-3 text-grey-7"
          active-color="primary"
          indicator-color="black"
          align="justify"
        />
        <q-tab-panel name="home">
            <div class="text-h6">Food</div>
            <q-input class="white" v-model.lazy="search" filled type="search" hint="Start typing in a food like (onions)">
         <template v-slot:prepend>
          <q-icon name="search" />
         </template>
        <template v-slot:append>
      <q-checkbox v-model="fodmap" label="Fodmap" />
      <q-checkbox v-model="scd" label="SCD" />
      <q-checkbox v-model="safe" label="My SafeFoods" />
      <q-checkbox v-model="unsafe" label="My UnSafeFoods" />
        </template>
      </q-input>
      <q-list bordered separator>
      <q-virtual-scroll
      :scroll-target="scrollTarget"
      :items="filteredItems"
      separator
    >
      <template v-slot="{ item, index }">
        <q-item
          :key="index"
        >
          <q-item-section class="q-pa-lg">
            <q-item-label class="text-h3">
              {{ item.name }} - {{ item.category }}
            </q-item-label>
          </q-item-section>
          <q-item-section side center>
            <q-item-tag>
              <q-chip class="q-mr-sm" text-color="white" color="red" v-if="item.scd_safe == 5">
                SCD Unsafe <q-icon name="warning" color="white" class="q-ml-sm" />
              </q-chip>
              <q-chip class="q-mr-sm" text-color="white" v-on:click="saveFoodlist()" color="green" v-if="item.scd_safe == 1">
                SCD Safe
              </q-chip>
            </q-item-tag>
            <q-item-tag>
              <q-chip color="red" text-color="white" v-if="item.fodmap_safe == 5">
                 Fodmap Unsafe <q-icon name="warning" color="white" class="q-ml-sm" />
              </q-chip>
              <q-chip color="green" text-color="white" v-if="item.fodmap_safe == 1">
                Fodmap Safe {{item.amount}}
              </q-chip>
            </q-item-tag>
            <q-item-tag>
              <q-toggle
                v-model="item.safe_food"
                color="green"
                label="Safe Food?"
                true_value=1
                left-label
                keep-color
              />
            </q-item-tag>
          </q-item-section>
        </q-item>
      </template>
    </q-virtual-scroll>
    </q-list>
    </q-tab-panel>
      </q-card>
  </div>
</template>

<script>
import { LocalStorage } from 'quasar'
export default {
  name: 'PageIndex',
  created () {
    try {
      const foodls = LocalStorage.getItem('foodlist')
      if (foodls !== null) this.foodlist = foodls
    } catch (e) {
      console.log(e)
    }
    if (this.foodlist.length < 1) {
      console.log('in here')
      this.$axios.get('http://informational.ca/awds.html')
        .then((response) => {
          console.log(response)
          this.foodlist = response.data
        })
        .catch(() => {
          this.$q.notify({
            color: 'negative',
            position: 'top',
            message: 'Loading failed',
            icon: 'report_problem'
          })
        })
    }
  },
  methods: {
    addSafefoods: function () {

    },
    saveFoodlist: function () {
      LocalStorage.setItem('foodlist', this.foodlist)
    }
  },
  data () {
    return {
      icons: null,
      active: false,
      foodlist: [],
      search: '',
      add: [],
      scd: false,
      fodmap: false,
      safe: false,
      unsafe: false,
      tab: 'home'
    }
  },
  computed: {
    autocomplete () {
      console.log('in here')
      return this.foodlist.filter(v => v.name === this.name)
    },
    filteredItems () {
      if (this.search.length < 3 && this.scd === false && this.fodmap === false && this.unsafe === false && this.safe === false) {
        return this.foodlist
      }
      return this.foodlist.filter(item => {
        console.log(item)
        if (this.fodmap === true && item.fodmap_safe > 3) {
          return false
        }
        if (this.scd === true && item.scd_safe > 3) {
          return false
        }
        if (this.unsafe === true && item.safe_food !== false) {
          return false
        }
        if (this.safe === true && item.safe_food !== true) {
          return false
        }
        return item.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1
      })
    }
  }
}
</script>
