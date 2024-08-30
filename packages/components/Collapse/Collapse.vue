<script setup lang="ts">
import type {CollapseProps,CollapseEmits,CollapseItemName} from './types';
import { provide,ref,watch } from 'vue';
import {COLLAPSE_STX_KEY} from "./constants"
defineOptions({
    name:'ErCollapse'
});
const props=defineProps<CollapseProps>();
const emits=defineEmits<CollapseEmits>();
const activeNames=ref(props.modelValue);
if(props.accordion&&activeNames.value.length>1){
    console.warn('');
    //错误测试
}
function handleItemClick(item:CollapseItemName){
    let _activeNames=[...activeNames.value];
    //开启手风琴模式
    if(props.accordion){
        _activeNames=[_activeNames[0]===item?"":item];
        updateActiveNames(_activeNames);
        return
    }
    const index=_activeNames.indexOf(item);
    if(index>-1)
    {
        _activeNames.splice(index,1);
        //存在情况下
    }
    else{
        _activeNames.push(item);
        //不存在情况下则需要push
    }
    updateActiveNames(_activeNames);
}
function updateActiveNames(newActiveNames:CollapseItemName[]){
    activeNames.value=newActiveNames;
    emits("update:modelValue",newActiveNames);
    emits("change",newActiveNames);
}
watch(
    ()=>props.modelValue,
(newNames)=>updateActiveNames(newNames)
)
provide(COLLAPSE_STX_KEY,{
activeNames,
handleItemClick
})
</script>

<template>
<div class="er-collapse">
    <slot></slot>
</div>
</template>
<style scoped>
@import './style.css';
</style>