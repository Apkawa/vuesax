<template lang="html">
  <transition name="popup-t">
    <div
      v-show="active"
      ref="con"
      :class="[`vs-popup-${color}`,{'fullscreen':fullscreen}]"
      class="vs-component con-vs-popup"
      @click="close($event,true)">
      <div
        :style="styleCon"
        class="vs-popup--background"/>
      <div
        ref="popupx"
        :style="stylePopup"
        class="vs-popup">

        <!-- //header -->
        <header
          :style="styleHeader"
          class="vs-popup--header">
          <div class="vs-popup--title">
            <h3>{{ title }}</h3>
          </div>
          <vs-icon
            v-if="!buttonCloseHidden"
            ref="btnclose"
            :icon-pack="iconPack"
            :icon="iconClose"
            :style="stylePopup"
            class="vs-popup--close vs-popup--close--icon"
            @click="close"
          />
        </header>

        <!-- // slots  -->
        <div
          :style="styleContent"
          :class="classContent"
          class="vs-popup--content">
          <slot/>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import _color from '../../utils/color.js'
export default {
  name:'VsPopup',
  props:{
    color:{
      default:'primary',
      type:String
    },
    active: {
      default:false,
      type: Boolean
    },
    title:{
      default:'popup',
      type:String
    },
    buttonCloseHidden:{
      default:false,
      type:Boolean
    },
    fullscreen:{
      default:false,
      type:Boolean
    },
    backgroundColor:{
      default:null,
      type:String
    },
    backgroundColorPopup:{
      default:'rgb(255,255,255)',
      type:String
    },
    styleContent:{
      default:null,
      type: String
    },
    classContent:{
      default:null,
      type: String
    },
    iconPack:{
      default:'material-icons',
      type:String
    },
    iconClose:{
      default:'close',
      type:String
    },
  },
  computed:{
    styleHeader(){
      return {
        color: _color.getColor(this.color,1),
      }
    },
    styleAfter(){
      return {
        background: _color.getColor(this.color,1)
      }
    },
    styleCon(){
      return {
        background: _color.getColor(this.backgroundColor,1)
      }
    },
    stylePopup(){
      return {
        background: _color.getColor(this.backgroundColorPopup,1)
      }
    }
  },
  mounted(){
    this.insertBody()
  },
  destroyed() {
    this.$el.remove()
  },
  methods:{
    giveColor(color){
      return _color.rColor(color)
    },
    close(event,con){
      if(con){
        if(event.target.className.indexOf('vs-popup--background')!=-1){
          this.$emit('update:active',false)
          this.$emit('close', false)
        } else if(this.$refs && event.srcElement == this.$refs.btnclose.$el){
          this.$emit('update:active',false)
          this.$emit('close', false)
        }
      }
    },
    insertBody(){
      let elx = this.$refs.con
      let popups = document.querySelectorAll('div.con-vs-popup');
      let child = document.body.firstChild;
      if (popups.length > 0) {
        child = popups[popups.length - 1].nextSibling
      }
      document.body.insertBefore(elx, child)
    },
  }
}
</script>
