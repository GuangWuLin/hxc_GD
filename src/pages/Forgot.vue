<template>
    <section class="forgot-section">
        <div class="section-label">
            {{'找回'+typeText+'密码'}}
        </div>
        <div class="section-inner">
            <el-steps :space="200" :active="activeStep" process-status="process" finish-status="finish" :center="true" :align-center="true">
                <el-step title="填写账号"></el-step>
                <el-step title="验证身份"></el-step>
                <el-step title="设置新密码"></el-step>
            </el-steps>
            <el-row class="mt20 t-center" v-if="activeStep===0">
                <el-form :model="forgot.form" class="forgot-form" label-width="110px" :rules="forgot.rules" ref="forgotForm">
                    <el-form-item label="服务所属区域" prop="zoneCode">
                        <el-select v-model="forgot.form.zoneCode" placeholder="服务所属区域" popper-class="login-select" ref="loginSelect" disabled remote filterable>
                            <el-option v-for="(item,index) in zones" :label="item.label" :value="item.value" :disabled="item.disabled" :key="index">
                            </el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="手机号" prop="phone">
                        <el-input auto-complete="off" v-model="forgot.form.phone" placeholder="输入手机号" class="input" :maxlength="11"></el-input>
                    </el-form-item>
                </el-form>
            </el-row>
            <el-row class="mt20 t-center" v-if="activeStep===1">
                <el-form :model="forgot.form" class="forgot-form" label-width="110px" :rules="forgot.rules" ref="forgotForm">
                    <el-form-item label="手机号" prop="phoneDisabled">
                        <el-input auto-complete="off" v-model="forgot.form.phone" :disabled="true" class="input" style="border:0;"></el-input>
                    </el-form-item>
                    <el-form-item label="验证码" prop="code">
                        <el-input auto-complete="off" v-model="forgot.form.code" placeholder="输入验证码" class="input" style="width:50%;"></el-input>
                        <el-button type="primary" @click="sendCodeByPhone" class="ml5" :disabled="codeDisable">{{sendCodeByPhoneText}}</el-button>
                    </el-form-item>
                </el-form>
            </el-row>
            <el-row class="mt20 t-center" v-if="activeStep===2">
                <el-form :model="forgot.form" class="forgot-form" label-width="110px" :rules="forgot.rules" ref="forgotForm">
                    <el-form-item label="新密码" prop="password">
                        <el-input type="password" placeholder="新密码" v-model="forgot.form.password" class="input"></el-input>
                    </el-form-item>
                    <el-form-item label="确认密码" prop="repeatPassword">
                        <el-input type="password" placeholder="再次输入新密码" v-model="forgot.form.repeatPassword" class="input"></el-input>
                    </el-form-item>
                </el-form>
            </el-row>
            <el-row class="mt30 t-center" v-if="activeStep===3">
                <em class="el-icon-circle-check"></em>
                <div class="jump-tips">
                    <div>恭喜您，密码找回成功</div>
                    <div>
                        <em>{{jumpInterval}}</em>秒后浏览器将跳转至登录页面</div>
                </div>
            </el-row>
            <el-row class="mt20 t-center" v-if="activeStep<3">
                <el-col :span="12">
                    &nbsp;
                    <el-button class="button" @click="prevStep" v-if="activeStep>0">上一步</el-button>
                </el-col>
                <el-col :span="12">
                    <el-button type="primary" @click="nextStep" v-if="activeStep<2" :loading="forgot.loading">下一步</el-button>
                    <el-button type="primary" @click="nextStep" v-else :loading="forgot.loading">完成</el-button>
                </el-col>
            </el-row>
        </div>
        <el-dialog title="慧学车" v-model="forgot.services.visible" size="tiny" custom-class="services-dialog" :close-on-click-modal="false" @close="DialogClose">
            <el-row>
                <el-radio-group v-model="forgot.services.select">
                    <el-radio v-for="(item,index) in forgot.services.data" :label="item" :key="index">{{item.name}}</el-radio>
                </el-radio-group>
            </el-row>
            <span slot="footer" class="dialog-footer">
                <el-button @click="forgot.services.visible = false">取 消</el-button>
                <el-button type="primary" @click="activeStep++">确 定</el-button>
            </span>
        </el-dialog>
    </section>
</template>

