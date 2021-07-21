<template>
  <h1>Events For Good</h1>
  <div class="events">
    <div class="pagination">
    <router-link
      id="minPageSize"
      :to="{ name: 'EventList', query: {  page: page, limit: limit - 1} }"
      rel="prev"
      v-if="limit != 1"
    >
    - Page size
    </router-link>
    {{limit}}
    <router-link
      id="addPageSize"
      :to="{ name: 'EventList', query: {  page: page, limit: limit + 1} }"
      rel="prev"
      v-if="limit < totalEvents"
      >
      + Page size
      </router-link>
    </div>
    
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <div class="pagination">
      <router-link
      id="page-prev"
      :to="{ name: 'EventList', query: { page: page - 1, limit: limit} }"
      rel="prev"
      v-if="page != 1"
    >
      Prev Page
    </router-link>
    {{page}}
    <router-link
      id="page-next"
      :to="{ name: 'EventList', query: { page: page + 1 , limit: limit} }"
      rel="next"
      v-if="hasNextPage"
    >
      Next Page
    </router-link>
    </div>
    
  </div>
</template>

<script>
// @ is an alias to /src
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import { watchEffect } from '@vue/runtime-core'
// import axios from 'axios'
export default {
  name: 'EventList',
  props: {
    page: {
      type: Number,
      required: true
    },
    limit: {
      type: Number,
      required: true
    }
  },
  components: {
    EventCard // register it as a child component
  },
  data() {
    return {
      events: null,
      totalEvents: 0,
    }
  },
  created() {
    watchEffect(() => {
      EventService.getEvents(this.limit, this.page)
        .then((res) => {
          this.events = res.data
          this.totalEvents = res.headers['x-total-count']
          console.log(res.headers)
        })
        .catch((err) => {
          console.log(err)
        })
    })
  },
  computed: {
    hasNextPage() {
      let totalPages = Math.ceil(this.totalEvents / this.limit)
      return this.page < totalPages
    }
  }
}
</script>
<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}
#page-prev {
  text-align: left;
}
#page-next {
  text-align: right;
}
#addPageSize {
  text-align: right;
}
#minPageSize {
  text-align: left;
}
</style>
