<template>
    <section v-loading="pageLoading">
        <el-row class="mb20">
            <el-col :span="20">
                <el-radio-group v-model="dropoutType" @change="dropoutTypeChange">
                    <el-radio-button label="在训学员"></el-radio-button>
                    <el-radio-button label="已退学学员"></el-radio-button>
                </el-radio-group>
            </el-col>
        </el-row>
        <el-row v-show="dropoutType==='在训学员'">
            <!--工具条-->
            <el-row :span="24" class="toolbar">
                <el-form :inline="true" :model="inSchool">
                    <el-row>
                        <el-input class="search-input mr10 left" v-model="inSchool.keyWord" placeholder="输入学员姓名、电话或身份证号码" icon="search" :on-icon-click="inClick"></el-input>
                    </el-row>
                </el-form>
            </el-row>
            <!--列表-->
            <el-row :span="24" class="mt20">
                <el-table :data="inSchool.students" highlight-current-row>
                    <el-table-column prop="studentName" label="姓名">
                    </el-table-column>
                    <el-table-column prop="phone" label="电话">
                    </el-table-column>
                    <el-table-column prop="carTypeName" label="车型">
                    </el-table-column>
                    <el-table-column prop="cardNo" label="身份证号码" width='200'>
                    </el-table-column>
                    <el-table-column prop="signTime" label="报名日期">
                    </el-table-column>
                    <!--<el-table-column prop="recruitUser" label="招生人">
                        </el-table-column>-->
                    <el-table-column label="操作" width="110">
                        <template scope="scope" align="center">
                            <el-button type="text" size="small" @click="outSchoolClick(scope.$index, scope.row)">办理退学</el-button>
                        </template>
                    </el-table-column>
                </el-table>
                <!--工具条-->
                <el-pagination layout="total, sizes, prev, pager, next, jumper" :page-size="pageSize" :total="inSchool.total" :page-sizes="[10, 50, 100, 200, 300, 400]" @current-change="handleCurrentChange" @size-change="handleSizeChange">
                </el-pagination>
            </el-row>
            <!--办理退学-->
            <el-dialog title="办理退学" v-model="outSchoolModel" :close-on-click-modal="false" custom-class="dropOut-win">
                <div class="win-inner">
                    <el-row>
                        <p>为学员办理退学后，该学员将无法参加计时培训、预约培训等各类培训项目，驾校将无法对该学员进行学时审核、阶段记录上报、收费、邀请预约、充值扣费等各类操作。 确认要对以下学员进行退学操作吗？</p>
                    </el-row>
                    <el-row>
                        学员姓名：{{this.dropoutSchool.studentName}}
                    </el-row>
                    <el-row>
                        身份证号码：{{this.dropoutSchool.cardNo}}
                    </el-row>
                    <el-row>
                        <el-input type="textarea" placeholder="填写退学原因" v-model="dropoutSchool.comments" :rows="5" :maxlength="50" resize="none"></el-input>
                    </el-row>
                </div>
                <div slot="footer" class="mb20">
                    <el-button @click.native="outSchoolModel = false" size="large">取消</el-button>
                    <el-button @click="saveDropout" type="primary" size="large">确定</el-button>
                </div>
            </el-dialog>
        </el-row>
        <el-row v-show="dropoutType==='已退学学员'">
            <!--工具条-->
            <el-row :span="24" class="toolbar">
                <el-form :inline="true" :model="outSchool">
                    <el-row>
                        <el-input class="search-input mr10 left" v-model="outSchool.keyWord" placeholder="输入学员姓名、电话或身份证号码" icon="search" :on-icon-click="outClick"></el-input>
                    </el-row>
                </el-form>
            </el-row>
            <!--列表-->
            <el-row :span="24" class="mt20">
                <el-table :data="outSchool.students" highlight-current-row>
                    <el-table-column prop="studentName" label="姓名">
                    </el-table-column>
                    <el-table-column prop="phone" label="电话">
                    </el-table-column>
                    <el-table-column prop="carTypeName" label="车型">
                    </el-table-column>
                    <el-table-column prop="cardNo" label="身份证号码" width='200'>
                    </el-table-column>
                    <el-table-column prop="signTime" label="报名日期">
                    </el-table-column>
                    <el-table-column prop="recruitUser" label="招生人">
                    </el-table-column>
                    <el-table-column prop="outTime" label="退学时间">
                    </el-table-column>
                    <el-table-column prop="transactor" label="办理人">
                    </el-table-column>
                    <el-table-column label="操作" width="110">
                        <template scope="scope" align="center">
                            <el-button type="text" size="small" @click="outSchoolDetail(scope.$index, scope.row)">详情</el-button>
                        </template>
                    </el-table-column>
                </el-table>
                <!--工具条-->
                <el-pagination layout="total, sizes, prev, pager, next, jumper" :page-size="pageSize" :total="outSchool.total" :page-sizes="[10, 50, 100, 200, 300, 400]" @current-change="handleCurrentChange" @size-change="handleSizeChange">
                </el-pagination>
            </el-row>
            <!--办理退学-->
            <el-dialog title="办理退学" v-model="outSchoolDetailModel" :close-on-click-modal="false" size="small">
                <div class="basic mt10" :gutter="10">
                    <el-row>
                        <el-col :span="6">
                            <span>学员姓名：{{this.dropoutDetail.studentName}}</span>
                        </el-col>
                        <el-col :span="6">
                            <span>电话：{{this.dropoutDetail.phone}}</span>
                        </el-col>
                        <el-col :span="6">
                            <span>车型：{{this.dropoutDetail.carTypeName}}</span>
                        </el-col>
                        <el-col :span="6">
                            <span>报名日期：{{this.dropoutDetail.signTime}}</span>
                        </el-col>
                    </el-row>
                      <el-row>
                        <el-col :span="6">
                            <span>招生人：{{this.dropoutDetail.recruitUser}}</span>
                        </el-col>
                        <el-col :span="6">
                            <span>办理人：{{this.dropoutDetail.transactor}}</span>
                        </el-col>
                        <el-col :span="12">
                            <span>退学时间：{{this.dropoutDetail.outTime}}</span>
                        </el-col>
                    </el-row>
                    <el-row>
                        <span>退学原因：{{this.dropoutDetail.comments}}</span>
                    </el-row>
                </div>
                <div slot="footer" class="dialog-footer">
                    <el-button type="primary" @click.native="outSchoolDetailModel = false" size="large">关闭</el-button>
                </div>
            </el-dialog>
        </el-row>
    </section>
