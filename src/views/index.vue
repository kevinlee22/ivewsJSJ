<style scoped lang="less">
    .index{
        width: 100%;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        text-align: center;
        h1{
            height: 150px;
            img{
                height: 60%;
            }
        }
        h2{
            color: #666;
            margin-bottom: 50px;
            p{
                margin: 0 0 50px;
            }
        }
        
    }
</style>
<template>
    <div class="index">
        <Row type="flex" justify="center" align="middle">
            <Col span="24">
                <h1>
                    <img src="../images/logo.png">
                </h1>
                <h2>
                    <p>Welcome to your iView app!</p>
                </h2>
            </Col>
        </Row>
        <div>
            <v-table
                is-horizontal-resize
                style="width:100%"
                :columns="columns"
                :table-data="tableData"
                row-hover-color="#eee"
                row-click-color="#edf7ff"
                @on-custom-comp="customCompFunc"
            ></v-table>
            <input type="button" value="查看数据" @click="look">
        </div>
    </div>
</template>
<script>
    import Vue from 'vue'

    export default{
        data() {
            return {
                tableData: [
                        {brand:'apple',"name":"赵伟","tel":"156*****1987","hobby":"钢琴、书法、唱歌","address":"上海市黄浦区金陵东路569号17楼"},
                        {brand:'apple',"name":"李伟","tel":"182*****1538","hobby":"钢琴、书法、唱歌","address":"上海市奉贤区南桥镇立新路12号2楼"},
                        {brand:'Mi',"name":"孙伟","tel":"161*****0097","hobby":"钢琴、书法、唱歌","address":"上海市崇明县城桥镇八一路739号"},
                        {brand:'',"name":"周伟","tel":"197*****1123","hobby":"钢琴、书法、唱歌","address":"上海市青浦区青浦镇章浜路24号"},
                        {brand:'',"name":"吴伟","tel":"183*****6678","hobby":"钢琴、书法、唱歌","address":"上海市松江区乐都西路867-871号"}
                     ],
                    columns: [
                        {
                            field: 'custome', title:'序号', width: 50, titleAlign: 'center', columnAlign: 'center',
                            formatter: function (rowData,rowIndex,pagingIndex,field) {
                                return rowIndex < 3 ? '<span style="color:red;font-weight: bold;">' + (rowIndex + 1) + '</span>' : rowIndex + 1
                            }, isFrozen: true,isResize:true
                        },
                        {field: 'name', title:'姓名', width: 80, titleAlign: 'center',columnAlign:'center',isResize:true},
                        {field: 'tel', title: '手机号码', width: 150, titleAlign: 'center',columnAlign:'center',isResize:true},
                        {field: 'hobby', title: '爱好', width: 150, titleAlign: 'center',columnAlign:'center',isResize:true},
                        {field: 'address', title: '地址', width: 230, titleAlign: 'center',columnAlign:'left',isResize:true},
                        {field: 'custome-adv', title: '操作',width: 200, titleAlign: 'center',columnAlign:'center',componentName:'table-operation',isResize:true}
                    ]

            }
        },
        methods:{
            customCompFunc(params){
                var index = params.index;
                params.rowData.brand = params.brand;
                this.tableData.splice(index,1,params.rowData);
            },
            look(){
                console.log(this.tableData)
            }
        }
    }

    // 自定义列组件
    Vue.component('table-operation',{
        template:`
            <Select  @on-change="changeHandle" v-model="rowData.brand"  style="width:200px">
                <Option value="apple">苹果</Option>
                <Option value="HuaWei">华为</Option>
                <Option value="Mi">小米</Option>
            </Select>`,
        props:{
            rowData:{
                type:Object
            },
            field:{
                type:String
            },
            index:{
                type:Number
            }
        },
        data(){
            return {
                data2:''
            }
        },
        methods:{
            changeHandle(value){
               // 参数根据业务场景随意构造
               let params = {brand:value,index:this.index,rowData:this.rowData};
               this.$emit('on-custom-comp',params);
            }
        }
    })
</script>
