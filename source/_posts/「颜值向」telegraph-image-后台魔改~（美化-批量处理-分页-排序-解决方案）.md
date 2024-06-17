---
title: 「颜值向」Telegraph-Image 后台魔改~（美化/批量处理/分页/排序 - 解决方案）
date: 2024-06-17 19:05:00
comments: true
---
<!--StartFragment-->

> 如果你还未拥有自己的**Telegraph免费图床**，可参考我的上篇文章：
>
> [「白嫖向」Telegraph+PicGo+Typora 免费无限图床（完整图文教程）-CSDN博客](https://blog.csdn.net/weixin_73436849/article/details/139605621?spm=1001.2014.3001.5502)

最近部署了[免费图床 Telegraph-Image - Github](https://github.com/cf-pages/Telegraph-Image)，体验到了~~白嫖~~**开源**的快乐，当然想好好维护一下，给咱们的图片好好装饰一下她们的新家。

目前只修改了后台代码`admin.html`，希望[项目](https://github.com/cf-pages/Telegraph-Image)作者能够开放压缩前的的源代码，并提供自定义前台的指南。

### 原版后台

#### `admin.html`

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <!-- import CSS -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/element-ui@2.15.3/lib/theme-chalk/index.css" integrity="sha256-ghr1zmXTODLKl1HULQd6fq1MIe7m3FJiNTOCT8sddLM=" crossorigin="anonymous">
  <style>
    .el-image__inner.el-image__inner {
      width: 100%;
      height: 90px;
    }
    .el-image {
      text-align: center;
    }
    .el-button+.el-button {
      margin: 0;
    }
  </style>
</head>
<body>
  <div id="app">
    <el-container>
        <el-header>
         <div style="
         margin: auto;
         line-height: 60px;
         font-size: xx-large;
         position: relative;
     ">Dashboard
     

     <span style="
     position: absolute;
     right: 0px;
      " v-if="showLogoutButton"><el-button
                    size="mini"
                    type="info"
                    @click="handleLogout()">退出登录</el-button></span></div> 
        </el-header>
        <el-main><el-row :gutter="12">
            <el-col :span="24">
              <el-card shadow="always">
                记录总数量:
                {{ Number }} 
              </el-card>
            </el-col>
            <!--<el-col :span="8">
              <el-card shadow="hover">
                <el-tooltip class="item" effect="dark" content="白名单数量" placement="top-start">
                   
                </el-tooltip>
                白名单数量：{{ WhiteList }}
              </el-card>
            </el-col>
            <el-col :span="8">
              <el-card shadow="hover">
                <el-tooltip class="item" effect="dark" content="黑名单数量" placement="top-start">
                
              </el-tooltip>
              黑名单数量：{{ BlackList }}
              </el-card>
            </el-col>-->
          </el-row>
          <template>
            <el-table
              :data="tableData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
              style="width: 100%">
              <el-table-column
                label="name"
                prop="name">
              </el-table-column>
              <el-table-column
                label="preview"
                prop="preview"
                align="center">
                <template slot-scope="scope">
                  <video v-if="scope.row.name.indexOf('.mp4')>0" style="width: 100%; height: 180px;" controls>
                    <source :src="'/file/'+scope.row.name" type="video/mp4">
                  </video>
                  <el-image
                    v-else
                    style="width: 100%; height: 100%;"
                    :src="'/file/'+scope.row.name"
                    :zoom-rate="1.2"
                    :preview-src-list="['/file/'+scope.row.name]"
                    fit="cover"
                    lazy
                  />

                </template>
              </el-table-column>
              <el-table-column
                label="data"
                prop="data">
                <template slot-scope="scope">
                <el-popover trigger="hover" placement="top">
                  <p>{{ scope.row.metadata }}</p>
                  <div slot="reference" class="name-wrapper">
                    <el-tag size="medium">{{ scope.row.metadata }}</el-tag>
                  </div>
                </el-popover>
              </template>
              </el-table-column>
              <el-table-column
              align="right">
                <template slot="header" slot-scope="scope">
                  <el-input
                    v-model="search"
                    size="mini"
                    placeholder="输入关键字搜索"/>
                </template>
                <template slot-scope="scope">
                  <el-button
                    size="mini"
                    type="primary"
                    @click="handleCopy(scope.$index,scope.row.name)">复制地址</el-button>
                  <el-button
                    size="mini"
                    type="primary"
                    @click="handleWhite(scope.$index,scope.row.name)">白名单</el-button>
                  <el-button
                    size="mini"
                    type="info"
                    @click="handleBlock(scope.$index,scope.row.name)">黑名单</el-button>
                    <el-button
                    size="mini"
                    type="danger"
                    @click="handleDelete(scope.$index,scope.row.name)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
          </template>
        </el-main>
    </el-container>
  </div>
</body>
  <!-- import Vue before Element -->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.min.js" integrity="sha256-kXTEJcRFN330VirZFl6gj9+UM6gIKW195fYZeR3xDhc=" crossorigin="anonymous"></script>
  <!-- import JavaScript -->
  <script src="https://cdn.jsdelivr.net/npm/element-ui@2.15.3/lib/index.js" integrity="sha256-OFVFYfqhQ9nDnKh+NfIsefpy/fnjTwkK909ZYgo45nw=" crossorigin="anonymous"></script>
  <script>
    var app=new Vue({
      el: '#app',
      data: {
        Number:0,
        WhiteList:0,
        BlackList:0,
        showLogoutButton:false,
        tableData: [],
        dialogFormVisible: false,
        formLabelWidth: '120px',
        form: {
          name: '',
          id: ''
        },
        search: '',
        password:'123456'
      },
      methods: {
      handleBlock(index,key) {
        console.log(key);
        if (confirm("确认加入黑名单吗?")) {
        console.log("Yes")
        var requestOptions = {
          method: 'GET',
          redirect: 'follow',
          //include authorization credientials
          credentials: 'include'
        };

        fetch("./api/manage/block/"+key, requestOptions)
          .then(response => response.text())
          .then(result => {console.log(result);
            this.tableData[index].metadata=result;})
          .catch(error =>  {alert("An error occurred while synchronizing data with the server, please check the network connection");console.log('error', error)});
        
        } else {
        console.log("No")
      }
    },
    handleDelete(index,key) {
        console.log(key);
        if (confirm("确认删除该条记录吗?")) {
        console.log("Yes")
        var requestOptions = {
          method: 'GET',
          redirect: 'follow',
          //include authorization credientials
          credentials: 'include'
        };

        fetch("./api/manage/delete/"+key, requestOptions)
          .then(response => response.text())
          .then(result => {console.log(result);this.tableData.remove(index);})
          .catch(error =>  {alert("An error occurred while synchronizing data with the server, please check the network connection");console.log('error', error)});
        
        } else {
        console.log("No")
      }
    },
    handleWhite(index,key) {
      console.log(key);
      if (confirm("确认加入白名单吗?")) {
      console.log("Yes")
      var requestOptions = {
        method: 'GET',
        redirect: 'follow',
        //include authorization credientials
        credentials: 'include'
      };
      fetch("./api/manage/white/"+key, requestOptions)
          .then(response => response.text())
          .then(result => {console.log(result);this.tableData[index].metadata=result;})
          .catch(error =>  {alert("An error occurred while synchronizing data with the server, please check the network connection");console.log('error', error)});
        
        } else {
        console.log("No")
      }

      },
      handleLogout(){
        window.location.href="./api/manage/logout";
      },
      handleCopy(index, key) {
        const text = `${document.location.origin}/file/${key}`;
        if (navigator.clipboard) {
            // clipboard api 复制
            navigator.clipboard.writeText(text);
        } else {
            const textarea = document.createElement('textarea');
            document.body.appendChild(textarea);
            // 隐藏此输入框
            textarea.style.position = 'fixed';
            textarea.style.clip = 'rect(0 0 0 0)';
            textarea.style.top = '10px';
            // 赋值
            textarea.value = text;
            // 选中
            textarea.select();
            // 复制
            document.execCommand('copy', true);
            // 移除输入框
            document.body.removeChild(textarea);
        }
        this.$message({
            message: '复制文件链接成功~',
            type: 'success'
        });
      },
    },
      
    mounted () {
      //check if the user is logged in
      //read the basic auth credientials from the browser
      var requestOptions = {
        method: 'GET',
        redirect: 'follow',
        //include authorization credientials
        credentials: 'include'
      };
      fetch("./api/manage/check", requestOptions)
        .then(response => response.text())
        .then(result => {console.log(result);
          if(result=="true"){
            this.showLogoutButton=true;
          }else if(result=="Not using basic auth."){
            
          }
          else{
            window.location.href="./api/manage/login";
          }
        })
        .catch(error =>  {alert("An error occurred while synchronizing data with the server, please check the network connection");console.log('error', error)});



      Array.prototype.remove = function(from, to) {
        var rest = this.slice((to || from) + 1 || this.length);
        this.length = from < 0 ? this.length + from : from;
        return this.push.apply(this, rest);
      };
      var requestOptions = {
  method: 'GET',
  redirect: 'follow',
  //include authorization credientials
  credentials: 'include'
  
};


fetch("./api/manage/list", requestOptions)
  //判断是否需要登录
  .then(response => {
    if(response.status==401){
      alert("请先登录");
      window.location.href="./api/manage/login";
    }
    else{
      return response;
    }
  })
  .then(response => response.text()) 
  .then(result => {this.tableData=JSON.parse(result);console.log(result);this.Number=this.tableData.length})
  .catch(error => {alert("An error occurred while synchronizing data with the server, please check the network connection");console.log('error', error)});

    }
    })
    
  </script><!-- Hotjar Tracking Code -->
<script>
    (function(h,o,t,j,a,r){
        h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
        h._hjSettings={hjid:2531461,hjsv:6};
        a=o.getElementsByTagName('head')[0];
        r=o.createElement('script');r.async=1;
        r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
        a.appendChild(r);
    })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');
</script>
<script type="text/javascript">
  (function(c,l,a,r,i,t,y){
      c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
      t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
      y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
  })(window, document, "clarity", "script", "7t5ai7agat");
</script>
</html>
```

#### 官方示例

![img](https://imgtc-3i1.pages.dev/file/945dd0dabc1d2793d82ed.png)

### 魔改后台

#### `admin.html`

```
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>ImgTC | Admin</title>
  <!-- Import CSS -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/element-ui@2.15.3/lib/theme-chalk/index.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free/css/all.min.css">
  <style>
    body {
      background: linear-gradient(90deg, #ffd7e4 0%, #c8f1ff 100%);
      font-family: 'Arial', sans-serif;
      color: #333;
      margin: 0;
      padding: 0;
    }

    .header-content {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 20px;
      background-color: rgba(255, 255, 255, 0.75);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      transition: background-color 0.5s ease, box-shadow 0.5s ease;
      border-bottom-left-radius: 10px;
      border-bottom-right-radius: 10px;
    }

    .header-content:hover {
      background-color: rgba(255, 255, 255, 0.85);
      box-shadow: 0 6px 10px rgba(0, 0, 0, 0.2);
    }

    .title {
      font-size: 1.8em;
      font-weight: bold;
      cursor: pointer;
      transition: color 0.3s ease;
      color: #333;
    }

    .title:hover {
      color: #B39DDB; /* 使用柔和的淡紫色 */
    }

    .search-card {
      margin-left: auto;
      margin-right: 20px;
    }

    .stats {
      font-size: 1.2em;
      margin-right: 20px;
      display: flex;
      align-items: center;
      background: rgba(255, 255, 255, 0.9);
      padding: 5px 10px;
      border-radius: 10px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      transition: background-color 0.3s ease, box-shadow 0.3s ease;
      color: #333;
    }

    .stats .fa-database {
      margin-right: 10px;
      font-size: 1.5em;
      transition: color 0.3s ease;
      color: inherit;
    }

    .stats:hover {
      background-color: #f0eaf8; /* 使用柔和的淡紫色 */
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.15);
      color: #B39DDB; /* 使用柔和的淡紫色 */
    }

    .stats:hover .fa-database {
      color: #B39DDB; /* 使用柔和的淡紫色 */
    }

    .header-content .actions {
      display: flex;
      align-items: center;
      gap: 15px;
    }

    .header-content .actions i {
      font-size: 1.5em;
      cursor: pointer;
      transition: color 0.3s, transform 0.3s;
      color: #333;
    }

    .header-content .actions i:hover {
      color: #B39DDB; /* 使用柔和的淡紫色 */
      transform: scale(1.2);
    }

    .header-content .actions .el-dropdown-link i {
      color: #333;
    }

    .header-content .actions .el-dropdown-link i:hover {
      color: #B39DDB; /* 使用柔和的淡紫色 */
    }

    .header-content .actions .disabled {
      color: #bbb;
      pointer-events: none;
    }

    .header-content .actions .enabled {
      color: #B39DDB; /* 使用柔和的淡紫色 */
    }

    .search-card .el-input__inner {
      border-radius: 20px;
      width: 300px;
      height: 40px;
      font-size: 1.2em;
      border: none;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
      transition: width 0.3s;
    }

    .search-card .el-input__inner:focus {
      width: 400px;
    }

    .main-container {
      display: flex;
      flex-direction: column;
      padding: 20px;
      min-height: calc(100vh - 80px);
    }

    .content {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      grid-template-rows: repeat(3, 1fr);
      gap: 20px;
      padding: 10px;
      flex-grow: 1;
    }

    .el-card {
      width: 100%;
      background: rgba(255, 255, 255, 0.6);
      border-radius: 8px;
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      position: relative;
      transition: transform 0.3s ease;
    }

    .el-card:hover {
      transform: scale(1.05);
    }

    .el-image {
      width: 100%;
      height: 200px;
      object-fit: cover;
      transition: opacity 0.3s ease;
    }

    .el-image:hover {
      opacity: 0.8;
    }

    .file-info {
      padding: 10px;
      background: rgba(0, 0, 0, 0.6);
      color: white;
      text-align: center;
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      box-sizing: border-box;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .image-overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(0, 0, 0, 0.6);
      opacity: 0;
      transition: opacity 0.3s ease;
      pointer-events: none;
    }

    .el-card:hover .image-overlay {
      opacity: 1;
    }

    .overlay-buttons {
      display: flex;
      gap: 10px;
      pointer-events: auto;
    }

    .pagination-container {
      display: flex;
      justify-content: center;
      margin-top: 20px;
      padding-bottom: 20px;
    }

    .el-checkbox {
      position: absolute;
      top: 10px;
      right: 10px;
      transform: scale(1.5);
      z-index: 10;
    }
  </style>
</head>
<body>
  <div id="app">
    <el-container>
      <el-header>
        <div class="header-content">
          <span class="title" @click="refreshDashboard">Dashboard</span>
          <div class="search-card">
            <el-input v-model="search" size="mini" placeholder="输入关键字搜索"></el-input>
          </div>
          <span class="stats">
            <i class="fas fa-database"></i> 记录总数量: {{ Number }}
          </span>
          <div class="actions">
            <el-tooltip content="排序" placement="bottom">
              <el-dropdown @command="sort" :hide-on-click="false">
                <span class="el-dropdown-link">
                  <i :class="sortIcon"></i>
                </span>
                <el-dropdown-menu slot="dropdown">
                  <el-dropdown-item command="dateDesc" :class="{ 'el-dropdown-menu__item--selected': sortOption === 'dateDesc' }">按时间倒序</el-dropdown-item>
                  <el-dropdown-item command="nameAsc" :class="{ 'el-dropdown-menu__item--selected': sortOption === 'nameAsc' }">按名称升序</el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>
            </el-tooltip>
            <el-tooltip content="批量复制" placement="bottom">
              <i class="fas fa-link" :class="{ disabled: selectedFiles.length === 0 }" @click="handleBatchCopy"></i>
            </el-tooltip>
            <el-tooltip content="批量删除" placement="bottom">
              <i class="fas fa-trash-alt" :class="{ disabled: selectedFiles.length === 0 }" @click="handleBatchDelete"></i>
            </el-tooltip>
            <el-tooltip content="退出登录" placement="bottom">
              <i class="fas fa-home" @click="handleLogout"></i>
            </el-tooltip>
          </div>
        </div>
      </el-header>
      <el-main class="main-container">
        <div class="content">
          <template v-for="(item, index) in paginatedTableData" :key="index">
            <el-card>
              <el-checkbox v-model="item.selected"></el-checkbox>
              <el-image
                :src="'/file/' + item.name"
                :preview-src-list="['/file/' + item.name]"
                fit="cover"
                lazy></el-image>
              <div class="image-overlay">
                <div class="overlay-buttons">
                  <el-button size="mini" type="primary" @click.stop="handleCopy(index, item.name)">复制地址</el-button>
                  <el-button size="mini" type="danger" @click.stop="handleDelete(index, item.name)">删除</el-button>
                </div>
              </div>
              <div class="file-info">{{ item.name }}</div>
            </el-card>
          </template>
        </div>
        <div class="pagination-container">
          <el-pagination
            background
            layout="prev, pager, next"
            :total="filteredTableData.length"
            :page-size="pageSize"
            @current-change="handlePageChange"
            :current-page.sync="currentPage">
          </el-pagination>
        </div>
      </el-main>
    </el-container>
  </div>

  <!-- Import Vue before Element -->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.min.js"></script>
  <!-- Import JavaScript -->
  <script src="https://cdn.jsdelivr.net/npm/element-ui@2.15.3/lib/index.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        Number: 0,
        showLogoutButton: false,
        tableData: [],
        search: '',
        currentPage: 1,
        pageSize: 15,
        selectedFiles: [],
        sortOption: 'dateDesc',
        isUploading: false
      },
      computed: {
        filteredTableData() {
          return this.tableData.filter(data => !this.search || data.name.toLowerCase().includes(this.search.toLowerCase()));
        },
        paginatedTableData() {
          const sortedData = this.sortData(this.filteredTableData);
          const start = (this.currentPage - 1) * this.pageSize;
          const end = start + this.pageSize;
          return sortedData.slice(start, end);
        },
        sortIcon() {
          return this.sortOption === 'dateDesc' ? 'fas fa-sort-amount-down' : 'fas fa-sort-alpha-up';
        }
      },
      watch: {
        tableData: {
          handler(newData) {
            this.selectedFiles = newData.filter(file => file.selected);
          },
          deep: true
        },
        sortOption(newOption) {
          localStorage.setItem('sortOption', newOption);
        }
      },
      methods: {
        refreshDashboard() {
          location.reload();
        },
        handleDelete(index, key) {
          this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            fetch(`./api/manage/delete/${key}`, { method: 'GET', credentials: 'include' })
              .then(response => response.ok ? this.tableData.splice(index, 1) : Promise.reject())
              .then(() => {
                this.updateStats();
                this.$message.success('删除成功!');
              })
              .catch(() => this.$message.error('删除失败，请检查网络连接'));
          }).catch(() => this.$message.info('已取消删除'));
        },
        handleBatchDelete() {
          this.$confirm('此操作将永久删除选中的文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            const promises = this.selectedFiles.map(file => fetch(`./api/manage/delete/${file.name}`, { method: 'GET', credentials: 'include' }));

            Promise.all(promises)
              .then(results => {
                results.forEach((response, index) => {
                  if (response.ok) {
                    const fileIndex = this.tableData.findIndex(file => file.name === this.selectedFiles[index].name);
                    if (fileIndex !== -1) {
                      this.tableData.splice(fileIndex, 1);
                    }
                  }
                });
                this.selectedFiles = [];
                this.updateStats();
                this.$message.success('批量删除成功!');
              })
              .catch(() => this.$message.error('批量删除失败，请检查网络连接'));
          }).catch(() => this.$message.info('已取消批量删除'));
        },
        handleBatchCopy() {
          const links = this.selectedFiles.map(file => `${document.location.origin}/file/${file.name}`).join('\n');
          navigator.clipboard ? navigator.clipboard.writeText(links).then(() => this.$message.success('批量复制链接成功~')) :
            this.copyToClipboardFallback(links);
        },
        copyToClipboardFallback(text) {
          const textarea = document.createElement('textarea');
          document.body.appendChild(textarea);
          textarea.style.position = 'fixed';
          textarea.style.clip = 'rect(0 0 0 0)';
          textarea.style.top = '10px';
          textarea.value = text;
          textarea.select();
          document.execCommand('copy');
          document.body.removeChild(textarea);
          this.$message.success('批量复制链接成功~');
        },
        handleLogout() {
          window.location.href = '/';
        },
        handleCopy(index, key) {
          const text = `${document.location.origin}/file/${key}`;
          navigator.clipboard ? navigator.clipboard.writeText(text).then(() => this.$message.success('复制文件链接成功~')) :
            this.copyToClipboardFallback(text);
        },
        handlePageChange(page) {
          this.currentPage = page;
        },
        updateStats() {
          this.Number = this.tableData.length;
        },
        sort(command) {
          this.sortOption = command;
        },
        sortData(data) {
          return this.sortOption === 'nameAsc' ? data.sort((a, b) => a.name.localeCompare(b.name)) :
            data.sort((a, b) => b.metadata.TimeStamp - a.metadata.TimeStamp);
        }
      },
      mounted() {
        fetch("./api/manage/check", { method: 'GET', credentials: 'include' })
          .then(response => response.text())
          .then(result => result === "true" ? this.showLogoutButton = true : window.location.href = "./api/manage/login")
          .catch(() => this.$message.error('同步数据时出错，请检查网络连接'));

        fetch("./api/manage/list", { method: 'GET', credentials: 'include' })
          .then(response => response.json())
          .then(result => {
            this.tableData = result.map(file => ({ ...file, selected: false }));
            this.updateStats();
            const savedSortOption = localStorage.getItem('sortOption');
            if (savedSortOption) {
              this.sortOption = savedSortOption;
            }
            this.sortData(this.tableData);
          })
          .catch(() => this.$message.error('同步数据时出错，请检查网络连接'));
      }
    });
  </script>
</body>
</html>
```

#### 效果展示

![image-20240614191231653](https://imgtc-3i1.pages.dev/file/ace383c828ec4e7120a74.png)

### 主要修改

* 优化界面，将照片做成卡片，悬停可复制链接/删除
* 删除了黑/白名单功能
* 增加了批量删除/复制链接
* 增加了按时间倒序排序
* 增加了分页功能，减少卡顿

### 说明

* 可将我的代码直接复制到`admin.html`并提交到你的Github仓库，Cloudfare会自动部署
* 点击图片可触发preview功能
* 默认按上传时间排序
* **批量上传**功能暂时无法实现，推荐使用**[PicGo](https://github.com/Molunerfinn/PicGo)**（这也解决了上传后需要手动访问一次才能在后台显示的Bug，另外Markdown图片链接也可以在PicGo上复制）
* **重命名**功能暂时无法实现（原项目使用KV存储，文件名不可修改且应该也没有对应api）

### 碎碎念

原项目已经很久没维护了，或许我可以试着再开发？欢迎大家分享有没有什么建议...



<!--EndFragment-->