<template>
  <el-aside width="78px" :class="['aside', { 'draggable': asideDraggable }]">
    <div class="aside-inner">
      <mo-logo-mini />
      <ul class="menu top-menu">
        <li @click="nav('/task')" class="non-draggable">
          <mo-icon name="menu-task" width="20" height="20" />
        </li>
        <li @click="showAddTask()" class="non-draggable">
          <mo-icon name="menu-add" width="20" height="20" />
        </li>
      </ul>
      <ul class="menu bottom-menu">
        <li @click="nav('/preference')" class="non-draggable">
          <mo-icon name="menu-preference" width="20" height="20" />
        </li>
        <li @click="showAboutPanel" class="non-draggable">
          <mo-icon name="menu-about" width="20" height="20" />
        </li>
      </ul>
    </div>
  </el-aside>
</template>

<script>
  import is from 'electron-is'
  import { mapState } from 'vuex'
  import LogoMini from '@/components/Logo/LogoMini'
  import '@/components/Icons/menu-task'
  import '@/components/Icons/menu-add'
  import '@/components/Icons/menu-preference'
  import '@/components/Icons/menu-about'

  export default {
    name: 'mo-aside',
    components: {
      [LogoMini.name]: LogoMini
    },
    computed: {
      ...mapState('app', {
        currentPage: state => state.currentPage
      }),
      asideDraggable: function () {
        return is.macOS()
      }
    },
    methods: {
      showAddTask (taskType = 'uri') {
        this.$store.dispatch('app/showAddTaskDialog', taskType)
      },
      showAboutPanel () {
        // if (is.renderer()) {
        //   this.$electron.ipcRenderer.send('command', 'application:about')
        // } else {
        this.$store.dispatch('app/showAboutPanel')
        // }
      },
      nav (page) {
        this.$router.push({
          path: page
        })
      }
    }
  }
</script>

<style lang="scss">
  .aside-inner {
    display: flex;
    height: 100%;
    flex-flow: column;
  }
  .logo-mini {
    margin-top: 40px;
  }
  .menu {
    list-style: none;
    padding: 0;
    margin: 0 auto;
    user-select: none;
    cursor: default;
    > li {
      width: 32px;
      height: 32px;
      margin-top: 24px;
      cursor: pointer;
      border-radius: 16px;
      transition: background-color 0.25s;
      &:hover {
        background-color: rgba(255, 255, 255, 0.15);
      }
    }
    svg {
      padding: 6px;
      color: #fff;
    }
  }
  .top-menu {
    flex: 1;
  }
  .bottom-menu {
    margin-bottom: 24px;
  }
</style>
