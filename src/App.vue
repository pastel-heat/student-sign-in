<template>
  <!-- build app out of components, and bind component props to data from app.vue -->
  <NewStudentForm v-on:student-added="newStudentAdded"></NewStudentForm>

  <StudentTable v-bind:students="students" 
  v-on:presence-changed='presenceChanged'
  v-on:delete-student="deleteStudent"
  ></StudentTable>

  <StudentMessage v-bind:student="mostRecentStudent"></StudentMessage>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'

export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },
  data() {
    return {
      students: [],
      mostRecentStudent: {}
    }
  },
  methods: {
    newStudentAdded(student) {
      this.students.push(student)
      this.students.sort((s1, s2) => {
        if (s1.name.toLowerCase() > s2.name.toLowerCase()) {
            return 1
        }
        if (s1.name.toLowerCase() < s2.name.toLowerCase()) {
            return -1
        }
        return 0
      })
    },
    presenceChanged(student, present) {
      // find and update presence of student specified by presence-changed event

      // identify student to update
      let updateStudent = this.students.find( function(s) {
        return(s.name === student.name && s.starID === student.starID)
      })

      // update student presence and update mostRecentStudent for studentmessage component
      if (updateStudent) {
        updateStudent.present = present
        this.mostRecentStudent = updateStudent
      }
    },
    deleteStudent(student) {
      // filter deleted student out of array
      this.students = this.students.filter( function(s) {
        return (s != student) 
      })

      // clear most recent student
      this.mostRecentStudent = {}
    }
  }
}
</script>

<style>

@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css"; 

img {
  height: 30px;
}
</style>
