<template>
    <section>
        <div>
            <el-radio-group v-model="radioHeaderSel" @change="teacherTypeChange">
                <!--<el-radio-button label="培训汇总"></el-radio-button>-->
                <el-radio-button label="理论教室"></el-radio-button>
                <el-radio-button label="模拟教室"></el-radio-button>
            </el-radio-group>
            <!--理论 模拟切换-->
        </div>
        <!--模拟和理论所有字段相同-->
        <div>
            <div class='titleCount'>
                <span>{{radioHeaderSel === '理论教室'?`理论车总数：${classCount}`:`模拟车总数：${simulationCount}`}}</span>
                <span style='margin-left:100px;'>在训数：{{ radioHeaderSel === '理论教室'?classTrainCount:simulationTrainCount}}</span>
            </div>
            <!--queryData使用计算属性 根据 radioHeaderSel不同切换不同的数组-->
            <div class='cardList' v-if='queryData.length'>
                <div class='cards' v-for='(item,index) in queryData' :key='index'>
                    <el-row>
                        <el-col class='cardTitle' :span="8">
                            <span>{{radioHeaderSel === '理论教室'?`理论车牌号：${item.carNo}`:`模拟车牌号：${item.carNo}`}}</span>
                        </el-col>
                        <el-col class='cardTitle' :span="8">
                            <span :style="{color:item.isOnline?(item.isOnline===2?'#169BD5':'#FFCC00'):'#999'}">{{item.isOnline?(item.isOnline===2?'培训中':'待机中'):'已离线'}}</span>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col class='cardContent' :span="8">
                            <span>当前教练：{{item.teacherName?item.teacherName:'--'}}</span>
                        </el-col>
                        <el-col class='cardContent' :span="8">
                            <span>签到时间：{{item.signInTime?new Date(item.signInTime*1).Format('yyyy-MM-dd HH:mm:ss'):'--'}}</span>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col class='cardContent' :span="8">
                            <span>当前签到人数：{{item.training}}</span>
                        </el-col>
                        <el-col class='cardContent' :span="8">
                            <span>今日签到总数：{{item.totalTraining}}</span>
                        </el-col>
                        <el-col class='cardContent' :span="6">
                            <span>今日培训总分钟数：{{item.totalTrainMins}}</span>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col>
                            <el-button type='text' class='right mr20' @click.native='btnClick(item)'>查看详情</el-button>
                        </el-col>
                    </el-row>
                </div>
            </div>
            <div class="glyph-icon icon-empty empty-data mt20 noDatas" v-else>
                <p> 您的驾校尚未开展这项业务，联系成都网阔公司了解如何开始</p>
            </div>
        </div>
        <!--当日培训记录详情-->
        <el-dialog title="当日培训记录" :visible.sync="dialogFormVisible" :close-on-click-modal='false'>
            <el-table :data='trainRecord' style='margin:10px 0;'>
                <el-table-column prop="signInTime" label="签到时间" >
                </el-table-column>
                <el-table-column label="签退时间" prop="signOutTime" >
                </el-table-column>
                <el-table-column prop="teacherName" label='教练'>
                </el-table-column>
                <el-table-column prop="studentName" label='学员'>
                </el-table-column>
                <el-table-column prop="studyTime" label='分钟数'>
                </el-table-column>
            </el-table>
            <div v-if='trainRecord.length' style='width:100%;text-align:center;height:40px;line-height:40px;cursor:pointer;' @click='addMore'>加载更多 ...</div>
        </el-dialog>
    </section>
