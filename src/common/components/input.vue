<template>
  <el-input
    v-model="val"
    :size="size"
    :type="type"
    :placeholder="placeholder"
    :class="className"
    @focus="handleFocus"
    @blur="handleBlur"
  />
</template>
<script>
import { Component, Vue, Prop, Watch } from 'vue-property-decorator'

@Component({})
export default class FeInput extends Vue{
  val = this.value

  @Prop({default:''})
  value

  @Prop({default:''})
  defaultValue

  @Prop({default:'number'})
  type

  @Prop({default:''})
  size

  @Prop({default:''})
  placeholder

  @Prop({default:false})
  isRequire

  @Prop({default:null})
  min

  @Prop({default:null})
  max

  @Prop({default:''})
  className

  @Watch('val')
  valChange(nval){
    this.$emit('input',nval)
  }

  @Watch('value')
  valueChange(nval,oval){
    if(nval !== oval){
      this.val = nval
    }
  }

  handleFocus(){
    this.$emit('inputFocus')
  }

  handleBlur(){
    if(this.min === null || this.max === null){
      this.$emit('onBlur')
      return
    }
    const val = this.val === '' ? this.val : Number(this.val);
    if(this.isRequire && val === ''){
      this.val = this.defaultValue || this.min
    }else if(val < 0){
      this.val = Math.abs(this.val)
    }else if(val < this.min){
      this.val = this.defaultValue || this.min
    }else if(val > this.max){
      this.val = this.defaultValue || this.max
    }
    this.$emit('onBlur')
  }
}
</script>
