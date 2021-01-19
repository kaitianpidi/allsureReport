# allure报告，在“查看页面的整合”
## 1.implementCase.ftl 添加页面
```
    <div iv class="panel-body" style="padding-bottom:0px;" id="panel-body5">
        <button id="returnPreviousPage5" type="button" class="btn btn-primary" onclick="returnPreviousPage5()">
            <span class="glyphicon glyphicon-step-backward" aria-hidden="true">返回</span>
        </button>
        <iframe id="allureIframe" src="" frameborder="0" width="100%" height="80%" >
        </iframe>
    </div>
```

## 2. implement.js  中的operateEventsTow方法 
为了方便调试，这里原来的代码，放在了else中， if{}中的代码是要执行的allure报告。

```
//点击执行查看结果页面，需要跳转
window.operateEventsTow = {
    'click .btn-primary': function (e, value, row, index) {
        // if(true){ //allure报告
        if(jobName=="testallure"){ //已有的报告名
            // var url ="/allure2/jobName1/allureReport";
            // window.open(url, "_self");

            $("#panel-body1").hide();
            $("#panel-body2").hide();
            $("#panel-body3").hide();
            $("#panel-body4").hide();


            // $("#dataTable2").bootstrapTable('destroy');
            $("#returnPreviousPage4").hide();
            // $("#returnPreviousPage3").show();
            // $("#resultShow").show();
            // $("#panel-body4").show();

            returPage5=1;
            $("#allureIframe").attr("src","/allure2/"+row.jobName+"/allureIndex#suites");
            $("#panel-body5").show();
        }else {  //以前代码
            ............
            // var jobName=row
            
          }
        }
    }
```

## 3.其他页面，添加：隐藏 panel-body5 页面
### 3.1 implementCase.ftl  
```
  function dataTable() {
   $("#panel-body5").hide();
  }
}
```
## 4. 增加 returnPreviousPage5()方法
 如： 在  implement.js 
 这里使用 returPage5变量，来区分，“查看结果”是来自“首页”还是“查看历史记录”。
 ```
//该地址的 首页中的“查看结果"的返回页
var returPage5;
function returnPreviousPage5() {

    $("#panel-body5").hide();  //在function returnPreviousPage() 基础上，增加隐藏 panel-body5
    if(returPage5 ==1){
        $("#toolbar").show();
        $("#panel-body1").show();
    }
    if(returPage5==2){
        $("#toolbar2").show();
        $("#panel-body2").show();
    }
}
 ```
 ## 5. ”查看历史记录“ 的事件修改
 implementCase.ftl
 if{}中 为allure报告的内容
 ```
 window.operateEventsSix={
    'click .btn-primary': function (e, value, row, index){
        if(row.jobName=="testallure"){
            $("#panel-body2").hide();
            // $("#dataTable2").bootstrapTable('destroy');
            $("#returnPreviousPage3").hide();
            // $("#returnPreviousPage4").show();
            // $("#resultShow").show();
            // $("#panel-body4").show();

            returPage5=2;
            $("#allureIframe").attr("src","/allure2/"+row.jobName+"/allureIndex#suites");
            $("#panel-body5").show();
        }else{//原来的代码}

 ```
