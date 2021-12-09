<template>
  

            <div id="Homer" style="margin-top: 10px;">
                <el-container>
                    <el-aside style="height:120px;width: 260px;padding-top: 20px;padding-left: 5px;">
                        <div>
                            <el-input class="search-icon" icon="el-icon-search" placeholder="搜索名称" v-model="search" style="width: 74%;text-align: left;">
                            </el-input>
                            <el-button icon="el-icon-refresh" style="font-size: 5%;"></el-button>
                        </div>
                    </el-aside>
                    <el-container>
                        <el-header style="height:auto;text-align: left;padding-top: 20px;">
                        <el-form :model="form" :rules="formrules"   style="height:auto"  ref="form"  :show-message="false">
                          <el-form-item > 
                           <el-button type="warning"  plain @click="newModule" style="margin-left: 15px;">新建模块</el-button>
                            <el-button type="success" plain @click="inquire" style="margin-left: 15px;">选择条件</el-button>
                                    <el-dialog title="新建模块" :visible.sync="ModuleformFormVisible"  style="width: 35%; ">
                                        <div style="margin-left: 20px;font-size: 13px;">
                                            <el-form :model="Moduleform"   ref="Moduleform"  :style="{display: Moduleformdisplay}"  :show-message="false" class="demo-ruleForm">
                                                <el-form-item label="id:" :label-width="ModuleformformLabelWidth" style="font-weight: 600;margin-bottom: 20px; ">
                                                    <span>0</span>
                                                </el-form-item>
                                                <el-form-item label="BI_名称:" :label-width="ModuleformformLabelWidth" style="width: 75%;font-weight: 600; margin-bottom: 20px;" prop="Module" :rules="[
                                                { required: true}]">
                                                    <el-input v-model="Moduleform.Module" autocomplete="off"></el-input>
                                                </el-form-item>
                                                <el-form-item label="父级_ID:" :label-width="ModuleformformLabelWidth" style="width: 75%;font-weight: 600; margin-bottom: 20px; " prop="Parent_ID" :rules="[
                                                { required: true}]">
                                                    <el-input v-model="Moduleform.Parent_ID" autocomplete="off"></el-input>
                                                </el-form-item>
                                                <el-form-item label="排序号:" :label-width="ModuleformformLabelWidth" style="width: 75%; ">
                                                    <el-input v-model="Moduleform.Queue_number" autocomplete="off"></el-input>
                                                </el-form-item>
                                            </el-form>
                                        </div>
                                    
                                        <div slot="footer" class="dialog-footer">
                                            <el-button type="primary"  @click="ModuleSubmit">保存</el-button>
                                        </div>
                                    </el-dialog>
                                   
                          </el-form-item>
                          
                          <el-form-item    prop="checkList"  :style="{display: checkListdisplay}" >
                           <el-dialog title="选择条件" :visible.sync="dialogFormVisible" style="width: 40%;">                  
                                    <el-checkbox-group v-model="form.checkList">
                                  <el-checkbox v-for="check in checkLists" :label="check.name" :key="check.id" border   :disabled="checkdisabled" style="margin: 10px 30px 0px 15px;">{{check.name}}</el-checkbox>  
                                    </el-checkbox-group> 
                                        <div slot="footer" class="dialog-footer">
                                            <el-button type="primary" @click="onSubmit()">确定</el-button>
                                        </div>
                                   
                                    </el-dialog>
                                   
                                      </el-form-item>
                               <el-form-item  :style="{display:Condition_querysdisplay}"> 
                             
                                     <el-form :model="Condition_query"  :inline="true" :style="{display: Condition_querydisplay}" :show-message="false"  ref="Condition_query">  

                                    <el-form-item  prop="storeName"  style="margin-left: 15px;" :rules="[{required:this.form.storeName===true?true:false}]"   :style="{display: storeNamedisplay}" >
                                      <el-select v-show="form.storeName === true" v-model="Condition_query.storeName"    placeholder="仓库名称" >
                                        <el-option v-for="item in storeNamedata" :key="item.value" :label="item.labelname" :value="item.value">
                                        </el-option>
                                    </el-select>   
                                      </el-form-item>   
                                                                     
                                     <el-form-item prop="consignor" style="margin-left: 15px;"   :rules="[{required:this.form.consignor===true?true:false}]" :style="{display: consignordisplay}"> 
                                    <el-select v-show="form.consignor === true" v-model="Condition_query.consignor" multiple  collapse-tags  placeholder="货主名称">
                                        <el-option v-for="item in consignordata" :key="item.value" :label="item.label" :value="item.value">
                                        </el-option>
                                    </el-select>  
                                    </el-form-item>

                                  <el-form-item prop="unitdocuments" style="margin-left: 15px;" :rules="[{required:this.form.unitdocuments===true?true:false}]" :style="{display: unitdocumentsdisplay}"> 
                                    <el-select v-show="form.unitdocuments === true" v-model="Condition_query.unitdocuments" multiple  collapse-tags  placeholder="单据单位">
                                        <el-option v-for="item in unitdocumentsdata" :key="item.value" :label="item.label" :value="item.value">
                                        </el-option>
                                    </el-select>  
                                        </el-form-item>
                                     
                                     
                                    <el-form-item prop="Operator" style="margin-left: 15px;" :rules="[{required:this.form.Operator===true?true:false}]" :style="{display: Operatordisplay}"> 
                                    <el-select v-show="form.Operator === true" v-model="Condition_query.Operator" multiple collapse-tags  placeholder="操作人">
                                        <el-option v-for="item in Operatordata" :key="item.value" :label="item.label" :value="item.value">
                                        </el-option>
                                    </el-select>
                                    </el-form-item>
                                    <el-form-item prop="product" style="margin-left: 15px;" :rules="[{required:this.form.product===true?true:false}]" :style="{display: productdisplay}"> 
                                    <el-select v-show="form.product === true"  v-model="Condition_query.product" multiple collapse-tags  placeholder="商品名称">
                                        <el-option v-for="item in productdata" :key="item.value" :label="item.label" :value="item.value">
                                        </el-option>
                                    </el-select>  
                                    </el-form-item>
                                   
                                       <el-form-item prop="value1" style="margin-left: 15px;" :rules="[{required:this.form.value1===true?true:false}]" :style="{display: value1display}">                             
                                    <el-date-picker v-show="form.value1 === true" v-model="Condition_query.value1" type="daterange"  range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期">
                                    </el-date-picker>    
                                    </el-form-item>
                                     <el-form-item prop="Time" style="margin-left: 15px;" :rules="[{required:this.form.Time===true?true:false}]" :style="{display: Timedisplay}"> 
                                     <el-date-picker v-show="form.Time === true"  v-model="Condition_query.Time" type="date"  placeholder="选择日期"></el-date-picker>
                                    </el-form-item>
                                    <el-form-item prop="years" style="margin-left: 15px;" :rules="[{required:this.form.years===true?true:false}]" :style="{display: yearsdisplay}"> 
                                      <el-date-picker v-show="form.years === true" v-model="Condition_query.years" type="year"  placeholder="选择年"></el-date-picker>
                                    </el-form-item>

                                   <el-form-item prop="month" style="margin-left: 15px;" :rules="[{required:this.form.month===true?true:false}]" :style="{display: monthdisplay}"> 
                                      <el-date-picker v-show="form.month === true" v-model="Condition_query.month" type="month"  placeholder="选择月"></el-date-picker>
                                     
                                    </el-form-item>
                                    
                                      <el-form-item :style="{display: searchs }"  style="margin-left: 15px;"> 
                                      <el-button type="primary" icon="el-icon-search" style="margin-left: 15px;" :style="{display: searchs }" @click="Submitquery(Condition_query)">查询</el-button>
                                    <el-button type="primary" icon="el-icon-delete" style="margin-left: 15px;" :style="{display: deletes }" @click="Reset">重置</el-button>
                                      </el-form-item>                                      
                                    </el-form>                                  
                      </el-form-item> 
                        </el-form>
                        </el-header>
                        <el-main>
                          <iframe src="http://ureport.honglian.hlscm.cn/renren-admin/ureport/designer" frameborder="0" scrolling="auto" style="width: 100%; height: 1077px;"></iframe>
                        </el-main>
                    </el-container>
                </el-container>
            </div>
        
        </template>

