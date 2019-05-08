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
    <table class="table sc-table rounded container">
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
    <div class="container">
      <button class="btn btn-primary float-right" @click="submitFinal()">Submit</button>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import VueToast from "vue-toast-notification";
import "vue-toast-notification/dist/index.css";
Vue.use(VueToast);
export default {
  created() {
    this.receiveNotificatoin();
  },
  data() {
    return {
      UserName: "Mohamed Mostafa Mostafa",
      days: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday"],
      schedule: [
        [
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: false, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ]
        ],
        [
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ]
        ],
        [
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ]
        ],
        [
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ]
        ],
        [
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ],
          [
            { name: "ECEN 203", id: "12", valid: true, class: "inactiveClass" },
            { name: "CSES 203", id: "13", valid: true, class: "inactiveClass" }
          ]
        ]
      ]
    };
  },
  methods: {
    receiveNotificatoin() {
      Vue.$toast.error(
        "Some students are asking to join CS201-4 that you are enrolled in and it`s full. so, if you can recreate your schedule and register in another class would be appreciated.",
        { duration: 20000, position: "top-left" }
      );
    },
    selectClass(day, slot, lecture) {
      if (this.schedule[day][slot][lecture].class == "inactiveClass") {
        this.schedule[day][slot][lecture].class = "activeClass";
      } else if (this.schedule[day][slot][lecture].class == "activeClass") {
        this.schedule[day][slot][lecture].class = "inactiveClass";
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
      Vue.$toast.success("Submitted successfully!", { position: "top-left" });
      this.schedule.forEach(day => {
        day.forEach(slot => {
          slot.forEach(lecture => {
            lecture.class = "inactiveClass";
          });
        });
      });
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
