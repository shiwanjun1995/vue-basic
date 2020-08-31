<template>
    <div class="custom-input">
        <!-- 需要引入elementUI 利用其input的样式 -->
        <input class="el-input__inner"
            v-model="inputVal"
            placeholder="请输入纯数字"
            @input="handleInput('inputVal',$event)"
        />
    </div>
</template>
<script>
export default {
    props: {
        // 默认值
        defaultNum: {
            type: String,
            default: ''
        },
        // 正整数
        isInt: {
            type: Boolean,
            default: true
        },
        // 保留几位小数
        keepPoint: {
            type: Number,
            default: 2
        }
    },
    data() {
        return {
            inputVal: this.defaultNum, // 输入框的值也是传递给外面组件的值
        }
    },
    methods: {
        handleInput(val,event) {
            if (this.isInt) {
                // 替换非数字的字符
                let value = event.target.value.replace(/\D+/, '')
                this[val] = value
            } else {
                // 替换成只包含指定小数位的数字字符
                let matchArr = event.target.value.match(/\d+\.?\d*/)
                this[val] = matchArr ? matchArr[0] : ''
                // 如果保留了小数点还要还要判断包含小数点的标志性符号. 因为如果定义了isInt:false也可以输入正整数
                let str = String(this[val])
                if (this.keepPoint) {
                    // 首先通过 indexOf('.')找到包含小数点.的位置[小数是从哪里开始的]
                    // 例：substr(start,length) 返回的是从起始位置开始起读取长度为length个的字符
                    // 如果想要读取几位就需要从这个索引值的位置开始 +(.xx)总共包含3位数 | +(.xxx)总共包含4位数 所以需要在指定读取小数位上+1
                    if (str.includes('.')) {
                        this[val] = str.substr(0, str.indexOf('.') + (this.keepPoint + 1))
                    } else {
                        // 如果输入框中没有包含小数点. 还需要排除首位为01、02的字符
                        if (str != '') this[val] = String(parseFloat(this[val]))
                    }
                }
            }
            // this[val]其实就是inputVal
            this.$emit('update',this[val])
        }
    },
}
// 参考链接：https://github.com/StoneCode5/vue-component
</script>
