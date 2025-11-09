<template>
  <div class="maid-card p-3 rounded-4 mb-3">
    <div class="mb-3 d-flex align-items-center">
      <label class="form-label me-2 w-auto">Full Name:</label>
      <input
        v-model="name"
        type="text"
        class="form-input flex-grow-1"
        placeholder="Enter name and surname"
        :class="{ 'remaining-pill': accepted }"
      />
    </div>

    <div class="form-check mb-3 d-flex align-items-center">
      <input
        v-model="accepted"
        class="form-check-input custom-checkbox me-2"
        type="checkbox"
        id="acceptedCheck"
      />
      <label
        class="form-check-label"
        for="acceptedCheck"
        style="color: #08182e"
      >
        Accepted
      </label>
    </div>

    <table class="table table-bordered task-table">
      <thead>
        <tr>
          <th>Task</th>
          <th>Done</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <input
              v-model="task.name"
              type="text"
              class="form-input"
              placeholder="Enter task"
              :class="{ 'remaining-pill': accepted }"
            />
          </td>
          <td class="text-center">
            <input
              v-model="task.done"
              type="checkbox"
              class="custom-checkbox"
            />
          </td>
        </tr>
      </tbody>
    </table>

    <button class="btn btn-gold mt-2" @click="addTask">Add Task</button>
  </div>
</template>

<script>
export default {
  name: "MaidOfHonorCard",
  data() {
    return {
      name: "",
      accepted: false,
      tasks: [{ name: "", done: false }],
    };
  },
  methods: {
    addTask() {
      this.tasks.push({ name: "", done: false });
    },
  },
};
</script>

<style scoped>
.maid-card {
  background: rgba(244, 231, 204, 0.95);
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.08);
}

.form-label {
  margin-bottom: 0;
}

.form-input {
  width: 100%;
  padding: 0.5rem 1rem;
  border-radius: 30px;
  border: 1px solid #08182e;
  background: rgba(212, 175, 55, 0.3);
  color: #08182e;
  transition: background 0.3s ease, color 0.3s ease;
}

.remaining-pill {
  background: #d4af37 !important;
  color: #fff !important;
}

.btn-gold {
  background: #08182e;
  border: none;
  color: #d4af37;
  font-weight: 600;
  padding: 0.5rem 1.5rem;
  border-radius: 999px;
  transition: background 0.3s ease;
}

.btn-gold:hover {
  background: #0c274f;
  color: #d4af37;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 1rem;
  background: rgba(244, 231, 204, 0.95);
}

.table th,
.table td {
  border: 1px solid #08182e;
  padding: 0.5rem;
  background: rgba(244, 231, 204, 0.95);
}

.table th {
  color: #08182e;
}

.custom-checkbox {
  width: 18px;
  height: 18px;
  accent-color: #d4af37;
}
</style>
