<!-- 学员上传车管 -->
<template>
    <section>
        <el-row>
            <el-col :span="24" class="toolbar">
                <el-radio-group v-model="pageListSel" class="mb20" @change="handlePageListSelChange">
                    <el-radio-button label="待上报学员"></el-radio-button>
                    <el-radio-button label="待审核学员"></el-radio-button>
                    <el-radio-button label="审核失败学员"></el-radio-button>
                </el-radio-group>
            </el-col>
        </el-row>
        <el-row v-show="pageListSel==='待上报学员'">
            <!--工具条-->
            <el-row :span="24" class="toolbar">
                <el-form :inline="true" :model="tabOne">
                    <el-form-item>
                        <el-input class="search-input mr10 left" v-model="tabOne.keyWord" placeholder="输入学员姓名、电话" icon="search" :on-icon-click="searchData"></el-input>
                    </el-form-item>
                    <el-button type="primary" class="mr20 right" @click.native='allReport' :disabled="!tabOne.multipleSelection.length">批量上报</el-button>
                </el-form>
            </el-row>
            <!--列表-->
            <el-row v-loading="pageLoading" class="mb20">
                <!--列表-->
                <el-table border :data="tabOne.students" @selection-change="handleSelectionChange">
                    <el-table-column type="selection" width="65">
                    </el-table-column>
                    <el-table-column prop="studentName" label="姓名" min-width='80'>
                    </el-table-column>
                    <el-table-column prop="phone" label="电话" width='135'>
                    </el-table-column>
                    <el-table-column prop="carTypeName" label="车型" width='90'>
                    </el-table-column>
                    <el-table-column prop="schoolName" label="分校">
                    </el-table-column>
                    <el-table-column prop="cardNo" label="身份证号" width='200'>
                    </el-table-column>
                    <el-table-column prop="uploadState" label="备案状态" :formatter="formatter">
                    </el-table-column>
                    <el-table-column prop="reportState" label="上报状态" :formatter="formatter">
                    </el-table-column>
                    <el-table-column prop="signTime" label="报名日期" width='120'>
                    </el-table-column>
                    <el-table-column label="操作" width="140">
                        <template scope="scope">
                            <el-button type="text" size="small" @click.native="report(scope.row)">上报车管</el-button>
                        </template>
                    </el-table-column>
                </el-table>
                <!--工具条-->
                <el-pagination class="mt20" @size-change="handleSizeChange" layout="total,sizes,prev,pager,next,jumper" @current-change="handlePageChange" :page-sizes="[10, 50, 100, 200, 300, 400]" :page-size="tabOne.pageSize" :current-page="tabOne.page" :total="tabOne.total">
                </el-pagination>
            </el-row>
        </el-row>
        <el-row v-show="pageListSel==='待审核学员'">
            <!--工具条-->
            <el-row :span="24" class="toolbar">
                <el-form :inline="true" :model="tabTwo">
                    <el-form-item>
                        <el-input class="search-input mr10 left" v-model="tabTwo.keyWord" placeholder="输入学员姓名、电话" icon="search" :on-icon-click="searchData"></el-input>
                    </el-form-item>
                    <el-button type="primary" class="mr20 right" @click.native='allReport' :disabled="!tabTwo.multipleSelection.length">批量办结</el-button>
                </el-form>
            </el-row>
            <!--列表-->
            <el-row v-loading="pageLoading" class="mb20">
                <!--列表-->
                <el-table border :data="tabTwo.students" @selection-change="handleSelectionChange">
                    <el-table-column type="selection" width="65">
                    </el-table-column>
                    <el-table-column prop="studentName" label="姓名" min-width='80'>
                    </el-table-column>
                    <el-table-column prop="phone" label="电话" width='135'>
                    </el-table-column>
                    <el-table-column prop="carTypeName" label="车型" width='90'>
                    </el-table-column>
                    <el-table-column prop="schoolName" label="分校">
                    </el-table-column>
                    <el-table-column prop="cardNo" label="身份证号" width='200'>
                    </el-table-column>
                    <el-table-column prop="uploadState" label="备案状态" :formatter="formatter">
                    </el-table-column>
                    <el-table-column prop="reportState" label="上报状态" :formatter="formatter">
                    </el-table-column>
                    <el-table-column prop="signTime" label="报名日期" width='120'>
                    </el-table-column>
                    <el-table-column label="操作" width="140">
                        <template scope="scope">
                            <el-button type="text" size="small" @click.native="report(scope.row)">车管办结</el-button>
                            <el-button type="text" size="small" @click.native="reject(scope.row)">审核失败</el-button>
                        </template>
                    </el-table-column>
                </el-table>
                <!--工具条-->
                <el-pagination class="mt20" @size-change="handleSizeChange" layout="total,sizes,prev,pager,next,jumper" @current-change="handlePageChange" :page-sizes="[10, 50, 100, 200, 300, 400]" :page-size="tabTwo.pageSize" :current-page="tabTwo.page" :total="tabTwo.total">
                </el-pagination>
            </el-row>
        </el-row>
        <el-row v-show="pageListSel==='审核失败学员'">
            <!--工具条-->
            <el-row :span="24" class="toolbar">
                <el-form :inline="true" :model="tabThree">
                    <el-form-item>
                        <el-input class="search-input mr10 left" v-model="tabThree.keyWord" placeholder="输入学员姓名、电话" icon="search" :on-icon-click="searchData"></el-input>
                    </el-form-item>
                    <el-button type="primary" class="mr20 right" @click.native='allReport' :disabled="!tabThree.multipleSelection.length">批量重报</el-button>
                </el-form>
            </el-row>
            <!--列表-->
            <el-row v-loading="pageLoading" class="mb20">
                <!--列表-->
                <el-table border :data="tabThree.students" @selection-change="handleSelectionChange">
                    <el-table-column type="selection" width="65">
                    </el-table-column>
                    <el-table-column prop="studentName" label="姓名" min-width='80'>
                    </el-table-column>
                    <el-table-column prop="phone" label="电话" width='135'>
                    </el-table-column>
                    <el-table-column prop="carTypeName" label="车型" width='90'>
                    </el-table-column>
                    <el-table-column prop="schoolName" label="分校">
                    </el-table-column>
                    <el-table-column prop="cardNo" label="身份证号" width='200'>
                    </el-table-column>
                    <el-table-column prop="uploadState" label="备案状态" :formatter="formatter">
                    </el-table-column>
                    <el-table-column prop="reportState" label="上报状态" :formatter="formatter">
                    </el-table-column>
                    <el-table-column prop="signTime" label="报名日期" width='120'>
                    </el-table-column>
                    <el-table-column label="操作" width="140">
                        <template scope="scope">
                            <el-button type="text" size="small" @click.stop="report(scope.row)">重报车管</el-button>
                        </template>
                    </el-table-column>
                </el-table>
                <!--工具条-->
                <el-pagination class="mt20" layout="total,sizes,prev,pager,next,jumper" @size-change="handleSizeChange" @current-change="handlePageChange" :page-sizes="[10, 50, 100, 200, 300, 400]" :page-size="tabThree.pageSize" :total="tabThree.total">
                </el-pagination>
    
            </el-row>
        </el-row>
        <el-dialog size="tiny" :close-on-click-modal='false' title="车管审核失败，选择原因" :visible.sync="tabTwo.dialogFormVisible">
            <el-form :model="tabTwo">
                <el-select v-model="tabTwo.region">
                    <el-option label="近三年内有吸毒史" value="近三年内有吸毒史"></el-option>
                    <el-option label="照片无法识别" value="照片无法识别"></el-option>
                    <el-option label="身份信息有误" value="身份信息有误"></el-option>
                    <el-option label="其他" value="其他"></el-option>
                </el-select>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="tabTwo.dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="ensureReject">确 定</el-button>
            </div>
        </el-dialog>
    </section>
