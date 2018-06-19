<template>
  <div>
    <pull-to
      :top-load-method="refresh"
      :bottom-load-method="loadmore"
      @top-state-change="stateChange"
      @bottom-state-change="stateChange">
      <template slot="top-block" slot-scope="props">
        <div class="top-load-wrapper">
          <svg class="icon"
               :class="{
                  'icon-arrow': props.state === 'trigger',
                  'icon-loading': props.state === 'loading'
               }"
               aria-hidden="true">
            <use :xlink:href="iconLink"></use>
          </svg>
          {{ props.stateText }}
        </div>
      </template>
      <ul class="list">
        <li v-for="item in state.data">
          <p>
            <i :class="['fa fa-credit-card-alt fa-lg', item.level]"></i>
            &nbsp;&nbsp;姓名:&nbsp;{{ item.name }}
            &nbsp;&nbsp;年龄&nbsp;:{{ item.age }}
            &nbsp;&nbsp;性别&nbsp;:{{ item.gender }}
          </p>
          <TaskProgress :progress=item.progress />
        </li>
      </ul>
      <template slot="bottom-block" slot-scope="props">
        <div class="bottom-load-wrapper">
          <svg class="icon"
            :class="{
              'icon-arrow': props.state === 'trigger',
              'icon-loading': props.state === 'loading'
            }"
            aria-hidden="true">
            <use :xlink:href="iconLink"></use>
          </svg>
          {{ props.stateText }}
        </div>
      </template>
    </pull-to>
  </div>
</template>

<script>
import PullTo from 'vue-pull-to'
import TaskProgress from '@/components/TaskProgress'
import {mapState, mapActions} from 'vuex'

export default {
  name: 'task',
  components: {
    PullTo,
    TaskProgress
  },
  data() {
    return {
      dataList: [{
        name: '李三',
        age: 24,
        gender: true,
        progress: 1
      }, {
        name: '张晓',
        age: 21,
        gender: false,
        progress: 2
      }],
      iconLink: ''
    };
  },
  methods: {
    refresh(loaded) {
      this.refreshTask({
        params: null,
        loaded: loaded
      })
    },
    loadmore(loaded) {
      this.loadMoreTask({
        params: null,
        loaded: loaded
      })
    },
    stateChange(state) {
      if (state === 'pull' || state === 'trigger') {
        this.iconLink = '#icon-arrow-bottom'
      } else if (state === 'loading') {
        this.iconLink = '#icon-loading'
      } else if (state === 'loaded-done') {
        this.iconLink = '#icon-finish'
      }
    },
    ...mapActions([
      'loadMoreTask',
      'refreshTask'
    ])
  },
  computed: {
    ...mapState({
      state: function (state) {
        return {
          data: state.task.data
        }
      }
    })
  },
  beforeMount () {
    this.refreshTask({
      params: null,
      loaded: null
    });
  }
}
</script>

<style scoped rel="stylesheet/less" lang="less">
  @import "../less/vue-pull-to.less";
  @import "../less/common.less";
  .list li {
    overflow: hidden;
    height: auto;
    line-height: 12px;
    border-top: 1px solid #eee;
    padding: 10px;
    p {
      color: #333;
      font-size: 14px;
      line-height: 21px;
    }
    .task-progress {
      margin-top: 15px;
    }
    .fa-male, .fa-female {
      color: lightGray;
      font-size: 18px;
    }
  }
  .list li:nth-child(even) {
    background-color: white;
  }
  .list li:last-child {
    border-bottom: 1px solid #eee;
  }
  .router-view-search-customer-detail {
    position: absolute;
    top: 0;
    bottom: 0;
    display:block;
    width: 100%;
    overflow: hidden;
    background-color: white;
  }
</style>
