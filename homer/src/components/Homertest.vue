<template>
  

            <div id="Homer" style="margin-top: 10px;">
                <el-container>
                    <el-aside style="height:120px;width: 260px;padding-top: 20px;padding-left: 5px;">
                      
                    </el-aside>
                    <el-container>
                        <el-header style="height:auto;text-align: left;padding-top: 20px;">
                   
                             
                                        <el-form :model="Condition_query"  :inline="true" :show-message="false"  ref="Condition_query" :validate-on-rule-change="false">  


                                       <!--index 索引  为select取消联动   v-show="'form.'+`${item.required}`=== true"-->
                                       <el-form-item   v-for="(item,index) in Condition_querydata" style="margin-left: 15px;"   :rules= "[{type: 'array', required: true, trigger: 'blur',}]"  :prop="`Conditionoption[${index}]`"  :key="index">
                                      <el-select   v-model="Condition_query.Conditionoption[index]"   :placeholder="`${item.placeholder}`"   :value-key="`Conditionoption[${index}]`"  multiple  collapse-tags >
                                        <el-option v-for="item in item.Condition_option" :key="item.value" :label="item.label" :value="item.value">   
                                                                       
                                        </el-option>
                                    </el-select>   
                                                                     
                                      </el-form-item>  
                                       <el-form-item>   
                                        <el-button type="primary" icon="el-icon-search" style="margin-left: 15px;" @click="Submitquery(Condition_query)">查询</el-button>   
                                       </el-form-item> 


                                    </el-form>                                  
                      
                      
                        </el-header>
                        <el-main>
                          <iframe src="" frameborder="0" scrolling="auto" style="width: 100%; height: 1077px;"></iframe>
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
              
                Condition_querydata:[                                     
                    {
                        storeName:0,
                        placeholder:'货主名称',  
                        required:'consignor',                  
                        Condition_option: [
                            {
                            value: 0,
                            label: '测试货主'
                            }, 
                            {
                            value: 1,
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
                            value: 0,
                            label: '测试单位'
                            }, 
                            {
                            value: 1,
                            label: '测试单位1'
                            }
                            ],
                    },
                    {
                        id:2,
                        placeholder:'操作人名',  
                        required:'Operator',                  
                        Condition_option: [
                            {
                            value: 0,
                            label: '测试人名'
                            }, 
                            {
                            value:1,
                            label: '测试人名1'
                            }
                            ],
                    },
                    {
                        id:3,
                        placeholder:'商品名称',  
                        required:'product',                  
                        Condition_option: [
                            {
                            value: 0,
                            label: '测试商品'
                            }, 
                            {
                            value: 1,
                            label: '测试商品1'
                            }
                            ],
                    },
                    
                    ],
               


           
                
                /**表单Condition_query模型 */
                Condition_query:{                    
                    Conditionoption:[], 
 
                    },
        
            };
        },
        methods: {
           created() {
   this.multiple ? (this.Conditionoption = []) : (this.Conditionoption = "");
  },
            
           Submitquery(Condition_query) {
                this.$refs["Condition_query"].validate((valid) => {
                    if (valid) {                      
                      
                       alert("好的");
                    } else {                         
                            this.$message.error('查询条件不能为空');                                 
                           }
                })

            },  

getChange(ss,yy){console.log(ss,yy)},
   



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