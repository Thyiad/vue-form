<template>
  <base-date :options="options" v-model="val"></base-date>
</template>

<script>

import BaseDate from './base-date'

export default {
  components: { BaseDate },
  props: {
    options: {
      default: Object,
      required: true
    },
    value: {
      required: true,
      default: ''
    }
  },
  computed: {
  	isRange(){
  		return this.options && this.options.type && this.options.type.endsWith('range');
	},
    val: {
      get () {
      	if(this.isRange){
      		if(Array.isArray(this.value) && this.value.length===2){
      			return this.value.map(v=> new Date(v));
			}else{
      			return []
			}
		}else{
			return new Date(this.value)
		}
      },
      set (value) {
        var time = ''
        if (value) {
        	let tempValue = Array.isArray(value)?value:[value];
			time = tempValue.map(v=>{
				let year = v.getFullYear()
				let month = v.getMonth() + 1
				let day = v.getDate()
				let hour = v.getHours()
				let minute = v.getMinutes()
				let second = v.getSeconds()
				let real = function (number) {
					return number >= 10 ? number.toString() : '0' + number
				}
				return [
					year,
					'-',
					real(month),
					'-',
					real(day),
					'T',
					real(hour),
					':',
					real(minute),
					':',
					real(second)
				].join('')
			})
			if(!this.isRange){
        		time=time[0];
			}
        }
        this.$emit('input', time)
      }
    }
  }
}
</script>