</template>

<style lang="scss" scope>
.el-dialog--small {
    width: 1100px;
}

.exam .el-dialog--full {
    width: 1200px;
}

.dropOut-win{
    width: 575px;
    .win-inner{
        margin-top:20px;
        p{
            line-height:25px;
        }
        .el-row{
            &:first-child{
                margin-bottom:20px;
            }
            margin-bottom:15px;
        }
    }
}
</style>

<script>
import { request } from "../../api/api";
import { global } from "../../assets/javascript/global";
export default {
    data() {
        return {
            outSchoolDetailModel: false,
            outSchoolModel: false,
            pageLoading: false,
            dropoutType: "在训学员",
            pageSize: 10,
            schoolCode: JSON.parse(localStorage.getItem("user")).schoolCode,
            inSchool: {
                keyWord: "",
                currentPage: 1,
                total: 0,
                students: [],
            },
            outSchool: {
                keyWord: "",
                currentPage: 1,
                total: 0,
                students: [],
            },
            dropoutSchool: {
                studentName: "",
                cardNo: "",
                comments: "",
            },
            dropoutDetail: {
                studentName: "",
                phone: "",
                carTypeName: "",
                signTime: "",
                recruitUser: "",
                outTime: "",
                transactor: "",
            },

        }
    },
    methods: {
        dropoutTypeChange(val) {
            if (val === "在训学员") {
                this.queryStudents();
            }
            else if (val === "已退学学员") {
                this.queryOutStudents();
            }
        },
        //获取学员列表
        queryStudents() {
            this.pageLoading = true;
            let args = [this.schoolCode, this.inSchool.currentPage, this.pageSize, this.inSchool.keyWord];
            setTimeout(() => {
                request.basic.student.leaveStudent.inSchoolList(args).then((res) => {
                    this.pageLoading = false;
                    if (res.success) {
                        this.inSchool.total = res.object.num;
                        this.inSchool.students = res.object.list;
                    } 
                });
            }, 500);
        },
        //获取退学学员列表
        queryOutStudents() {
            this.pageLoading = true;
            let args = [this.schoolCode, this.outSchool.currentPage, this.pageSize, this.outSchool.keyWord];
            setTimeout(() => {
                request.basic.student.leaveStudent.leaveStudentList(args).then((res) => {
                    this.pageLoading = false;
                    if (res.success) {
                        this.outSchool.total = res.object.num;
                        this.outSchool.students = res.object.list;
                    } 
                });
            }, 500);
        },
        //办理退学
        saveDropout() {
            let args = this.dropoutSchool;
            request.basic.student.leaveStudent.studentLeaveSchool(args).then((res) => {
                if (res.success) {
                    this.outSchoolModel = false;
                    this.dropoutType = "已退学学员";
                    this.queryOutStudents();
                     this.$message.success("学员退学成功！");
                } else {
                    this.$message.error("学员退学失败！");
                }
            });
        },
        //查询按钮
        outClick(){
            this.outSchool.currentPage=1;
            this.queryOutStudents();
        },
         //查询按钮
        inClick(){
            this.inSchool.currentPage=1;
            this.queryStudents();
        },
        //办理退学按钮
        outSchoolClick(index, row) {
            this.outSchoolModel = true;
            this.dropoutSchool.comments = "";
            this.dropoutSchool.studentName = row.studentName;
            this.dropoutSchool.cardNo = row.cardNo;
            this.dropoutSchool.studentId = row.studentId;
        },
        //退学详情按钮
        outSchoolDetail(index, row) {
            this.outSchoolDetailModel = true;
            this.dropoutDetail.studentName = row.studentName ;
            this.dropoutDetail.phone = row.phone ;
            this.dropoutDetail.carTypeName = row.carTypeName ;
            this.dropoutDetail.signTime = row.signTime ;
            this.dropoutDetail.recruitUser = row.recruitUser ;
            this.dropoutDetail.outTime = row.outTime ;
            this.dropoutDetail.transactor = row.transactor ;
            this.dropoutDetail.comments = row.comments;
        },
        //分页
        handleCurrentChange(val) {
            if (this.dropoutType == "已退学学员") {
                this.outSchool.currentPage = val;
                this.queryOutStudents();
            } else {
                this.inSchool.currentPage = val;
                this.queryStudents();
            }
        },
        //分页
        handleSizeChange(val) {
            if (this.dropoutType == "已退学学员") {
                this.pageSize = val;
                this.queryOutStudents();
            } else {
                this.pageSize = val;
                this.queryStudents();
            }
        },
    },
    activated() {
        this.queryStudents();
    },
    mounted() {
    }
}

</script>