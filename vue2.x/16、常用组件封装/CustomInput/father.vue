<template>
    <div class="demo">
        <CustomInput
            :defaultNum="pureNum"
            :isInt="true"
            :keepPoint="2"
            @update="pureNum = $event"
        ></CustomInput>
    </div>
</template>

<script>
import CustomInput from './index'
export default {
    components: {
        CustomInput
    },
    data() {
        return {
            pureNum: '', // 父组件定义的输入框的值纯数字
        }
    },
}
</script>