<script>
import { request } from "../api/api";
import { global } from "../assets/javascript/global";
export default {
    data() {
        var validatePhone = (rule, value, callback) => {
            if (!global.fieldVerification.IsPhone(value)) {
                callback(new Error("电话号码格式不正确"));
            }
            else {
                callback();
            }
        };
        var validateRepeatPass = (rule, value, callback) => {
            if (value !== this.forgot.form.password) {
                callback(new Error("两次输入密码不一致"));
            } else {
                callback();
            }
        };
        return {
            type: "",
            typeText: "",
            zones: [],//global.options.zone
            activeStep: 0,
            forgot: {
                loading: false,
                services: {
                    data: [],
                    select: {},
                    visible: false
                },
                form: {
                    zoneCode: "",
                    phone: "",
                    code: "",
                    password: "",
                    repeatPassword: ""
                },
                rules: {
                    zoneCode: [
                        { required: true, message: "请选择服务所在地区", trigger: "change" }
                    ],
                    phone: [
                        { required: true, message: "请填写手机号", trigger: "blur" },
                        { validator: validatePhone, trigger: "blur" }
                    ],
                    code: [
                        { required: true, message: "请输入验证码", trigger: "blur" },
                        { min: 4, max: 6, message: "长度在 4 到 6 个字符之间", trigger: "change" }
                    ],
                    password: [
                        { required: true, message: "请输入新密码", trigger: "blur" },
                        { min: 6, max: 12, message: "长度在 6 到 12 个字符之间", trigger: "change" }
                    ],
                    repeatPassword: [
                        { required: true, message: "请重复输入新密码", trigger: "blur" },
                        { min: 6, max: 12, message: "长度在 6 到 12 个字符之间", trigger: "change" },
                        { validator: validateRepeatPass, trigger: "blur" }
                    ]
                }
            },
            codeDisable: false,
            sendCodeByPhoneText: "发送验证码",
            jumpInterval: 5
        }
    },
    methods: {
        //查询服务所在地
        queryServiceZone() {
            this.zones.push({
                label: "广东省",
                value: "440000"
            });
            this.forgot.form.zoneCode = "440000";
            // request.regCenter.query.serviceArea().then((res) => {
            //     if (res.success) {
            //         this.zones = [];
            //         let data = res.object;
            //         for (let item in data) {
            //             if (data[item].name.indexOf("广东") > -1) {
            //                 this.zones.push({
            //                     label: data[item].name + "省",
            //                     value: data[item].code
            //                 });
            //                 break;
            //             }
            //         }
            //         if (this.zones.length) {
            //             this.forgot.form.zoneCode = this.zones[0].value;
            //         }
            //         else {
            //             this.$message.error("无可用地区服务，请联系管理员");
            //         }
            //     }
            // });
        },
        queryServiceList() {
            request.regCenter.query.serviceList([this.forgot.form.zoneCode, this.forgot.form.phone]).then((res) => {
                if (res.success) {
                    let data = res.object;
                    if (!data.length) {
                        this.$message.error("账号不存在");
                    }
                    else {
                        if (data.length > 1) {
                            this.forgot.services.data = data;
                            this.forgot.services.visible = true;
                        }
                        else {
                            this.activeStep++;
                        }
                        localStorage.setItem("baseUrl", data[0].url + "/hxc");
                    }
                }
                else {
                    this.$message.error(res.message);
                }
                this.forgot.loading = false;
            });
        },
        sendCodeByPhone() {
            let timer = 60;
            let phone = this.forgot.form.phone;
            request.public.sendCodeByPhone(phone).then((res) => {
                if (res.success) {
                    this.codeDisable = true;
                    this.sendCodeByPhoneText = timer + "秒后重获";
                    let timer_interval = setInterval(() => {
                        timer--;
                        if (timer === 0) {
                            this.codeDisable = false;
                            clearInterval(timer_interval);
                            this.sendCodeByPhoneText = "发送验证码";
                        }
                        else {
                            this.sendCodeByPhoneText = timer + "秒后重获";
                        }
                    }, 1000);
                }
                else {
                    this.$message.error(res.message);
                }
            });
        },
        prevStep() {
            this.activeStep--;
        },
        nextStep() {
            this.$refs["forgotForm"].validate((valid) => {
                if (valid) {
                    this.forgot.loading = true;
                    if (this.activeStep === 0) {
                        this.queryServiceList();
                    }
                    else if (this.activeStep === 2) {
                        this.post(this.forgot.form);
                    }
                    else {
                        this.activeStep++;
                        this.forgot.loading = false;
                    }
                }
            });
        },
        post(form) {
            request.public.postResetPassword([form.phone, form.code, form.password, this.type]).then((res) => {
                if (res.success) {
                    this.activeStep++;
                    this.$message.success(this.typeText + "密码找回成功");
                    this.forgot.form = { zoneCode: "", phone: "", code: "", password: "", repeatPassword: "" };
                    let jump_interval = setInterval(() => {
                        if (this.jumpInterval === 0) {
                            clearInterval(jump_interval);
                            this.$router.replace("/");
                        }
                        else {
                            this.jumpInterval--;
                        }
                    }, 1000);
                }
                else {
                    this.activeStep--;
                    this.$message.error(this.typeText + "密码找回失败，原因：" + res.message);
                }
                this.forgot.loading = false;
            });
        },
        DialogClose() {
            this.forgot.services.select = {};
        }
    },
    created() {
        if (window.localStorage.getItem("zone") !== null) {
            this.zone = window.localStorage.getItem("zone");
        }
        this.forgot.form = { zoneCode: "", phone: "", code: "", password: "", repeatPassword: "" };
        this.type = this.$router.history.current.params.type;
        this.typeText = this.type === "login" ? "登录" : "支付";
        this.queryServiceZone();
    }
}
</script>

<style lang="scss">
.forgot-section {
    width: 60%;
    margin: 20px auto;
    .section-label {
        font-size: 2em;
    }
    .section-inner {
        width: 550px;
        margin: 30px auto;
        height: 60px;
        padding: 20px 0 0 0;
    }
}

.forgot-form {
    width: 620px;
    margin: 0 auto;
    .el-form-item__content {
        text-align: left;
    }
    .el-select,
    .input {
        width: 72%;
    }
}

.el-icon-circle-check {
    font-size: 4em;
    color: #13CE66;
}

.jump-tips {
    margin-top: 10px;
    div {
        line-height: 30px;
        em {
            font-style: normal;
            color: #FF4949;
            font-weight: bolder;
            margin-right: 5px;
        }
    }
}
</style>