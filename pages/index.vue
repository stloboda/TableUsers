<template>
  <div>
    <div class="container">
      <h1>Test task</h1>
      <table class="cross">
        <thead>
        <tr>
          <th @click="UserSort">Name</th>
          <th @click="UserSortedTwo">Username</th>
          <th @click="UserSort">Email</th>
          <th @click="UserSort">Website</th>
        </tr>
        </thead>
        <tbody>
        <tr @click="ShowUserDetails" v-for="user in users">
          <td>{{user.name}}</td>
          <td>{{user.username}}</td>
          <td>{{user.email}}</td>
          <td>{{user.website}}</td>
        </tr>
        </tbody>
      </table>

    </div>
    <div class="b-popup" v-if="ModalShow">
      <div class="b-popup-content">
        <div class="text-right">
          <button @click="onClose" class="btn btn-outline-secondary btn-sm">X</button>
          <table>
            <tr>
              <th>Street</th>
              <th>City</th>
              <th>Zipcode</th>
            </tr>
            <tr>
              <td>{{UserDetails.address.street}}</td>
              <td>{{UserDetails.address.city}}</td>
              <td>{{UserDetails.address.zipcode}}</td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import ModalWindow from '../components/ModalWindow'

  export default {
    name: "User",
    components: {ModalWindow},
    data: () => ({
      users: [],
      ModalShow: false,
      UserDetails: undefined,
      SortedUsers: '',
      SortedReverse: true,
      SortedUserName: true
    }),
    async asyncData({$axios}) {
      let users = [];
      try {
        users = await $axios.$get('https://jsonplaceholder.typicode.com/users');
        console.log(users)
      } catch (e) {
        console.log(e);
      }
      return {users}
    },
    methods: {
      onClose() {
        this.ModalShow = this.ModalShow ? false : true;

      },
      ShowUserDetails(Event) {
        this.users.forEach(Item => {
          if (Event.target.parentElement.cells [1].innerHTML === Item.username) this.UserDetails = Item;
        });
        this.ModalShow = this.ModalShow ? false : true;
      },
      UserSort() {
        this.SortedReverse = this.SortedReverse ? false : true;
        if (this.SortedReverse) {
          this.users.sort((a, b) => {
            if (a.name < b.name) return -1;
            if (a.name > b.name) return 1;
            if (!(a.name < b.name && a.name > b.name)) return 0;
          });
        } else {
          this.users.sort((a, b) => {
            if (a.name > b.name) return -1;
            if (a.name < b.name) return 1;
            if (!(a.name > b.name && a.name < b.name)) return 0;
          });
        }
      },
      UserSortedTwo() {
        this.SortedUserName = this.SortedUserName ? false : true;
        if (this.SortedUserName) {
          this.users.sort((a, b) => {
            if (a.username < b.username) return -1;
            if (a.username > b.username) return 1;
            if (!(a.username < b.username && a.username > b.username)) return 0;
          });
        } else {
          this.users.sort((a, b) => {
            if (a.username > b.username) return -1;
            if (a.username < b.username) return 1;
            if (!(a.username > b.username && a.username < b.username)) return 0;
          });
          this.users.sort((a, b) => {
            if (a.email > b.email) return -1;
            if (a.email < b.email) return 1;
            if (!(a.email > b.email && a.email < b.email)) return 0;
          });
        }
      },
    },
  }


</script>

<style>
  table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  thead th:hover {
    background: #c5c5c5;
  }

  td, th {
    border: 0 solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }

  * {
    font-family: Areal;
  }

  tbody tr:hover {
    background: #c5c5c5;
  }

  .b-popup {
    width: 100%;
    height: 2000px;
    background-color: rgba(0, 0, 0, 0.5);
    overflow: hidden;
    position: fixed;
    top: 0px;
  }

  .b-popup .b-popup-content {
    margin: 10% auto 0px auto;
    width: 500px;
    height: 150px;
    padding: 10px;
    background-color: #c5c5c5;
    border-radius: 5px;
    box-shadow: 0px 0px 10px #000;
  }

  .cross {
    cursor: pointer;
  }


</style>
