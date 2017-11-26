<template>
  <div class="page-container">
    <md-app md-waterfall md-mode="flexible">
      <md-app-toolbar class="md-large md-primary">
        <div class="md-toolbar-row">
          <div class="md-toolbar-section-start">
            <md-button class="md-icon-button" @click="menuVisible = !menuVisible">
              <md-icon>menu</md-icon>
            </md-button>
          </div>

          <div class="md-toolbar-section-end">
            <md-button class="md-icon-button">
              <md-icon>more_vert</md-icon>
            </md-button>
          </div>
        </div>

        <div class="md-toolbar-row md-toolbar-offset">
          <span class="md-display-1">今日清单</span>
          <md-progress-bar v-if="items.length > 0" md-mode="determinate" :md-value="percentage"></md-progress-bar>
        </div>
      </md-app-toolbar>

      <md-app-drawer :md-active.sync="menuVisible">
        <md-toolbar class="md-transparent" md-elevation="0">Navigation</md-toolbar>

        <md-list>
          <md-list-item>
            <md-icon>move_to_inbox</md-icon>
            <span class="md-list-item-text">Inbox</span>
          </md-list-item>

          <md-list-item>
            <md-icon>send</md-icon>
            <span class="md-list-item-text">Sent Mail</span>
          </md-list-item>

          <md-list-item>
            <md-icon>delete</md-icon>
            <span class="md-list-item-text">Trash</span>
          </md-list-item>

          <md-list-item>
            <md-icon>error</md-icon>
            <span class="md-list-item-text">Spam</span>
          </md-list-item>
        </md-list>
      </md-app-drawer>

      <md-app-content>
        <div class="list">
          <div class="cell" v-for="(item, index) in items" :key="index">
            <div class="content">
              <span class="title">{{ item.title }}</span>
            </div>
            <div class="action">
              <el-checkbox v-model="item.completed"></el-checkbox>
            </div>
          </div>
        </div>
        <md-button class="md-icon-button md-raised" @click="add">
          <md-icon>add</md-icon>
        </md-button>
      </md-app-content>
    </md-app>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      items: [],
      menuVisible: false
    }
  },
  computed: {
    percentage: function () {
      if (!this.items.length > 0) {
        return 0
      }
      var p = 0
      for (var i = 0; i < this.items.length; i++) {
        if (this.items[i].completed) {
          p = p + 1
        }
      }
      return Math.ceil(p / this.items.length * 100)
    }
  },
  methods: {
    add: function () {
      this.$prompt('力求简短明确', '添加一条', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        inputPattern: /\S+/,
        inputErrorMessage: '格式不正确'
      }).then(({ value }) => {
        if (value) {
          this.items.push({title: value, completed: false })
          this.$message({
            type: 'success',
            message: '成功添加: ' + value
          })
        }
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '取消输入'
        })
      })
    }
  },
  mounted: function () {
    for (var i = 0; i < 0; i++) {
      this.items.push({title: Math.random() * 1000, completed: Math.random() > 0.5})
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  scoped>
  .page-container {
    flex: 1;
  }
  .md-app {
    border: 1px solid red);
    flex: 1;
  }

  .md-app-toolbar {
    height: 196px;
  }

  // Demo purposes only
  .md-drawer {
    width: 230px;
    max-width: calc(100vw - 125px);
  }
</style>
<!--<style scoped>-->

<!--.container {-->
  <!--display: flex;-->
  <!--flex: 1;-->
  <!--flex-direction: column;-->
<!--}-->

<!--.header {-->
  <!--background-color: white;-->
  <!--padding: 15px;-->
  <!--display: flex;-->
  <!--flex-direction: column;-->
<!--}-->

<!--.title {-->
  <!--font-size: 24px;-->
<!--}-->

<!--.list {-->
  <!--flex: 1;-->
  <!--overflow: auto;-->
<!--}-->

<!--.add {-->
  <!--margin: 15px;-->
<!--}-->

<!--.cell {-->
  <!--display: flex;-->
  <!--flex-direction: row;-->
<!--}-->

<!--.cell .content {-->
  <!--flex: 1;-->
  <!--padding: 15px;-->
<!--}-->

<!--.cell .action {-->
  <!--width: 50px;-->
  <!--display: flex;-->
  <!--justify-content: center;-->
  <!--align-items: center;-->
<!--}-->

<!--h1, h2 {-->
  <!--font-weight: normal;-->
<!--}-->
<!--ul {-->
  <!--list-style-type: none;-->
  <!--padding: 0;-->
<!--}-->
<!--li {-->
  <!--display: inline-block;-->
  <!--margin: 0 10px;-->
<!--}-->
<!--a {-->
  <!--color: #42b983;-->
<!--}-->
<!--</style>-->