</template>

<script>
import $ from "jquery";
import { request } from "../../api/api";
import { global } from "../../assets/javascript/global";
export default {
    data() {
        return {
            tabOne: {
                keyWord: "",
                total: 0,
                students: [],
                page: 1,
                state: 30,
                pageSize: global.pageSize,
                multipleSelection: []
            },
            tabTwo: {
                keyWord: "",
                total: 0,
                students: [],
                page: 1,
                state: 40,
                pageSize: global.pageSize,
                multipleSelection: [],
                region: '近三年内有吸毒史',
                dialogFormVisible: false,
                refuse: ''
            },
            tabThree: {
                keyWord: "",
                total: 0,
                students: [],
                pageSize: global.pageSize,
                page: 1,
                multipleSelection: [],
                state: 60
            },
            pageLoading: false,
            pageListSel: "待上报学员",
            userInfo: JSON.parse(localStorage.getItem("user"))
        }
    },
    methods: {
        reject(row) {
            this.tabTwo.dialogFormVisible = true;
            this.tabTwo.refuse = row;
        },
        ensureReject() {
            let students = [];
            let para = {
                studentId: this.tabTwo.refuse.studentId,
                reportState: 60,
                transactorId: this.userInfo.userId,
                transactor: this.userInfo.name,
                comments: this.tabTwo.region
            }
            students.push(para);
            request.backstageManagement.report.updateReportStateBatch(students).then(res => {
                if (res.success) {
                    this.tabTwo.dialogFormVisible = false;
                    this.getStudentsByReporting();
                    this.$message({
                        type: 'success',
                        message: '成功驳回'
                    });
                } else {
                    this.$message({
                        type: 'warning',
                        message: res.message
                    })
                }
            });
        },
        // 点击搜索框查询
        searchData() {
            this.getStudentsByReporting();
        },
        // 选择学员
        handleSelectionChange(val) {
            this.pageListSel === '待上报学员' ? (this.tabOne.multipleSelection = val) : this.pageListSel === '待审核学员' ? (this.tabTwo.multipleSelection = val) : (this.tabThree.multipleSelection = val);
        },
        //表格数据格式化
        formatter(row, column) {
            if (column.property === "uploadState") {
                return global.enums.uploadState[row.uploadState];
            }
            else if (column.property === "reportState") {
                return global.enums.reportState[row.reportState];
            }
        },
        // 选择每页展示数据的数量
        handleSizeChange(val) {
            this.pageListSel === '待上报学员' ? (this.tabOne.pageSize = val) : this.pageListSel === '待审核学员' ? (this.tabTwo.pageSize = val) : (this.tabThree.pageSize = val);
            this.getStudentsByReporting();
        },
        // 单独点每一行进行备案 或 其他
        report(row) {
            let students = [];
            let para = {
                studentId: row.studentId,
                reportState: this.pageListSel === '待审核学员' ? 50 : 40,
                transactorId: this.userInfo.userId,
                transactor: this.userInfo.name
            }
            students.push(para);
            request.backstageManagement.report.updateReportStateBatch(students).then(res => {
                if (res.success) {
                    this.$message({
                        type: 'success',
                        message: this.pageListSel === '待上报学员' ? '上报车管成功' : this.pageListSel === '待审核学员' ? '车管办结成功' : '重报车管成功'
                    });
                    this.getStudentsByReporting();
                } else {
                    this.$message({
                        type: 'warning',
                        message: res.message
                    })
                }
            });
        },
        allReport() {
            // console.log(1);
            let students = [];
            if (this.pageListSel === '待上报学员') {
                if (this.tabOne.multipleSelection.length) {
                    this.tabOne.multipleSelection.forEach(item => {
                        students.push({
                            studentId: item.studentId,
                            reportState: 40,
                            transactorId: this.userInfo.userId,
                            transactor: this.userInfo.name
                        });
                    });

                }
            } else if (this.pageListSel === '待审核学员') {
                if (this.tabTwo.multipleSelection.length) {
                    this.tabTwo.multipleSelection.forEach(item => {
                        students.push({
                            studentId: item.studentId,
                            reportState: 50,
                            transactorId: this.userInfo.userId,
                            transactor: this.userInfo.name
                        });
                    });
                }
            } else {
                if (this.tabThree.multipleSelection.length) {
                    this.tabThree.multipleSelection.forEach(item => {
                        students.push({
                            studentId: item.studentId,
                            reportState: 40,
                            transactorId: this.userInfo.userId,
                            transactor: this.userInfo.name
                        });
                    });
                }
            }
            request.backstageManagement.report.updateReportStateBatch(students).then(res => {
                if (res.success) {
                    this.getStudentsByReporting();
                    this.$message({
                        type: 'success',
                        message: this.pageListSel === '待上报学员' ? '批量上报成功' : this.pageListSel === '待审核学员' ? '批量办结成功' : '批量重报成功'
                    });
                } else {
                    this.$message({
                        type: 'warning',
                        message: res.message
                    })
                }
            });
        },
        //获取用户列表
        getStudentsByReporting() {
            this.pageLoading = true;
            let args = this.pageListSel === '待上报学员' ? [this.userInfo.schoolCode, this.tabOne.state, this.tabOne.keyWord, this.tabOne.page, this.tabOne.pageSize] : this.pageListSel === '待审核学员' ? [this.userInfo.schoolCode, this.tabTwo.state, this.tabTwo.keyWord, this.tabTwo.page, this.tabTwo.pageSize] : [this.userInfo.schoolCode, this.tabThree.state, this.tabThree.keyWord, this.tabThree.page, this.tabThree.pageSize];
            setTimeout(() => {
                request.backstageManagement.report.studentReportList(args).then(res => {
                    this.pageLoading = false;
                    global.printLog(res);
                    if (res.success) {
                        if (this.pageListSel === '待上报学员') {
                            this.tabOne.total = res.object.num;
                            this.tabOne.students = res.object.list;
                        } else if (this.pageListSel === '待审核学员') {
                            this.tabTwo.total = res.object.num;
                            this.tabTwo.students = res.object.list;
                        } else {
                            this.tabThree.total = res.object.num;
                            this.tabThree.students = res.object.list;
                        }
                    } else {
                        this.$message({
                            type: 'warning',
                            message: res.message
                        })
                    }
                });
            }, 1000);
        },
        //翻页查询
        handlePageChange(val) {
            this.pageListSel === '待上报学员' ? (this.tabOne.page = val) : this.pageListSel === '待审核学员' ? (this.tabTwo.page = val) : (this.tabThree.page = val);
            this.getStudentsByReporting();
        },
        handlePageListSelChange(val) {
            this.getStudentsByReporting();
        }
    },
    activated() {
        this.getStudentsByReporting();
    },
    //页面挂载
    mounted() {
    }
}

</script>

<style lang="scss">

</style>