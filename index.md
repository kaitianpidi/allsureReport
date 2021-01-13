1. 创建页面：  templates/allure2/allureReport.ftl 
把allure-report中的index.html，改成allureReport.ftl 
<#include "/layout/layout.ftl">
<@body>
    <#--<meta charset="utf-8">-->
    <#--<title>Allure Report</title>-->
    <link rel="favicon" href="/lib/allure2/favicon.ico?v=2">
    <link rel="stylesheet" href="/lib/allure2/styles.css">
     <link rel="stylesheet" href="/lib/allure2/plugins/screen-diff/styles.css">
     
     .............................
     
     <div id="popup"></div>
<script src="/lib/allure2/app.js"></script>
    <script src="/lib/allure2/plugins/behaviors/index.js"></script>
    <script src="/lib/allure2/plugins/packages/index.js"></script>
    <script src="/lib/allure2/plugins/screen-diff/index.js"></script>

</@body>
