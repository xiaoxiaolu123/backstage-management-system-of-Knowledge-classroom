<!-- 编辑管理员 -->

<template>
    <div class="box">
        <FanHui :msg="'编辑管理员'"></FanHui>
        <!-- <div> -->
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-form-item label="角色" prop="">
                <el-select v-model="ruleForm.region" multiple placeholder="请选择" @change="handleSelectChange">
                    <el-option v-for="item in options" :key="item.id" :label="item.display_name" :value="item.id"> </el-option>
                </el-select>
                <el-link type="primary" @click.stop="Compile">角色管理</el-link>
            </el-form-item>
            <el-form-item label="姓名" prop="username">
                <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>
            <el-form-item label="邮箱" prop="mailbox">
                <el-input v-model="ruleForm.email"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="">
                <el-input v-model="ruleForm.password"></el-input>
                <el-alert title="不修改密码请勿填写" :closable="false" close-text type="info" show-icon> </el-alert>
            </el-form-item>
            <div class="denIu">
                <span>禁止登录</span> <el-switch v-model="ruleForm.is_ban_login" :active-value="1" :inactive-value="0" active-color="#409eff" inactive-color="#dcdfe6"> </el-switch>
            </div>
            <div class="bottom-form">
                <el-form-item>
                    <el-button type="primary" @click.stop="handleSubmit">保存</el-button>
                    <el-button @click.stop="Cancel">取消</el-button>
                </el-form-item>
            </div>
        </el-form>
        <!-- </div> -->
    </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
import FanHui from "@/components/FanHui.vue";
export default {
    //import引入的组件需要注入到对象中才能使用
    components: {
        FanHui,
    },
    data() {
        //这里存放数据
        return {
            form: {},
            value: false,
            ruleForm: {
                username: "",
                password: "",
                password_confirmation: "",
                mailbox: "",
                is_ban_login: "1",
                delivery: false,
                type: [],
                resource: "",
                desc: "",
                region: [],
            },
            rules: {
                username: [
                    {
                        required: true,
                        message: "姓名不能为空",
                        trigger: "blur",
                    },
                ],
                password: [
                    {
                        required: true,
                        message: "密码不能为空",
                        trigger: "blur",
                    },
                ],
                mailbox: [
                    {
                        required: true,
                        message: "邮箱不能为空",
                        trigger: "blur",
                    },
                ],
            },
            options: {},
            // region: [],
        };
    },
    //监听属性 类似于data概念
    computed: {},
    //监控data中的数据变化
    watch: {},
    //方法集合
    methods: {
        handleSelectChange() {
            this.$forceUpdate();
        },
        Cancel() {
            this.$router.push({
                path: "/system/system-administrator",
            });
        },
        Compile() {
            this.$router.push({
                path: "/system/adminroles",
            });
        },
        handleSubmit: function () {
            // 获取form表单，调用校验方法

            this.$request
                .put(`administrator/${this.$route.query.id}`, {
                    name: this.ruleForm.name,
                    email: this.ruleForm.email,
                    password: this.ruleForm.password,
                    is_ban_login: this.ruleForm.is_ban_login,
                    role_id: this.ruleForm.region,
                })
                .then(() => {
                    if (this.ruleForm.name.length == 0 || this.ruleForm.email == 0) {
                        this.$message.error(data.message);
                        return;
                    }
                    this.$message({
                        type: "success",
                        message: "编辑成功",
                    });
                    this.$router.push({
                        path: "/system/system-administrator",
                    });
                })
                .catch((e) => {
                    this.$message.error("编辑失败");
                });
            // console.log(this.ruleForm.region);
        },
        async getParameters(params) {
            let arr = await this.$request.get("administrator/create", { params }).then((res) => {
                // console.log(JSON.parse(JSON.stringify(res.data)));
                // console.log(res.data);
                this.options = res.data.roles;
            });
        },
    },
    //生命周期 - 创建完成（可以访问当前this实例）
    async created() {
        this.getParameters();
        let arr = await this.$request.get(`administrator/${this.$route.query.id}`);
        this.ruleForm = arr.data;
        // console.log(arr.data);
        this.ruleForm.region = arr.data.role_id;
    },
    //生命周期 - 挂载完成（可以访问DOM元素）
    mounted() {},
    beforeCreate() {}, //生命周期 - 创建之前
    beforeMount() {}, //生命周期 - 挂载之前
    beforeUpdate() {}, //生命周期 - 更新之前
    updated() {}, //生命周期 - 更新之后
    beforeDestroy() {}, //生命周期 - 销毁之前
    destroyed() {}, //生命周期 - 销毁完成
    activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style lang="less" scoped>
.box {
    width: 100%;
    height: auto;
    float: left;
    background-color: #fff;
    box-sizing: border-box;
    padding: 30px;
    border-radius: 15px;
    margin-bottom: 90px;
    box-shadow: 0 2px 4px 0 hsl(0deg 0% 40% / 5%);
    min-width: 1180px;
    .denIu {
        margin-left: 30px;
        margin-top: 30px;
        font-size: 16px;
        span {
            padding-right: 15px;
            clear: #606266;
        }
    }
    /deep/.el-form.demo-ruleForm {
        margin-left: 120px;
        font-size: 16px;
        margin-bottom: 30px;
    }
    /deep/.el-input__inner {
        width: 350px;
        height: 45px;
    }
    /deep/.el-link--inner {
        margin-left: 18px;
        font-size: 17px;
    }
    /deep/.el-form-item__label {
        font-size: 16px;
    }
    .bottom-form {
        position: fixed;
        bottom: 0;
        height: 95px;
        left: 200px;
        right: 0;
        z-index: 2000;
        box-sizing: border-box;
        padding-top: 28px;
        padding-bottom: 20px;
        padding-left: 0px;
        background-color: #fff;
        display: flex;
        box-shadow: 0 -2px 4px 0 hsl(0deg 0% 40% / 5%);
        /deep/.el-form-item {
            // padding-left: 30px;
            margin-left: -80px;
        }
        /deep/.el-button {
            font-weight: 400;
            height: 45px;
            width: 80px;
            font-size: 16px;
            margin-right: 20px;
        }
    }
    /deep/.el-alert--info.is-light {
        background-color: #ffffff;
        position: absolute;
        top: -5px;
        left: 350px;
    }
    /deep/.el-alert {
        width: 250px;
    }
    /deep/.el-alert__title {
        font-size: 16px;
    }
}
</style>
