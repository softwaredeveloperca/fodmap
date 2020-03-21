<template>
  <span>
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
      <q-chip class="q-mr-sm" text-color="white" color="green" v-if="item.scd_safe == 1">
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
        v-model="foodlist[index].safefood"
        color="green"
        label="Safe Food?"
        left-label
      />
    </q-item-tag>
  </q-item-section>
  </span>
</template>

<script>
export default {
  name: 'Foods',
  title: 'Food List'
}
</script>
