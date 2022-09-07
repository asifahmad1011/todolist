<template>
  <div class="holder d-flex align-items-center justify-content-center">
    <div class="container">
      <header class="text-center mb-5">
        <h1 class="display-4">Welcome back, User</h1>
        <p class="fst-italic text-muted">You have {{  data.todos.length  }} tasks coming up in the next days..</p>
      </header>
      <div class="row mx-md-n5">
        <div class="col-lg-8 mx-auto">
          <div class="card rounded border-0 shadow-sm position-relative">
            <div class="card-body p-5">
              <div class="d-flex align-items-center mb-4 pb-4 border-bottom">
                <div class="ms-3">
                  <h4 class="text-uppercase fw-weight-bold mb-0">{{  data.weekday[today]  }}</h4>
                  <p class="text-gray fst-italic mb-0">{{  new Date().toLocaleDateString()  }}</p>
                </div>
              </div>
              <div class="form-check mb-5">
                <input class="form-control mb-2" v-model="data.newTodoName.name" @keyup.enter="addTodo()"
                  placeholder="Todo Name" type="text">

                <textarea class="form-control" v-model="data.newTodoName.descrption" @keyup.enter="addTodo()"
                  placeholder="Description"></textarea>
                <button class="btn btn-primary mt-2 float-sm-end" @click="addTodo()">Add</button>
              </div><br>

              <p class="fst-italic text-muted">You have {{  uncheckedTodos  }} unchecked tasks. </p>

              <div class="form-check mb-2" v-for="(todo, index) in data.todos" :key="todo.id">

                <div class="card" v-if="data.todos.length > 0">
                  <div class="card-body">
                    <ul>
                      <li>
                        <div class="row text-start">
                          <p class="text-start">Created on: {{  data.weekday[new Date(todo.id).getDay()]  }} {{  new
                          Date(todo.id).toLocaleDateString()


                            }}</p>
                          <div class="d-inline-flex align-items-center">
                            <input class="me-2" name="checkbox" type="checkbox" id="flexCheckDefault" @click="changeStatus(index)">
                            <label for="checkbox"> {{  todo.name  }}
                            </label>
                          </div>

                        </div>
                      </li>
                      <li>
                        <div class="row text-start text-justify ms-2">

                          <p class="text-lg-left">{{  todo.descrption  }}</p>
                        </div>

                      </li>
                      <div class="text-end">
                        <li>
                          <button class="btn btn-danger" @click="deleteTodo(index)">
                            Delete
                          </button>
                        </li>
                      </div>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
// @ is an alias to /src
import { ref, reactive, computed } from 'vue'

export default {

  setup() {
    let newTodoName = ref({})
    let todos = ref([])

    let data = reactive({
      newTodoName: { name: "", descrption: "" },
      todos: [{
        id: 1662483341456,
        name: "User Story1", descrption: "The user should able to create tasks .After creation the user should be able to see all created tasks .The user can check a task and see an indicator of how many tasks are still unchecked.",
        unChecked: true
      }, {
        id: 1662483341456,
        name: "User Story2", descrption: "The user should able to create tasks .After creation the user should be able to see all created tasks .The user can check a task and see an indicator of how many tasks are still unchecked.", unChecked: true
      }, {
        id: 1662483341456,
        name: "User Story3", descrption: "The user should able to create tasks .After creation the user should be able to see all created tasks .The user can check a task and see an indicator of how many tasks are still unchecked.", unChecked: true
      }],
      weekday: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
      completed: false,
    })

    let today = computed(() => {
      let date = new Date()
      let day = date.getDay()
      return day
    })

    let uncheckedTodos = computed(() => {
      let count = 0
      for (let index = 0; index < data.todos.length; index++) {
        if (data.todos[index].unChecked) {
          count++
        }
        
      }
      return count
    })


    function addTodo() {
      if ((data.newTodoName.name != "") && (data.newTodoName.descrption != "")) {
        let newTodo = {
          id: Date.now(),
          name: data.newTodoName.name,
          descrption: data.newTodoName.descrption,
          inProgress: true
        }
        data.todos.push(newTodo)
        data.newTodoName.name = ""
        data.newTodoName.descrption = ""
        console.log("clicked...", newTodo)
      } else {

        alert("Please enter todo detail..")

      }
    }

    function deleteTodo(id) {
      data.todos.splice(id, 1)
    }

    function changeStatus(id){
      data.todos[id].unChecked = !data.todos[id].unChecked
      console.log(data.todos[id])
    }

    return {
      newTodoName,
      todos,
      data,
      addTodo,
      deleteTodo,
      changeStatus,
      today,
      uncheckedTodos
    }

  }


}
</script>

<style>
label {
  float: left;
}

.holder {
  background: #f0efe9;
  min-height: 100vh;
}

.rounded {
  border-radius: 1rem !important;
}

.text-gray {
  color: #aaa;
}

input:checked+label {
  text-decoration: line-through;
  color: #adb5bd;
}

.linethrough {
  text-decoration: line-through;
  color: #adb5bd;

}

label {
  cursor: pointer;
  font-size: large;
  font-weight: bold;
}

li {
  list-style: none;
}

</style>