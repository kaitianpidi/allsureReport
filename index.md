
![page页面](https://github.com/kaitianpidi/allsureReport/blob/gh-pages/allure_page.png?raw=true)
![资源目录](https://github.com/kaitianpidi/allsureReport/blob/gh-pages/allure_page.png?raw=true)
图片，如不能显示，可直接看下面的文章。

springboot项目整合allure 报告


#一. springboot项目引入资源文件

##1.在static下，建立目录 allure2 
 从allure-report中拷贝  data，widgets  两个目录到 static/allure2
 
##2.在lib 下，建立allure2  （也可以把使用上面的allure2 ， 这里是分开 存放资源）

 从allure-report中拷贝  plugins目录，app.js，favicon.ico，styles.css  四个内容 到 static/lib/allure2
 
#二. 创建页面：  templates/allure2/allureReport.ftl 

把allure-report中的页面，如：index.html，改成allureReport.ftl  
修改资源的连接地址，如下：

<#include "/layout/layout.ftl">

<@body>

    \<link rel="favicon" href="/lib/allure2/favicon.ico?v=2"\>
    
    \<link rel="stylesheet" href="/lib/allure2/styles.css"\>
    
     \<link rel="stylesheet" href="/lib/allure2/plugins/screen-diff/styles.css"\>
     
     .............................
     
     <div id="popup"></div>
<script src="/lib/allure2/app.js"></script>
    <script src="/lib/allure2/plugins/behaviors/index.js"></script>
    <script src="/lib/allure2/plugins/packages/index.js"></script>
    <script src="/lib/allure2/plugins/screen-diff/index.js"></script>

</@body>

#三 . 建立controller， 
@Controller
@RequestMapping("allure2")
public class allureReportController {
    @RequestMapping("allureReport")
    public String abnormalStatisticsPage(){
        return "allure2/allureReport";
    }
}
注意：  @RequestMapping("allure2") 中的 "allure2" 要和 static/allure2 的名称一致。否则 资源会找不到。

#四.  页面 调用 报告
在任意页面，添加超链接 （如：implementCase/implementCase.ftl页）

建立a 超链接标签， 连接地址： "/allure2/allureReport#suites"

\<button id="btn_add" type="button" \>
           \<a  href="/allure2/allureReport#suites" target="_blank">演示Allure报告\</a\>
\<\/button\>
