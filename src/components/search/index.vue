<template>
  <div class="vux-search-box">
    <div class="weui_search_bar" id="search_bar" :class="{weui_search_focusing: !isCancel}">
      <form class="weui_search_outer">
        <div class="vux-search-mask" @click="touch" v-show="!isFixed"></div>
        <div class="weui_search_inner">
          <i class="weui_icon_search"></i>
          <input type="text" class="weui_search_input" id="search_input" placeholder="{{placeholder}}" autocomplete="off" required v-model="value" v-el:input/>
          <a href="javascript:" class="weui_icon_clear" id="search_clear" @click="clear"></a>
        </div>
        <label for="search_input" class="weui_search_text" id="search_text">
          <i class="weui_icon_search"></i>
          <span>{{placeholder}}</span>
        </label>
      </form>
      <a href="javascript:" class="weui_search_cancel" id="search_cancel" @click="cancel">{{cancelText}}</a>
    </div>
    <div class="weui_cells weui_cells_access search_show" id="search_show" v-show="isFixed && results.length && value">
      <div class="weui_cell" v-for="item in results" @click="handleResultClick(item)">
        <div class="weui_cell_bd weui_cell_primary">
          <p>{{item.title}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    placeholder: {
      type: String,
      default: 'Search'
    },
    cancelText: {
      type: String,
      default: 'cancel'
    },
    value: {
      type: String,
      twoWay: true,
      default: ''
    },
    results: {
      type: Array,
      default: function () {
        return []
      }
    },
    autoFixed: {
      type: Boolean,
      default: true
    }
  },
  methods: {
    clear: function () {
      this.value = ''
      this.isFocus = true
      this.setFocus()
    },
    cancel: function () {
      this.value = ''
      this.isCancel = true
      this.isFixed = false
    },
    handleResultClick: function (item) {
      this.$dispatch('result-click', item)
      this.isCancel = true
      this.isFixed = false
    },
    touch: function () {
      this.isCancel = false
      if (this.autoFixed) {
        this.isFixed = true
      }
    },
    setFocus: function () {
      this.$els.input.focus()
    }
  },
  data () {
    return {
      isCancel: true,
      isFocus: false,
      isFixed: false
    }
  },
  watch: {
    isFixed: function (val) {
      if (val === true) {
        this.$el.classList.add('vux-search-fixed')
        this.setFocus()
        console.log('set focus')
        this.isFocus = true
      } else {
        this.$el.classList.remove('vux-search-fixed')
      }
    },
    value: function (val) {
      this.$emit('change', this.value)
    }
  }
}
</script>

<style>
.vux-search-fixed {
  position: fixed;
  height: 100%;
  left: 0;
  top: 0;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(5px);
}
.vux-search-box {
  width: 100%;
}
.search_show {
  margin-top: 0;
}
.vux-search-mask {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 5;
}
</style>
