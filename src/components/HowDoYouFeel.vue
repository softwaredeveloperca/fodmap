<template>
<div>
 <div class="text-h6 text-black">How do you feela?</div>
            <div class="q-pa-md">
            <div class="q-gutter-y-md column">
              <q-rating
                v-model="ratingModel"
                v-on:click="updateRating()"
                :max="5"
                size="3.5em"
                color="green-5"
                :icon="icons"
              />
            </div>
            </div>
</div>
</template>

<script>
export default {
  name: 'HowDoYou',
  title: 'How Do You Feel',
  methods: {
    updateRatings: function () {
      let myRatings = []
      try {
        myRatings = this.$q.localStorage.getItem('ratings')
      } catch (e) {
        myRatings.push({ ctime: Date.now(), value: this.ratingValue() })
        this.ratingValue = 0
        this.ratingModel = 0
        this.$q.localStorage.setItem('ratings', myRatings)
      }
    }
  },
  data () {
    return {
      ratingValue: 0,
      ratingModel: 0
    }
  }
}
</script>
