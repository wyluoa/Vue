<template>
  <div>
    <div style="width: 61.8%;margin: auto">
      <label>Group : </label>
      <select>
        <option v-for="opt of group" :key="opt" :label="opt" :value="opt"></option>
      </select>
    </div>
    <div class="table-container" id="drm-table">
      <hr ref="topline" style="margin: unset;border-color: transparent;" />
      <table>
        <thead>
          <tr>
            <th>checkin_id</th>
            <th>DRM</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item of dataList" :key="item.drm">
            <td>{{ item.checkin_id }}</td>
            <td>{{ item.drm }}</td>
            <td>{{ item.status }}</td>
          </tr>
        </tbody>
      </table>
       <transition name="fade">
        <button v-show="showScrollTopButton" @click="scrollToTop" class=scrollTopButton>TOP</button>
       </transition>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dataList: [],
      showScrollTopButton: false,
      group: []
    }
  },
  created () {
    this.initData()
  },
  mounted() {
    let IO = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        console.log(entry.isIntersecting)
        this.showScrollTopButton = !entry.isIntersecting
      })
    })
    IO.observe(this.$refs['topline'])
  },
  methods: {
    initData () {
      for (let i = 1; i <= 100; i++) {
        this.dataList.push({
          checkin_id: 123000 + i, 
          drm: `T-N03-CL-DR-00${i}`,
          status: i % 2 ? 'success' : 'fail'
        })
      }
      for (let i = 1;i <10 ; i++) {
        this.group.push('group' + i)
      }
    },
    scrollToTop () {
      let a = document.getElementById('drm-table')
      a.scrollTo({ top: 0, behavior: 'smooth' })
    }
  },
}
</script>

<style lang="scss" scoped>
select {
  min-width: 100px;
  padding: 4px;
  background: lemonchiffon;
  border: 1px solid #e8e8e8;
  border-radius: 5px;
  margin-bottom: 16px;
}
.table-container {
  height: 80vh;
  overflow: auto;
  margin: auto;
}
table {
  border-collapse: collapse;
  width: 61.8%;
  margin: auto;
}
th, td {
  border: 1px solid #e8e8e8;
  padding: 8px 16px;
}
tr th {
  background: lightgrey;
  z-index: 2;
}
th {
  position: sticky;
  top: 0;
  background: lightgrey;
  height: 40px;

  &::before {
    content: '';
    display: block;
    background: lightgrey;
    z-index: -1;
    height: 100%;
    width: 100%;
    position: absolute;
    top: -2px;
    left: 0;
  }
}
.scrollTopButton {
  position: absolute;
  bottom: 50px;
  right: 50px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: #f8f8f8;
  border: 1px solid #e8e8e8;
  color: #444;
  cursor: pointer;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.grid {
  display: grid;
  overflow: auto;

  &-row {
    display: grid;
    grid-template-columns: repeat(3, 150px);
    height: 40px;
  }
  &-col {
    border: 3px solid #e8e8e8
  }
}
</style>