</template>
<script>
import { request } from "../../api/api";
import { global } from "../../assets/javascript/global";
export default {
    data() {
        return {
            radioHeaderSel: '理论教室',
            dialogFormVisible: false,
            classCount: 0,
            classTrainCount: 0,
            theories:[],
            // theories: [
            //     {
            //         carNo: '川1234理',
            //         teacherName: 'jack',
            //         signInTime: '2017-01-02',
            //         training: 12,
            //         totalTraining: 20,
            //         totalTrainMins: 4512,
            //         isOnline: 0
            //     },
            //     {
            //         carNo: '川1233理',
            //         teacherName: 'nicholas',
            //         signInTime: '2017-01-05',
            //         training: 14,
            //         totalTraining: 54,
            //         totalTrainMins: 4545,
            //         isOnline: 1
            //     },
            //     {
            //         carNo: '川1232理',
            //         teacherName: 'parton',
            //         signInTime: '2017-01-07',
            //         training: 17,
            //         totalTraining: 66,
            //         totalTrainMins: 4554,
            //         isOnline: 2
            //     }
            // ],
            simulationCount: 0,
            simulationTrainCount: 0,
            simulations:[],
            // simulations: [
            //     {
            //         carNo: '川1234模',
            //         teacherName: 'lussy',
            //         signInTime: '2017-01-12',
            //         training: 12,
            //         totalTraining: 14,
            //         totalTrainMins: 4500,
            //         isOnline: 2
            //     },
            //     {
            //         carNo: '川1233模',
            //         teacherName: 'peter',
            //         signInTime: '2017-01-06',
            //         training: 14,
            //         totalTraining: 51,
            //         totalTrainMins: 4145,
            //         isOnline: 1
            //     },
            //     {
            //         carNo: '川1232模',
            //         teacherName: 'herry',
            //         signInTime: '2017-01-08',
            //         training: 17,
            //         totalTraining: 64,
            //         totalTrainMins: 4254,
            //         isOnline: 0
            //     }
            // ],
            trainRecord:[],
            // trainRecord: [
            //     {
            //         time: '2017-01-01 12:33:45',
            //         type: '教练',
            //         name: 'jack',
            //         behavior: '签到',
            //         operation: '查看照片'
            //     },
            //     {
            //         time: '2017-01-03 11:33:45',
            //         type: '学员',
            //         name: 'mike',
            //         behavior: '签到',
            //         operation: '查看学时'
            //     },
            //     {
            //         time: '2017-01-08 15:43:55',
            //         type: '学员',
            //         name: 'Tom',
            //         behavior: '签到',
            //         operation: '查看学时'
            //     },
            //     {
            //         time: '2017-01-11 08:23:41',
            //         type: '教练',
            //         name: 'Romel',
            //         behavior: '签到',
            //         operation: '查看照片'
            //     }
            // ],
            schoolCode: JSON.parse(localStorage.getItem("user")).schoolCode
        }
    },
    computed: {
        queryData: function() {
            return this.radioHeaderSel === '理论教室' ? this.theories : this.simulations
        }
    },
    methods: {
        // 理论模拟切换
        teacherTypeChange() {
            // console.log(this.radioHeaderSel);
        },
        // 点击行内 详情按钮
        btnClick(item) {
            this.dialogFormVisible = true;
            this.queryDetailBySim(item.sim);
            // console.log(item);
        },
        // 弹出框中加载更多当日培训记录
        addMore() {
            // console.log('add more');
        },
        // 获取理论教室和模拟教室的全部数据
        queryAllDatas() {
            request.timeTraining.classRoomList(this.schoolCode).then(res => {
                // console.log('=====')
                console.log(res);
                if (res.success) {
                    this.classCount = res.object.classCarCount;
                    this.classTrainCount = res.object.classTrainCount;
                    this.theories = res.object.classCarList;
                    this.simulationCount = res.object.simulatorCarCount;
                    this.simulationTrainCount = res.object.simulatorTrainCount;
                    this.simulations = res.object.simulatorCarList;
                } else {
                    this.$message({
                        type: 'warning',
                        message: '请求数据失败，原因： ' + res.message
                    });
                }
            });
        },
        // 根据sim号获取详情
        queryDetailBySim(params){
            this.trainRecord = [];
            request.timeTraining.studentSignToday(params).then(res => {
                // console.log('=====')
                console.log(res);
                if (res.success) {
                    // this.trainRecord = res.object;
                    res.object.forEach(item=>{
                        this.trainRecord.push({
                            signInTime:new Date(item.signInTime*1).Format('yyyy-MM-dd HH:mm:ss'),
                            signOutTime:new Date(item.signOutTime*1).Format('yyyy-MM-dd HH:mm:ss'),
                            sim:item.sim,
                            studentName:item.studentName?item.studentName:'--',
                            teacherName:item.teacherName?item.teacherName:'--',
                            studyTime:item.studyTime?item.studyTime:'--'
                        })
                    });
                } else {
                    this.$message({
                        type: 'warning',
                        message: '请求数据失败，原因： ' + res.message
                    });
                }
            });
        }
    },
    activated() {
        this.queryAllDatas();
    }
}
</script>
<style lang='scss'>
.titleCount {
    float: left;
    margin: 10px auto;
    font-size: 20px;
    font-weight: bold;
}

.cardList {
    clear: both;
    .cards {
        width: 100%;
        margin: 20px 0px;
        background: #F4F6F8;
        box-shadow: 5px 5px 5px -5px #99A9BF;
        border-radius: 5px;
        font-size: 16px;
        .cardTitle,
        .cardContent {
            margin: 10px 10px;
        }
        .cardTitle {
            font-weight: bold;
        }
        .cardContent {}
    }
}

.noDatas {
    clear: both;
}

.dialogTitle,
.dialogContent {
    margin: 10px 10px;
}
</style>


