<template>
  <div id="app">
    <h1>Books+Reviews</h1>
    <div class="dropdown-container">
      <div class="dropdown-left">
        <label for="listsNames">Choose a New York Times Bestseller List:</label>
        <el-dropdown
          trigger="click"
          @command="handleListCommand">
          <el-button type="primary">
            New York Times Bestseller Lists<i class="el-icon-arrow-down el-icon--right"></i>
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item
              v-for="list in listsNames"
              :command="list">
              {{ list.display_name }}
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
      <div class="dropdown-right">
        <label for="dates">Choose a date:</label>
        <el-dropdown
          trigger="click"
          @command="handleDateCommand">
          <el-button type="primary">
            Dates<i class="el-icon-arrow-down el-icon--right"></i>
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="current">Current</el-dropdown-item>
            <el-dropdown-item command="1">One year ago</el-dropdown-item>
            <el-dropdown-item command="2">Two years ago</el-dropdown-item>
            <el-dropdown-item command="3">Three years ago</el-dropdown-item>
            <el-dropdown-item command="4">Four years ago</el-dropdown-item>
            <el-dropdown-item command="5">Five years ago</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </div>
    <div class="bestseller-list" v-if="bestsellerList">
      <h1>{{ bestsellerList.display_name }}</h1>
      <ul v-for="book in bestsellerList.books">
        <li>{{book.title}} -- {{book.author}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  name: 'app',
  data () {
    return {
      listsNames: [],
      selectedList: null,
      selectedDate: null,
      bestsellerList: null
    }
  },
  watch: {
    selectedList (newList) {
      if (newList && this.selectedDate) {
        this.fetchBestsellerList(newList, this.selectedDate);
      }
    },
    selectedDate (newDate) {
      if (newDate && this.selectedList) {
        this.fetchBestsellerList(this.selectedList, newDate);
      }
    }
  },
  created () {
    this.fetchListsNames();
  },
  methods: {
    fetchListsNames () {
      const axios = require('axios');
      axios.get('https://api.nytimes.com/svc/books/v3/lists/names.json?api-key=gGwCvlIrTfzGUIM8GkxDIVo37GhWM1dc')
        .then(response => {
          this.listsNames = response.data.results;
      });
    },
    fetchBestsellerList (listName, date) {
      const formattedDate = this.formatDate(date);
      const axios = require('axios');
      axios.get(`https://api.nytimes.com/svc/books/v3/lists/${formattedDate}/${listName}.json?api-key=gGwCvlIrTfzGUIM8GkxDIVo37GhWM1dc`)
        .then(response => {
          this.bestsellerList = response.data.results;
      });
    },
    formatDate (date) {
      if (date === 'current') {
        return moment().format('YYYY-MM-DD');
      } else {
        return moment().subtract(date, 'years').format('YYYY-MM-DD');
      }
    },
    handleListCommand (list) {
      this.selectedList = list.list_name_encoded;
    },
    handleDateCommand (date) {
      let numberDate;
      if (date !== 'current') {
        numberDate = parseInt(date, 10);
      }
      this.selectedDate = numberDate || date;
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.el-dropdown-link {
  cursor: pointer;
  color: #409EFF;
}
.el-icon-arrow-down {
  font-size: 12px;
}

.dropdown-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: 15px;
}

.dropdown-left {
  padding: 15px;
  margin: 15px;
}

.dropdown-right {
  padding: 15px;
  margin: 15px;
}

.bestseller-list {
  padding: 15px;
  margin: 15px;
}
</style>
