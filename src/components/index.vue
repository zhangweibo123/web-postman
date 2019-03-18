<template>
  <div class="index">
    <div class="content">
    <h1>促进两岸和谐,推动祖国统一</h1>
      <el-row>
        <el-col :span="3">
          <el-select v-model="method" placeholder="请选择请求方式">
            <el-option label="GET" value="get"></el-option>
            <el-option label="POST" value="post"></el-option>
            <el-option label="DELETE" value="delete"></el-option>
            <el-option label="PUT" value="put"></el-option>
            <el-option label="PATCH" value="patch"></el-option>
            <el-option label="HEAD" value="head"></el-option>
          </el-select>
        </el-col>
        <el-col :span="3">
          <el-select v-model="http" placeholder="HTTP">
            <el-option label="HTTP" value="http://"></el-option>
            <el-option label="HTTPS" value="https://"></el-option>
          </el-select>
        </el-col> 
        <el-col :span="9">
          <el-input v-model="url" placeholder="请输入url"></el-input>
        </el-col>         
        <el-col :span="9">
          <el-input placeholder="请输入API" v-model="main" class="input-with-select">
            <el-button type="primary" slot="append" icon="el-icon-search" @click="send">发射</el-button>
          </el-input>
        </el-col>
      </el-row>

      <el-row style="margin-top:30px;">
          <el-col :span="3">
            <p>请求头参数</p>
            <el-button type="primary"  @click="addGet">新增参数</el-button>
          </el-col>
          <el-col :span="21">
              <table id="getTable">
                <tr>
                  <td>KEY</td>
                  <td>VALUE</td>
                  <td>操作</td>
                </tr>
                <tr v-for="(item,index) in gettable">
                    <td>
                      <el-input v-model="item.key" placeholder="请输入key"></el-input>
                    </td>
                    <td>
                      <el-input v-model="item.value" placeholder="请输入value"></el-input>
                    </td>
                    <td>
                      <a @click="delGet(index)">删除</a>
                    </td>
                </tr>
              </table>
          </el-col>
      </el-row>

      <el-row style="margin-top:30px;">
          <el-col :span="3">
            <p>请求体参数</p>
            <el-button type="primary"  @click="addPost">新增参数</el-button>
          </el-col>
          <el-col :span="21">
              <table id="posttable">
                <tr>
                  <td>KEY</td>
                  <td>VALUE</td>
                  <td>操作</td>
                </tr>
                <tr v-for="(item,index) in posttable">
                    <td>
                      <el-input v-model="item.key" placeholder="请输入key"></el-input>
                    </td>
                    <td>
                      <el-input v-model="item.value" placeholder="请输入value"></el-input>
                    </td>
                    <td>
                      <a @click="delPost(index)">删除</a>                      
                    </td>
                </tr>
              </table>
          </el-col>
      </el-row>
      <el-row style="margin-top:30px">
        <el-col :span="12">
          <el-input v-model="cookie1" placeholder="cookie key"></el-input>
        </el-col>
        <el-col :span="12">
          <el-input  v-model="cookie2"  placeholder="cookie value"></el-input>
        </el-col>
      </el-row>
       <el-row style="margin-top:20px">
        <el-col :span="12">
          <el-input  v-model="cookie3" placeholder="cookie key"></el-input>
        </el-col>
        <el-col :span="12">
          <el-input  v-model="cookie4" placeholder="cookie value"></el-input>
        </el-col>
      </el-row>
      <el-row style="margin-top:20px" >
        <el-col :span="24" style="text-align:center">
            <el-button type="primary" @click="setCookie">设置cookie</el-button>
            <el-button @click="delCookie">清除cookie</el-button>
        </el-col> 
          
      </el-row> 
      <!-- 数据展示区 -->
      <h2>数据展示区</h2>
      <div style="width:97%;padding:20px;border:1px solid #dddddd;height:200px;overflow:auto;">
      {{responseList}}

      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
      http:'http://',
      // 输入接口名
      main: "misc/city",
      // 调用方法
      method: "get",
      // url
      url:"api.purchase.twow-corp.com/api/v1.0/",
      // gettable数据
      gettable: [{
            key: 'client_id',
            value: 'k3iJKrIVROqKuspP6_STXA',
          }],
      // posttable数据
      posttable: [{
          key: 'cccc',
          value: '123',
        }],
      responseList:'',
      // 设置cookie
      cookie1:'token',
      cookie2:'Crun36ptTIG2tZ9jKc_pvw',
      cookie3:'installation_id',
      cookie4:'jt7xirleQQ24T-p7A63Gug',
    };
  },
  methods: {
    send: function() {
      let getParameter = '?';
      let postParameter = new Object();
      this.gettable.forEach(function(item,index){
        getParameter = getParameter + item.key + "=" + item.value + "&"
      })
      this.posttable.forEach(function(item,index){
          postParameter[item.key] = item.value
      })

      if(this.http =='' || this.main  =='' || this.method  =='' || this.url  ==''){
        this.$message({
          message: '请输入必填信息',
          type: 'warning'
        });
      }else{
          this.$axios({
              method: this.method,
              url: `${this.http}${this.url}${this.main}${getParameter}`,
              data: { 
                ...postParameter
              }
            })
            .then(function (response) {
              if(response.status == 200){
                this.$message({
                  message: '发射成功',
                  type: 'success'
                });
                this.responseList = response.data.data
              }else{
                  this.$message({
                    message: '发射失败',
                    type: 'warning'
                  });
              }
            }.bind(this))
            .catch(function (error) {
              console.log(error);
            });
        }     
      
    },
    setCookie: function() {
      document.cookie= `${this.cookie1}=${this.cookie2}`;
      document.cookie= `${this.cookie3}=${this.cookie4}`;
    },
    delCookie: function() {
      setCookie(token, ' ', -1);
      setCookie(installation_id, ' ', -1);
    },
    addGet: function() {
      this.gettable.push({
        key:'',
        value:''
      })
    },

    addPost: function() {
      this.posttable.push({
        key:'',
        value:''
      })
    },
    delGet: function(i) {
      this.gettable.splice(i,1)
    },
    delPost: function(i) {
      this.posttable.splice(i,1)
    },

  }
};
</script>

<style scoped>
.content {
  width: 80%;
  margin: auto;
  margin-top: 8vh;
}
.el-col {
  padding: 0px 10px;
}
#posttable,#getTable{width: 100%;border-collapse: collapse}
#posttable td, #getTable td{border: 1px solid #dddddd;text-align: center;padding: 10px 30px}
</style>
