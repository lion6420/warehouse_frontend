<template>
  <div :class="$style.singleSelect" :style="{width:width}" :id="'single-select_' + _uid.toString()">
    <div :class="$style.inputArea" @mouseleave="hideRemoveIcon" :id="'input_container_' + _uid.toString()">
      <div :class="$style.addonBefore">
        <slot name="addonBefore"></slot>
      </div>
      <input
        :class="$style.inputText"
        :placeholder="placeholder"
        :value="value"
        @input="$emit('input', $event.target.value)"
        @mouseover="showRemoveIcon('inputText')"
        :id="'inputArea_' + _uid.toString()" />
      <div
        :class="$style.removeIcon"
        :id="'removeIcon_' + _uid.toString()"
        @mouseover="showRemoveIcon('removeIcon')">
        <span class="fas fa-times-circle" @mouseover="showRemoveIcon('removeIcon')" @click="removeFunction"></span>
      </div>
    </div>
    <div :class="$style.optionsArea" v-if="expand_options" :id="'optionsArea_' + _uid.toString()" :style="{width:width}">
      <div :class="$style.option" v-for="(option,o_index) in options_show" :key="o_index" @click="selectFunction(option)"
        :style="option && option.disabled ? {color: '#828282', cursor: 'not-allowed'}:{}">
        <span :class="$style.optionText">{{option.label ? option.label:option}}</span>
      </div>
      <div :class="$style.noOptions" v-if="options.length === 0">
        <slot name="empty">
          <div :class="$style.noDataIcon">
            <span class="fas fa-folder-open"></span>
          </div>
          <span :class="$style.noDataText">No Data</span>
        </slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      require: true,
      default() {
        return []
      }
    },
    value: {
      type: String,
      require: true,
      default() {
        return ''
      }
    },
    width: {
      type: String,
      require: false,
      default() {
        return '200px'
      }
    },
    placeholder: {
      type: String,
      require: false,
      default() {
        return ''
      }
    },
    filter_disabled: {
      type: Boolean,
      require: false,
      default() {
        return false
      }
    }
  },
  data() {
    return {
      options_show: [],
      expand_options: false,
    }
  },
  created() {
    this.filterOptions()
  },
  mounted() {
    var self = this
    this.clickEvent = function(evt) {
      const clickTarget = evt.target
      if (clickTarget.id === 'inputArea_' + self._uid.toString()) {
        self.openOptionsArea()
      }
      else self.closeOptionsArea()
    }
    this.scrollEvent = function(evt) {
      if (evt.target.id === ('optionsArea_' + self._uid.toString())) return
      self.closeOptionsArea()
    }
    document.addEventListener('click', this.clickEvent)
    document.addEventListener('scroll', this.scrollEvent, true)
  },
  methods: {
    openOptionsArea() {
      this.expand_options = true
      var self = this
      // after DOM rendered
      this.$nextTick(function() {
        let DOM = document.getElementById('optionsArea_' + self._uid.toString())
        DOM.style.display = 'block'
        DOM.style.maxHeight = '200px'
        
        let position = self.getPosition()
        DOM.style.left = position.x + 'px'
        DOM.style.top = position.y + 'px'
      })
    },
    closeOptionsArea() {
      if (!this.expand_options) return
      var DOM = document.getElementById('optionsArea_' + this._uid.toString())
      DOM.style.display = 'none'
      DOM.style.maxHeight = '0px'
      this.expand_options = false
    },
    getPosition() {
      let x, y
      const DOM = document.getElementById('input_container_' + this._uid.toString())

      x = DOM.getBoundingClientRect().left + window.scrollX
      y = DOM.getBoundingClientRect().top + window.scrollY + DOM.offsetHeight + 1

      return {x:x, y:y}
    },
    showRemoveIcon(id) {
      const DOM = document.getElementById('removeIcon_' + this._uid.toString())
      DOM.style.display = 'block'
      if (id === 'removeIcon') DOM.style.color = '#9B9B9B'
      else DOM.style.color = '#C6C6C6'
    },
    hideRemoveIcon() {
      const DOM = document.getElementById('removeIcon_' + this._uid.toString())
      DOM.style.display = 'none'
    },
    selectFunction(option) {
      if (option && option.disabled) return
      this.$emit('input', option)
    },
    removeFunction() {
      this.$emit('input', '')
    },
    filterOptions() {
      if (this.value === '' || this.filter_disabled) this.options_show = this.options
      else {
        const optionRegExp = new RegExp(`${this.value}`)
        var filteredArray = []
        for (let i=0; i<this.options.length; i++) {
          if (optionRegExp.test(this.options[i])) filteredArray.push(this.options[i])
        }
        this.options_show = filteredArray
      }
    }
  },
  watch: {
    value() {
      this.filterOptions()
    },
    options() {
      this.filterOptions()
    }
  },
  destroyed() {
    document.removeEventListener('click', this.clickEvent)
    document.removeEventListener('scroll', this.scrollEvent, true)
  }
}
</script>

<style lang="scss" module>
@import './common/general.scss';
.singleSelect {
  background-color: #fff;
  .inputArea {
    @include block(100%, $radius: 3px);
    display: flex;
    border: 1px solid #a7a7a7;
    cursor: pointer;
    .addonBefore {
      padding: 4px;
    }
    .inputText {
      @include block(100%, 30px, $radius: 3px);
      outline: none;
      border: none;
      font-size: 15px;
      cursor: pointer;
      color: rgb(73, 73, 73);
    }
    .removeIcon {
      display: none;
      cursor: pointer;
      padding: 4px;
    }
  }
  .inputArea:focus-within {
    border: 1px solid #6294f1;
    box-shadow: 0px 0px 5px #739ce9;
  }
  
  .optionsArea {
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    transition: max-height 0.2s linear;
    box-shadow: 0px 0px 5px rgb(194, 194, 194);
    overflow-y: auto;
    font-family: Microsoft JhengHei;
    position: absolute;
    background-color: #fff;
    z-index: 1;
    .option {
      @include block(100%);
      display: flex;
      cursor: pointer;
      .optionText {
        @include block(90%);
        margin-left:10px;
        padding: 5px;
        font-size:16px;
        color: rgb(73, 73, 73);
      }
    }
    .option:hover {
      background-color: #eaf4ff;
    }
    .noOptions {
      @include block(100%);
      overflow: hidden;
      padding: 5px 0px;
      text-align: center;
      .noDataIcon {
        font-size:30px;
        color: rgb(148, 148, 148);
      }
      .noDataText {
        padding-left:10px;
        font-size:16px;
        color: rgb(148, 148, 148);
      }
    }
  }
}

</style>