<script>

    export default {
        name: 'HelloWorld',
        data() {
            return {
                dialogFormVisible: false,
                ModuleformFormVisible: false,
                searchs: 'none',
                deletes: 'none',
                search: '',
                Condition_querydisplay:'none',
                Condition_querysdisplay:'none',
                storeNamedisplay:'none',
                unitdocumentsdisplay:'none',
                consignordisplay:'none',
                productdisplay:'none',
                Operatordisplay:'none',
                value1display:'none',
                Timedisplay:'none',
                yearsdisplay:'none',
                monthdisplay:'none',
                Moduleformdisplay:'none',
                checkListdisplay:'none',
                checkdisabled:false,
             
                Condition_querydata:[                                     
                    {
                        storeName:0,
                        placeholder:'货主名称',  
                        required:'consignor',                  
                        Condition_option: [
                            {
                            value: '测试货主',
                            label: '测试货主'
                            }, 
                            {
                            value: '测试货主1',
                            label: '测试货主1'
                            }
                            ],
                    },
                    {
                        id:1,
                        placeholder:'单据单位',  
                        required:'unitdocuments',                  
                        Condition_option: [
                            {
                            value: '测试货主',
                            label: '测试货主'
                            }, 
                            {
                            value: '测试货主1',
                            label: '测试货主1'
                            }
                            ],
                    },
                    {
                        id:2,
                        placeholder:'操作人名',  
                        required:'Operator',                  
                        Condition_option: [
                            {
                            value: '测试货主',
                            label: '测试货主'
                            }, 
                            {
                            value: '测试货主1',
                            label: '测试货主1'
                            }
                            ],
                    },
                    {
                        id:3,
                        placeholder:'商品名称',  
                        required:'product',                  
                        Condition_option: [
                            {
                            value: '测试货主',
                            label: '测试货主'
                            }, 
                            {
                            value: '测试货主1',
                            label: '测试货主1'
                            }
                            ],
                    },
                    
                    ],
               


                /*日期组件*/
                pickerOptions: {
                    shortcuts: [{
                        text: '最近一周',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                            picker.$emit('pick', [start, end]);
                        }
                    }, {
                        text: '最近一个月',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                            picker.$emit('pick', [start, end]);
                        }
                    }, {
                        text: '最近三个月',
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                            picker.$emit('pick', [start, end]);
                        }
                    }]
                },
                /** checkList数据源*/
                checkLists :[
                    { id:'1',name:'仓库名称'}, 
                    { id:'2',name:'货主名称'}, 
                    { id:'3',name:'日期范围'}, 
                    { id:'4',name:'单据单位'}, 
                    { id:'5',name:'操作人名'}, 
                    { id:'6',name:'商品名称'}, 
                    { id:'7',name:'选择日期'}, 
                    { id:'8',name:'选择年份'}, 
                    { id:'9',name:'选择月份'}
                    ],
                   
                /* 选择仓库名称*/
               
                /* 选择单据单位*/
                unitdocumentsdata: [{
                    value: '测试单位',
                    label: '测试单位'
                }, {
                    value: '测试单位1',
                    label: '测试单位1'
                }],
            

                /* 选择货主名称*/
                consignordata: [{
                    value: '测试货主',
                    label: '测试货主'
                }, {
                    value: '测试货主1',
                    label: '测试货主1'
                }],
              
