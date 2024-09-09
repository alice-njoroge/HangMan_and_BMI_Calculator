<script setup>
import {computed, ref} from "vue";

const props = defineProps({
  users: {
    type: Array,
    default: () => [],
    required: true
  }
});
const currentUser = ref(null);

const handleClick = (event, user) => {
  event.preventDefault();
  currentUser.value = user;
}
const clonedUsers = ref(JSON.parse(JSON.stringify(props.users))); // structured clone is not working for me

const handleSorting = (sortingDirection) => {
  if (sortingDirection === 'descending'){
    return clonedUsers.value.sort((a, b) => b.username.localeCompare(a.username)); //I intentionally want to modify the clonedObject
  }
  return clonedUsers.value.sort((a, b) => a.username.localeCompare(b.username));

};

</script>

<template>
  <section class="user-list">
    <nav class="user-list__nav">
      <p> {{ users.length }} Users</p>
      <button class="user-list__nav-filter" @click="handleSorting">Asc ⇧</button>
      <button class="user-list__nav-filter user-list__nav-filter--active" @click="handleSorting('descending')">
        Desc ⇩
      </button>
      <a v-for="user in clonedUsers"
         @click="handleClick($event, user)"
         :key="user.id" href="/"
         class="user-list__nav-item"
         :class="{'user-list__nav-item--active' : user.id === currentUser?.id}"
      >{{ user.username }}
      </a>
    </nav>
    <div class="user-list__details">
      <h2 class="user-list__details-hl">Details</h2>
      <dl v-if="currentUser">
        <dt>name:</dt>
        <dd>{{ currentUser.name }}</dd>

        <dt>email:</dt>
        <dd>{{ currentUser.email }}</dd>

        <dt>phone:</dt>
        <dd> {{ currentUser.phone }}</dd>

        <dt>company:</dt>
        <dd>{{ currentUser.company.name }}</dd>
      </dl>
      <p v-else class="user-list__details-hint">
        Es wurde kein Nutzer gefunden / ausgewählt
      </p>
    </div>
  </section>
</template>

<style scoped>
* {
  padding: 0;
  margin: 0;
}

.user-list {
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  width: 80%;
  margin: 0 auto;
  border: 3px solid #00a8ff;
  border-radius: 5px;
}

.user-list__nav {
  width: 40%;
  padding: 1rem;
}

.user-list__nav-filter {
  text-transform: uppercase;
  font-size: 0.7rem;
  line-height: 1;
  color: #00a8ff;
  border: 2px solid #00a8ff;
  background-color: transparent;
  padding: 2px;
  margin-right: 5px;
  margin-bottom: 15px;
}

.user-list__nav-filter--active {
  color: #fff;
  border-color: transparent;
  background-color: #00a8ff;
}

.user-list__nav-item {
  display: block;
  text-decoration: none;
  color: #00a8ff;
  padding: 5px;
  border: 2px solid transparent;
  border-radius: 3px;
  margin-bottom: 2px;
}

.user-list__nav-item:hover {
  border-color: #00a8ff;
}

.user-list__nav-item--active {
  background-color: #00a8ff;
  color: #fff;
  border-color: transparent;
}

.user-list__details {
  width: 60%;
  padding: 1rem;
  color: #192a56;
}

.user-list__details-hl {
  color: #00a8ff;
}

.user-list__details dd {
  margin-bottom: 1rem;
}

.user-list__details-hint {
  border: 2px solid #e84118;
  border-radius: 5px;
  color: #e84118;
  padding: 0.5rem;
}
</style>
