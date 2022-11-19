<template>
  <div>
    <h1>All Events list</h1>

    <EventCard
    v-for="(event, index) in events"
    :key="index"
    :event="event"
    :data-index="index"

    />
  </div>
</template>

<script>
import {mapState} from 'vuex'
import EventCard from '~/components/EventCard.vue'
export default {
  components:{
    EventCard
  },
  async fetch({store, error}){ // fetch is a hook by nuxt inside 'pages', waits for the store
    try { await store.dispatch('events/fetchEvents')
    } catch(e) {
      error({
        statusCode: 503,
        message: 'Unable to fetch events at this time. Please try again'
      })
    }
  },
  head() { // <-- property used by vue-meta to add header tags
    return {
      title: 'Event Listing', // <-- For our title tag
      meta: [
        {
          hid: 'description',  
          name: 'description', // <-- for our meta description tag
          content:
            'Where you can find all the events taking place in your neighborhood'
        }
      ]
    }
  },
  computed: mapState({
      events: state => state.events.events
    })
}
</script>