storeNamedata: [{
                    value: '测试仓库',
                    label: '测试仓库'
                }, {
                    value: '测试仓库1',
                    label: '测试仓库1'
                }],
                /* 选择商品名称*/
                productdata: [{
                    value: '测试商品',
                    label: '测试商品'
                }, {
                    value: '测试商品1',
                    label: '测试商品1'
                }],
              

                /* 选择操作人*/
                Operatordata: [{
                    value: '测试人员',
                    label: '测试人员'
                }, {
                    value: '测试人员1',
                    label: '测试人员1'
                }],
              
              

                /*新建模块*/
                Moduledata: [{}],
                /*父级Id*/
                Parent_ID: [{}],
                /*排序号*/
                Queue_number: [{}],
               

                /*表单form模型*/
                
                form: {                          
                    checkList: [],   
                               
                },
                
                /**表单Condition_query模型 */
                Condition_query:{                    
                  
                    storeName: '',  
                   consignor:[],
                    Operator: '',
                    product: '',
                    unitdocuments:[],
                       
                    value1: '',
                    value2: '',
                    years: '',
                    month: '',
                    Time: '',
                    },
                /**效验查询条件是否被选中 */
                 formrules: {   
                    checkList: [{                    
                        required: true,
                        trigger: "blur",
                    }]                                                   
                },
                /*新建模块*/
                Moduleform: {

                    Module: '',
                    Parent_ID: 0,
                    Queue_number: 0,
                },
                formLabelWidth: '120px',
                ModuleformformLabelWidth: '100px',
            };
        },
        methods: {
            
            newModule(){
                 this.ModuleformFormVisible = true;
                 this.Moduleformdisplay='';
            },

handleCheckChange(val){


     
},
           
            inquire(){
this.dialogFormVisible = true;
 this.checkListdisplay='';
            },

getChange(ss,yy){console.log(ss,yy)},
            /**提交查询条件 */
            onSubmit() {
                this.$refs["form"].validate((valid, msg) => {
                    if (valid) {
                        //关闭下拉框
                        this.dialogFormVisible = false;
                        this. checkListdisplay='none';
                        this.Condition_querydisplay='';
                          this.Condition_querysdisplay='';
                       
                        //取出选中条件的值并根据值显示文本框
                        for (var i = 0; i < this.form.checkList.length; i++) {
                            var checkvalue = this.form.checkList[i];
                            switch (checkvalue) {
                                case '仓库名称':
                                    this.form.storeName= true ;   
                                    this.storeNamedisplay='';                             
                                    break;
                                case '货主名称':
                                    this.form.consignor = true;
                                     this.consignordisplay='';  
                                    break;
                                case '单据单位':
                                    this.form.unitdocuments = true;
                                     this.unitdocumentsdisplay='';  
                                    break;
                                case '日期范围':
                                    this.form.value1 = true;
                                     this.value1display='';  
                                    break;
                                case '操作人名':
                                    this.form.Operator = true;
                                     this.Operatordisplay='';  
                                    break;
                                case '商品名称':
                                    this.form.product = true;
                                     this.productdisplay='';  
                                    break;
                                case '选择日期':
                                    this.form.Time = true;
                                     this.Timedisplay='';  
                                    break;
                                case '选择年份':
                                    this.form.years = true;
                                     this.yearsdisplay='';  
                                    break;
                                case '选择月份':
                                    this.form.month = true;
                                    this.monthdisplay='';  
                                    break;
                            }
                          
                        }
                        this.searchs = '';
                        this.deletes = '';
                        this.$message({
                            message: '查询条件更新成功',
                            type: 'success'
                        });
                    } else {
                        this.$message.error('至少选择一个查询条件');


                    }
                });
            },
            /*重置文本框内容为空*/
            Reset() {
                //清楚提示信息
                this.$nextTick(()=>{this.$refs.Condition_query.resetFields();});
                //对表单Condition_query的内容重新赋值
                this.Condition_query.storeName = '';
                this.Condition_query.value1 = '';
                this.Condition_query.value2 = '';
                this.Condition_query.unitdocuments = '';
                this.Condition_query.consignor = '';
                this.Condition_query.product = '';
                this.Condition_query.Operator = '';
                this.Condition_query.years = '';
                this.Condition_query.month = '';
                this.Condition_query.Time = '';
            },

            /*根据条件提交查询*/
            Submitquery(Condition_query) {
                this.$refs["Condition_query"].validate((valid) => {
                    if (valid) {                      
                      
                       alert("好的");
                    } else {                         
                            this.$message.error('查询条件不能为空');                                 
                           }
                })

            },
            ModuleSubmit() {
                                this.$refs["Moduleform"].validate((valid, msg) => {
                    if (valid) {
                        this.ModuleformFormVisible = false;
                this.$message({
                    message: '保存成功',
                    type: 'success'
                });

                    } else {
                        this.$message.error('BI_名称不能为空');


                    }
                });
               


            },


        },
    }
</script>

<style>
    .el-header {
        background-color: #ffffff;
        color: #333;
        text-align: center;
        line-height: 60px; 
        height: auto;
        width: 100%;
        border-top-left-radius: 5px;
    }
    
    .el-aside {
        background-color: #ffffff;
        color: #333;
        height: 1200px;
        min-height: 120px;
        margin-right: 10px;
        border-radius: 5px;
    }
    
    .el-main {
        color: #333;
        line-height: 160px;
        padding: 0;
        width: 100%;
    }
    
    body>.el-container {
        margin-bottom: 40px;
    }
    
    .el-container:nth-child(5) .el-aside,
    .el-container:nth-child(6) .el-aside {
        line-height: 260px;
    }
    
    .el-container:nth-child(7) .el-aside {
        line-height: 320px;
    }
</style>