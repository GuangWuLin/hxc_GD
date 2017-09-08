<template>
    <section>
        <a href="" download=".xlsx" id="down-link"></a>
        <el-row style="margin-top:20px;">
            <el-col :span="25" class="toolbar">
                <el-form :inline="true">
                    <el-form-item>
                        <el-input class="search-input mr10 left" v-model="keyWord" placeholder="输入学员姓名或身份证"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-date-picker type="daterange" @change="handleDateRangeChange" :clearable='false' placeholder="选择日期范围" v-model="dateRange" format="yyyy-MM-dd" :editable="false">
                        </el-date-picker>
                    </el-form-item>
                    <el-form-item class="right" style="margin-right:49px;">
                        <el-button type="primary" @click="queryDatas">查询</el-button>
                        <el-button type="primary" @click="handleCommand">导出表格</el-button>
                    </el-form-item>
                </el-form>
            </el-col>
        </el-row>
        <el-table :data="tableData" v-loading="pageLoading">
            <el-table-column prop="studentName" label="学员姓名" min-width='80'>
            </el-table-column>
            <el-table-column label="科目一学时" min-width='90'>
                <template scope="scope">
                    <el-progress :text-inside="true" :stroke-width="18" :percentage="scope.row.subject1.completePercent"></el-progress>
                </template>
            </el-table-column>
            <el-table-column prop="subject1.processState" label='签章状态' width='90'>
            </el-table-column>
            <el-table-column prop="subject1.examPass" label='考试成绩'  width='90'>
            </el-table-column>
            <el-table-column label='科目二学时' min-width='90'>
                <template scope="scope">
                    <el-progress :text-inside="true" :stroke-width="18" :percentage="scope.row.subject2.completePercent"></el-progress>
                </template>
            </el-table-column>
            <el-table-column prop="subject2.processState" label='签章状态' width='90' >
            </el-table-column>
            <el-table-column prop="subject2.examPass" label='考试成绩'  width='90' >
            </el-table-column>
            <el-table-column label='科目三学时' min-width='90'>
                <template scope="scope">
                    <el-progress :text-inside="true" :stroke-width="18" :percentage="scope.row.subject3.completePercent"></el-progress>
                </template>
            </el-table-column>
            <el-table-column prop="subject3.processState" label='签章状态' width='90'>
            </el-table-column>
            <el-table-column prop="subject3.examPass" label='考试成绩'  width='90' >
            </el-table-column>
            <el-table-column label='科目四学时' min-width='90'>
                <template scope="scope">
                    <el-progress :text-inside="true" :stroke-width="18" :percentage="scope.row.subject4.completePercent"></el-progress>
                </template>
            </el-table-column>
            <el-table-column prop="subject4.processState" label='签章状态' width='90'>
            </el-table-column>
            <el-table-column prop="subject4.examPass" label='考试成绩'  width='90'>
            </el-table-column>
        </el-table>
        <!--工具条-->
        <el-pagination @current-change="handleCurrentChange" layout="total, prev, pager, next, jumper" :page-size="pageSize" :current-page="page" :total="total">
        </el-pagination>
    </section>
</template>
<script>
import { request } from "../../api/api";
import { global } from "../../assets/javascript/global";
export default {
    data() {
        return {
            dateRange: [],
            keyWord: '',
            page:1,
            pageSize:global.pageSize,
            schoolCode: JSON.parse(localStorage.getItem("user")).schoolCode,
            total:0,
            tableData:[],
            pageLoading:false
            // tableData: [
            //     {
            //         studentName: '青青子衿',
            //         subject1: '40',
            //         signatureStatus: '已签',
            //         score: '通过',
            //         subject2: '50',
            //         signatureStatus2: '已签',
            //         score2: '通过',
            //         subject3: '80',
            //         signatureStatus3: '已签',
            //         score3: '未通过',
            //         subject4: '0',
            //         signatureStatus4: '未签',
            //         score4: '未通过'
            //     }
            // ]
        }
    },
    methods: {
        // 日期变化
        handleDateRangeChange() {
          this.page = 1;  
        },
        // 查询数据
        queryDatas() {
            // console.log(this.dateRange[0].Format('yyyy-MM-dd'));
            this.tableData = [];
            this.pageLoading = true;
            let params = [this.schoolCode,this.page,this.pageSize,this.keyWord,this.dateRange[0].Format('yyyy-MM-dd'),this.dateRange[1].Format('yyyy-MM-dd')];
            // console.log(params);
            request.backstageManagement.studentStageStudyStatistics(params).then(res=>{
                if(res.success){
                    // console.log(res);
                    this.pageLoading = false;
                    this.total = res.object.num;
                    res.object.list.forEach(item=>{
                        this.tableData.push({
                            studentId:item.studentId,
                            studentName:item.studentName,
                            subject1:{
                                completePercent:item.studentStage[0].completePercent,
                                examPass:item.studentStage[0].examPass?'通过':'未通过',
                                processState:item.studentStage[0].processState>50?(item.studentStage[0].checkState ===20?'已合格':'待审核'):item.studentStage[0].processState>=10?'待上报':'未签字',                                
                                stage:item.studentStage[0].stage                          
                            },
                            subject2:{
                                completePercent:item.studentStage[1].completePercent,
                                examPass:item.studentStage[1].examPass?'通过':'未通过',
                                processState:item.studentStage[1].processState>50?(item.studentStage[1].checkState ===20?'已合格':'待审核'):item.studentStage[1].processState>=10?'待上报':'未签字',                                                                
                                stage:item.studentStage[1].stage                          
                            },
                            subject3:{
                                completePercent:item.studentStage[2].completePercent,
                                examPass:item.studentStage[2].examPass?'通过':'未通过',
                                processState:item.studentStage[2].processState>50?(item.studentStage[2].checkState ===20?'已合格':'待审核'):item.studentStage[2].processState>=10?'待上报':'未签字',                                                                                                
                                stage:item.studentStage[2].stage                          
                            },
                            subject4:{
                                completePercent:item.studentStage[3].completePercent,
                                examPass:item.studentStage[3].examPass?'通过':'未通过',
                                processState:item.studentStage[3].processState>50?(item.studentStage[3].checkState ===20?'已合格':'待审核'):item.studentStage[3].processState>=10?'待上报':'未签字',                                                                                                
                                stage:item.studentStage[3].stage                          
                            },
                        });
                    });
                }else{
                    this.$message({
                        type:'warning',
                        message:res.message
                    });
                }
            });

        },
        // 导出数据
        handleCommand() {

        },
        // 初始化时间 当日至三月前
        timeCheck() {
            this.dateRange = [];
            const end = new Date();
            let year = end.getFullYear();
            let month = end.getMonth() + 1;
            let day = end.getDate();
            const start = new Date(`${year}-${month * 1 - 3}-${day}`);
            this.dateRange = [start, end];
        },
        // 翻页
        handleCurrentChange(val){
            this.page = val;
            this.queryDatas();            
        }
    },
    deactivated(){
        this.page = 1;
    },
    mounted(){
        this.timeCheck();
        this.queryDatas();
    }
}
</script>
<style lang='sass'>

</style>


