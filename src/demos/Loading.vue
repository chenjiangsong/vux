<template>
  <div>
    <group>
      <switch title="Toggle" :value.sync="show1" @change="show1change"></switch>
    </group>
    <loading :show="show1" :text="text1"></loading>
  </div>
</template>

<script>
import { Loading, Group, Switch } from '../components/'

export default {
  components: {
    Loading,
    Group,
    Switch
  },
  data () {
    return {
      show1: false,
      text1: 'Processing'
    }
  },
  methods: {
    show1change: function (val) {
      const _this = this
      if (val) {
        tick(0, function (percent) {
          if (percent === 100) {
            _this.show1 = false
            _this.text1 = 'Start processing'
            return
          }
          _this.text1 = `${percent}% completed`
        })
      }
    }
  }
}

function tick (i, cb) {
  setTimeout(function () {
    i++
    cb(i)
    if (i < 100) {
      tick(i, cb)
    }
  }, 50)
}
</script>
