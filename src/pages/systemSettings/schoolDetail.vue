<template>
    <section>
        <div class="basic mt20" style="margin-top:0;padding:0 20px;">
            <el-row class="devideline">
                <el-col :span="5">
                    <span>驾校名称：{{this.schoolForme.schoolName}}</span>
                </el-col>
                <el-col :span="6">
                    <span>驾校全称：{{this.schoolForme.schoolFullName}}</span>
                </el-col>
                <el-col :span="6">
                    <span>驾校地址：{{this.schoolForme.schoolAddress}}</span>
                </el-col>
                <el-col :span="3">
                    <div style="border:groove;width:106px;position:absolute;left:1050px;top:-18px">
                        <img :src="this.schoolForme.schoolLogo" style="height:120px;width:100px">
                    </div>
                </el-col>
            </el-row>
    
            <el-row class="devideline">
                <el-col :span="5">
                    <span>法人名称：{{this.schoolForme.legalPerson}}</span>
                </el-col>
    
                <el-col :span="6">
                    <span>校长名称：{{this.schoolForme.principal}}</span>
                </el-col>
                <el-col :span="6">
                    <span>校长电话：{{this.schoolForme.phone}}</span>
                </el-col>
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>驾校座机：{{this.schoolForme.landline}}</span>
                </el-col>
                <el-col :span="6">
                    <span>驾校联系人：{{this.schoolForme.contact}}</span>
                </el-col>
                <el-col :span="6">
                    <span>培训范围：{{this.schoolForme.scope}}</span>
                </el-col>
    
                <el-col :span="6">
                    <span>经营许可证号：{{this.schoolForme.licenseNo}}</span>
                </el-col>
    
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>经营许可证日期：{{this.schoolForme.licenseDate}}</span>
                </el-col>
                <el-col :span="6">
                    <span>营业执照注册号：{{this.schoolForme.businesslicenseNo}}</span>
                </el-col>
                <el-col :span="6">
                    <span>统一社会信用代码：{{this.schoolForme.creditNo}}</span>
                </el-col>
                <el-col :span="6">
                    <span>经营状态：{{this.schoolForme.operatingState}}</span>
                </el-col>
    
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>分类等级：{{this.schoolForme.schoolLevel}}</span>
                </el-col>
                <el-col :span="6">
                    <span>邮编:{{this.schoolForme.postalCode}}</span>
                </el-col>
                <el-col :span="6">
                    <span>提前预约时间：{{this.schoolForme.advanceAppointmentHour}}</span>
                </el-col>
                <el-col :span="6">
                    <span>取消预约提前时间：{{this.schoolForme.advanceCancelHour}}</span>
                </el-col>
    
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>强制签退时间:{{this.schoolForme.afterExitMin}}</span>
                </el-col>
                <el-col :span="6">
                    <span>训前提前禁约:{{this.schoolForme.studyBeforHour}}</span>
                </el-col>
                <el-col :span="6">
                    <span>提前签到时间:{{this.schoolForme.beforeSignMin}}</span>
                </el-col>
                <el-col :span="6">
                    <span>年培训量:{{this.schoolForme.trainingCount}}</span>
                </el-col>
    
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>投入预约车辆:{{this.schoolForme.orderCarCount}}</span>
                </el-col>
                <el-col :span="6">
                    <span>教练总数:{{this.schoolForme.teacherCount}}</span>
                </el-col>
                <el-col :span="6">
                    <span>车辆总数:{{this.schoolForme.carCount}}</span>
                </el-col>
                <el-col :span="6">
                    <span>考核员总数:{{this.schoolForme.checkerCount}}</span>
                </el-col>
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>安全员总数:{{this.schoolForme.securityCount}}</span>
                </el-col>
                <el-col :span="6">
                    <span>场地总面积:{{this.schoolForme.siteScale}}</span>
                </el-col>
                <el-col :span="6">
                    <span>教室总面积:{{this.schoolForme.schoolRoom}}</span>
                </el-col>
                <el-col :span="5">
                    <span>理论教室总面积:{{this.schoolForme.theoryRoom}}</span>
                </el-col>
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>教员总数:{{this.schoolForme.allteachCount}}</span>
                </el-col>
                <el-col :span="6">
                    <span>短信条数:{{this.schoolForme.smsBalance}}</span>
                </el-col>
                <el-col :span="6">
                    <span>备注:{{this.schoolForme.comments}}</span>
                </el-col>
                <el-col :span="5">
                    <span>是否开启计时:{{this.schoolForme.timer}}</span>
                </el-col>
    
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>是否开启财务审核:{{this.schoolForme.fnCheck}}</span>
                </el-col>
                <el-col :span="6">
                    <span>是否开启车辆限制:{{this.schoolForme.astrictCar}}</span>
                </el-col>
                <el-col :span="6">
                    <span>是否强制签退:{{this.schoolForme.forceExit}}</span>
                </el-col>
                <el-col :span="5">
                    <span>是否预约限制签到:{{this.schoolForme.judgeAppointment}}</span>
                </el-col>
            </el-row>
            <el-row class="devideline">
                <el-col :span="5">
                    <span>是否开启预约:{{this.schoolForme.appointment}}</span>
                </el-col>
            </el-row>
        </div>
    </section>
