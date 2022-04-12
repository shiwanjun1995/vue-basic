<template>
    <div class="avatar-upload" v-loading="loading">
        <div class="avatar-cover" :style="priview">
            <i class="el-icon-plus"></i>
        </div>
        <input class="avatar-input" type="file" name="file" @change="avatarChange"/>
    </div>
</template>

<script>
// 引入post请求 引入定义的上传图片的api
import { post } from 'Axios';
import API from '@/api/api.js';
export default {
    name: 'uploadAvatar',
    props: {
        vUrl: {
            type: String,
            default: ''
        }
    },
    data() {
        return {
            priviewUrl: '',
            loading: false
        }
    },
    watch: {
        // 监视外部引入的的地址
        vUrl(newVal, oldVal) {
            this.priviewUrl = newVal;
        }
    },
    computed: {
        // 前景图
        priview() {
            let v = this.priviewUrl;
            return v ? { backgroundImage: `url(${v})` } : {};
        }
    },
    methods: {
        // 传入事件对象 可获取事件的一系列属性
        avatarChange(e) {
            this.loading = true;
            let files = e.target.files;
            // 文件对应的MIMEType类型 .jpg .jpeg 对应MIMEType类型都为 image/jpeg
            let regJPGPNG = /\/(jpeg|png)$/;
            // 只能上传 jpg 和 png 格式的图片 前端直接判断 免去服务器的鸭力
            let isJPGPNG = regJPGPNG.test(files[0].type);
            let isLT1M = files[0].size / 1024 / 1024 < 1;
            if (!isJPGPNG) {
                this.$warning("上传图片格式只能是 JPG、PNG 格式!");
                return false;
            };
            if (!isLT1M) {
                this.$warning("上传图片大小不能超过 1MB!");
                return false;
            };
            // FormData 对象的使用：
            // 1.用一些键值对来模拟一系列表单控件：即把form中所有表单元素的name与value组装成一个queryString
            // 2. 异步上传二进制文件。
            let params = new FormData();
            // 后端定义的上传二进制文件属性名 key
            params.append('multipartFile', files[0]);
            post(API.API_POST_IMGUPLOAD, params).then(res => {
                const { data } = res;
                if (data.code === 200) {
                    this.priviewUrl = data.body;
                    this.$emit('update:vUrl', this.priviewUrl);
                    // 给浏览器缓存图片
                    // src属性一定要写到 onload 的后面 否则程序在IE中会出错
                    let image =  new Image();
                    image.onload = (() => {
                        this.loading = false;
                    });
                    image.src = this.priviewUrl;
                } else {
                    this.loading = false;
                    this.$error(res.message);
                }
            }).catch((err) => {
                this.loading = false;
                this.$error('上传图片失败，请重试');
            })
        }
    }
}
</script>

<style lang='css' scoped>
    .avatar-upload {
        width: 135px;
        height: 135px;
        border: 1px dashed #ccc;
        box-sizing: border-box; /** 怪异盒模型 */
        position: relative;
    }
    .avatar-cover {
        position: absolute;
        top: 2px;
        right: 2px;
        bottom: 2px;
        left: 2px;
        background: #eee;
        /**项目排列方式 */
        display: flex;
        justify-content: center; /**水平轴主轴*/
        align-items: center; /**垂直轴交叉轴 */
        /**背景图填充方式 */
        background-position: center; /**设置了一个值的话 第二个值默认为center */
        background-repeat: no-repeat;
        background-size: cover;
    }
    .avatar-cover i {
        color: #bbb;
        font-size: 25px;
    }
    .avatar-input {
        position: absolute;
        width: 100%;
        height: 100%;
        left: 0;
        top: 0;
        opacity: 0;
    }
</style>
