<template>
  <div class="assignment-tracker">
    <h1>Assignment Tracker</h1>
    <div
      v-for="assignment in filteredAssignments"
      :key="assignment.title"
      class="assignment"
      :class="{ completed: assignment.percentComplete === 100 }"
    >
      <h3>{{ assignment.title }}</h3>
      <p>Due Date: {{ assignment.dueDate }}</p>
      <!-- Calculate days remaining -->
      <p>Days Remaining: {{ calculateDaysRemaining(assignment.dueDate) }}</p>
      <p>Percent Complete: {{ assignment.percentComplete }}%</p>
    </div>
    <!-- Toggle button to show/hide completed assignments -->
    <div class="toggle">
      <input type="checkbox" id="showCompleted" v-model="showCompleted" />
      <label for="showCompleted">Show Completed Assignments</label>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      assignments: [],
      showCompleted: false,
    };
  },
  computed: {
    filteredAssignments() {
      if (this.showCompleted) {
        return this.assignments;
      } else {
        return this.assignments.filter(
          (assignment) => assignment.percentComplete < 100
        );
      }
    },
  },
  mounted() {
    this.fetchAssignments();
  },
  methods: {
    fetchAssignments() {
      fetch("assignments.json")
        .then((response) => response.json())
        .then((data) => {
          this.assignments = data.assignments;
        })
        .catch((error) => {
          console.error("Error fetching assignments:", error);
        });
    },
    calculateDaysRemaining(dueDate) {
      const oneDay = 24 * 60 * 60 * 1000;
      const today = new Date();
      const dueDateObj = new Date(dueDate);
      const diffDays = Math.round((dueDateObj - today) / oneDay);
      return diffDays;
    },
  },
};
</script>

<style>
.assignment-tracker {
  max-width: 300px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #74e291;
}
h3 {
  color: #59b4c3;
}
.assignment {
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 20px;
  margin-bottom: 20px;
}

.assignment h3 {
  margin-top: 0;
}

.toggle {
  margin-top: 20px;
}

.toggle label {
  cursor: pointer;
}

.toggle input {
  margin-right: 5px;
}

.completed {
  text-decoration: line-through;
  color: #999;
}
</style>