</template>
<style scope>
.devideline {
    border-bottom: 1px dotted #dedede;
    margin: 20px 20px;
    padding-bottom: 12px;
    font-size: 14px;
}
</style>
   
<script>
import { request } from "../../api/api";
import { global } from "../../assets/javascript/global";
export default {
    data() {
        var schoolDateVali = (rule, value, callback) => {
            if (value == '') {
                callback(new Error('请填写驾校简称'));
            } else {
                callback();
            }
        };
        return {
            schoolCode: JSON.parse(localStorage.getItem("user")).schoolCode,
            schoolForme: { //修改的表单
            },
        }
    },
    methods: {
        //根据驾校编号查询详情    
        queryBySchoolCode() {
            let args = [this.schoolCode];
            request.systemSettings.schoolDetail.queryBySchoolCode(args).then((res) => {
                if (res.success === true) {
                    this.schoolForme = res.object;
                    this.schoolForme.timer = this.changeBool(this.schoolForme.timer);
                    this.schoolForme.fnCheck = this.changeBool(this.schoolForme.fnCheck);
                    this.schoolForme.astrictCar = this.changeBool(this.schoolForme.astrictCar);
                    this.schoolForme.forceExit = this.changeBool(this.schoolForme.forceExit);
                    this.schoolForme.judgeAppointment = this.changeBool(this.schoolForme.judgeAppointment);
                    this.schoolForme.appointment = this.changeBool(this.schoolForme.appointment);
                    this.schoolForme.operatingState =  this.changeState( this.schoolForme.operatingState);
                     this.schoolForme.schoolLevel =  this.schoolLevel( this.schoolForme.schoolLevel);
                }
            });
        },
        //转换状态
        changeBool(bool) {
            let str = "";
            if (bool == true) {
                str = "是";
            } else if (bool == false) {
                str = "否"
            }
            return str;
        },
        //经营状态转换
        changeState(state){
            let str = "";
            if(state==1){
                str = "营业";
            }else if(state==2){
                str="停业";
            }else if(state==3){
                str="整改";
            }else if(state==4){
                str="停业整顿";
            }else if(state==5){
                str="歇业";
            }else if(state==6){
                str="注销";
            }else if(state==9){
                str="其他";
            }
            return str ;
        },
        //分类等级
        schoolLevel(level){
            let str = "";
            if(level==1){
                str = "一级";
            }else if(level==2){
                str = "二级";
            }else if(level==3){
                str = "三级";
            }
            return str ;
        }
    },

    //加载组件
    components: {
    },
    //页面激活之后
    activated() {
        this.queryBySchoolCode();
    },
    //页面加载之后
    mounted() {

    }
}

</script>