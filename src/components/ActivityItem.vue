<template>
  <router-link :to="{ name: 'detail', params: {id: activity.id} }">
    <article class="bg-white h-[200px] rounded-lg p-5 flex flex-col justify-between shadow-lg">
      <h1 data-cy="activity-item-title" class="text-xl font-bold">{{activity.title}}</h1>
      <h3 data-cy="activity-item-date" class="flex justify-between items-center">
        {{convertCreatedDate}}
        <span data-cy="activity-item-delete-button" @click.prevent="handleDelete({ id: activity.id, title: activity.title })">
          <font-awesome-icon icon="fa-solid fa-trash" class="mr-2 hover:text-red-600 fa-lg" />
        </span>
      </h3>
    </article>
  </router-link>
</template>

<script>
import moment from 'moment'

export default {
  name: "ActivityItem",
  props: {
    activity: {
      type: Object,
      default: () => {}
    }
  },
  computed: {
    convertCreatedDate() {
      return `${moment(this.activity.created_at).format('DD MMMM YYYY')}`
    }
  },
  methods: {
    handleDelete(activity) {
      this.$emit('delete-activity', activity);
    }
  }
}
</script>

<style scoped>

</style>
