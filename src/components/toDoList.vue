<template>
  <div id="app">
    <div class="container"> 
      
    </div>
    <h1>Daily Me</h1>
    <input v-model="newActivity" @keyup.enter="addActivity" placeholder="Add New Schedule" />
    <input type="time" v-model="newTime" />
    <button @click="addActivity">Add</button>

    <ul>
      <li v-for="(activity, index) in filteredActivities" :key="index" class="activity-item">
        <input type="checkbox" v-model="activity.completed" />
        <span :class="{ completed: activity.completed }">
          {{ index + 1 }}.   {{ activity.name }} - {{ activity.time }}
        </span>
        <button @click="removeActivity(index)">Delete</button>
      </li>
    </ul>
    <button @click="filterIncomplete">Show Unfinished Activities</button>

  </div>
</template>

<script>
export default {
  data() {
    return {
      newActivity: '',
      newTime: '',
      newActivity: '',
      activities: [],
      showIncomplete: false,
    };
  },
  computed: {
    filteredActivities() {
      return this.showIncomplete
        ? this.activities.filter(activity => !activity.completed)
        : this.activities;
    },
  },
  methods: {
    addActivity() {
      if (this.newActivity.trim() && this.newTime) {
        this.activities.push({
          name: this.newActivity,
          time: this.newTime,
          completed: false,
        });
        this.newActivity = '';
        this.newTime = '';
        this.saveToLocalStorage();
      }
    },
    removeActivity(index) {
      this.activities.splice(index, 1);
      this.saveToLocalStorage();
    },
    filterIncomplete() {
      this.showIncomplete = !this.showIncomplete;
    },
    saveToLocalStorage() {
      localStorage.setItem('activities', JSON.stringify(this.activities));
    },
  },
  watch: {
    activities: {
      handler() {
        this.saveToLocalStorage();
      },
      deep: true,
    },
  },
  mounted() {
    const savedActivities = localStorage.getItem('activities');
    if (savedActivities) {
      this.activities = JSON.parse(savedActivities);
    }
  },
};

</script>


<style>
body {
  size: 2px;
  font-family: Arial, sans-serif;
  background-color: skyblue;
  color: black;
}
#app {
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
}
h1 {
  margin-top: 20px;
  padding: 20px;
  background-color: lightblue;
}


input {
  padding: 8px;
  margin: 10px 0;
  text-align: center;
}

ul {
  text-decoration: none;
  padding : 10px;
  margin: 10px 0;
}

button {
  font-size: 15px;
  margin-left:10px;
  padding: 6px;
  text-decoration: none;


}

.completed {
  text-decoration: line-through;
  color: gray;
}
input[type="time"] {
  padding: 6px;
  margin-left: 10px;
}

.activity-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: 10px;
  background-color: lightblue;
  border-radius: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
}

.activity-item span {
  flex-grow: 1;
  text-align: left;
  margin-left: 10px;
  word-break: break-word;
}


ul {
  text-align: left;
  padding-left: 0;
}

</style>