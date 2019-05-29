<template>
  <div>
    <nav class="navbar navbar-dark bg-dark sc-navbar">
      <div class="container-fluid">
        <div class="navbar-header">
          <a class="navbar-brand" href="#">Software Clash</a>
        </div>
        <ul class="nav navbar-nav navbar-right">
          <li>
            <a href="#" class="sc-username">
              <span class="glyphicon glyphicon-log-in">
                <font-awesome-icon icon="user"/>
              </span>
              {{UserName}}
            </a>
          </li>
        </ul>
      </div>
    </nav>

    <table v-if="auth" class="table sc-table rounded container">
      <caption>
        Spring 19 class schedule
        <p>
          Sectoins with the
          <span class="text-danger">RED</span> text are full and
          <span class="text-danger">cannot be selected!</span> Click on it to request joining.
        </p>
      </caption>
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">1st slot</th>
          <th scope="col">2nd slot</th>
          <th scope="col">3rd slot</th>
          <th scope="col">4th slot</th>
          <th scope="col">5th slot</th>
          <th scope="col">6th slot</th>
        </tr>
      </thead>
      <tbody v-for="(item, day) in schedule" :key="day">
        <tr>
          <th>{{days[day]}}</th>
          <td v-for="(subitem, slot) in item" :key="slot">
            <ul v-for="(subsubitem, lecture) in subitem" :key="lecture">
              <li
                v-if="subsubitem.valid"
                :class="subsubitem.class"
                :id="subsubitem.id"
                @click="selectClass(day,slot,lecture)"
              >{{subsubitem.name}}</li>
              <li
                v-if="!subsubitem.valid"
                class="text-danger"
                :id="subsubitem.id"
                @click="requestClass(day,slot,lecture)"
              >{{subsubitem.name}}</li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="auth" class="container">
      <button class="btn btn-primary float-right" @click="submitFinal()">Submit</button>
    </div>
    <div v-if="!auth">
      <h1>You don`t have access right now to the schedule, please log in later in your appointed time.</h1>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import VueToast from "vue-toast-notification";
import "vue-toast-notification/dist/index.css";
import axios from "axios";
import VueAxios from "vue-axios";
import Axios from "axios";
Vue.use(VueToast);
Vue.use(VueAxios, axios);
export default {
  data() {
    return {
      UserName: "Mohamed Mostafa Mostafa",
      days: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday"],
      schedule: [],
      courses: [],
      auth: false,
      student: ""
    };
  },
  mounted() {
    console.clear();
    this.student = localStorage.student;
    this.authenticateTime();
    this.getSchedule();
    this.receiveNotificatoin();
    console.clear();
  },
  watch: {
    student(newStudnet) {
      localStorage.student = newStudnet;
    }
  },
  methods: {
    receiveNotificatoin() {
      if (this.auth) {
        Vue.$toast.info(
          "Some students are asking to join CS201-4 that you are enrolled in and it`s full. so, if you can recreate your schedule and register in another class would be appreciated.",
          { duration: 20000, position: "top-left" }
        );
      }
    },
    authenticateTime() {
      Axios.get(
        "http://localhost:5000/api/v1/auth/" + this.student,
        {
          headers: {
            "Access-Control-Allow-Origin": "*"
          }
        },
        { crossdomain: true }
      )
        .then(res => {
          console.log("res", res);
          this.auth = res.data.success;
        })
        .catch(err => {
        });
    },
    getSchedule() {
      Axios.get(
        "http://localhost:5000/api/v1/schedule",
        {
          headers: {
            "Access-Control-Allow-Origin": "*"
          }
        },
        { crossdomain: true }
      )
        .then(res => {
          console.log("res", res);
          this.schedule = res.data.data;
        })
        .catch(err => {
        });
    },
    arrayRemove(arr, value) {
      return arr.filter(function(ele) {
        return ele != value;
      });
    },
    selectClass(day, slot, lecture) {
      if (this.schedule[day][slot][lecture].class == "inactiveClass") {
        this.schedule[day][slot][lecture].class = "activeClass";
        this.courses.push(this.schedule[day][slot][lecture].id);
      } else if (this.schedule[day][slot][lecture].class == "activeClass") {
        this.schedule[day][slot][lecture].class = "inactiveClass";
        this.courses.pop(this.schedule[day][slot][lecture].id);
        this.arrayRemove(this.courses, this.schedule[day][slot][lecture].id);
      }
      console.log("this", day, slot, lecture);
      console.log("day", this.schedule[day][slot][lecture]);
    },
    requestClass(day, slot, lecture) {
      this.schedule[day][slot][lecture].valid = true;
      this.schedule[day][slot][lecture].class = "text-primary";
      Vue.$toast.success("Request Sent Succefully!", { position: "top-left" });
    },
    submitFinal() {
      if (this.student == "" || this.student == undefined) {
        Vue.$toast.error("Student Not Found", { position: "top-left" });
      } else if (this.courses.length < 9) {
        Vue.$toast.error("please select at least 9 slots", {
          position: "top-left"
        });
      } else {
        if (confirm("Confirm Submissoin ?")) {
          axios
            .post("http://localhost:5000/api/v1/courses", {
              studentID: this.student,
              courses: this.courses
            })
            .then(res => {
              console.log("res", res);
            })
            .catch(err => {
            });
          Vue.$toast.success("Submitted successfully!", {
            position: "top-left"
          });
          this.schedule.forEach(day => {
            day.forEach(slot => {
              slot.forEach(lecture => {
                lecture.class = "inactiveClass";
              });
            });
          });
        }
      }
    }
  }
};
</script>


<style scoped>
.sc-username {
  color: #ffffff;
}
.sc-table {
  margin-top: 60px;
}
li {
  list-style: none;
  cursor: pointer;
}
.activeClass {
  background-color: yellow;
}
</style>
