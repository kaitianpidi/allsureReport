# 项目中的 文件放到其他地方， 在项目访问方式不变的情况下，也可以正常访问，实现原理：
1.前端js，查看项目中文件的访问地址。    
2. 后台，写一个服务， 提供相同的访问地址。  数据内容由该服务提供（即，读取文件，返回给浏览器）

#功能实现：
##参考代码（https://www.cnblogs.com/javajetty/p/9648551.html 最后一段 “返回指定地址的文件流”）
##1.代码：
```
package com.compass;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import sun.rmi.runtime.Log;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;


@Controller
@RequestMapping("allure2/{jobName}")
public class allureReportController {
    //报告地址： D:\allureReport\{jobName}\allure-report
    String allurePath="D:\\allureReport";  //该地址放到配置文件中。

    @RequestMapping("allureReport")
    public String abnormalStatisticsPage(){
        return "allure2/allureReport";
    }

//http://localhost:29412/allure2/jobName1/widgets/behaviors.json  模拟接收的 请求地址  (假设是 jobName1 的报告)
    @RequestMapping(value = "/widgets/{fileName}")
    @ResponseBody
    public void getUrlFile(@PathVariable("fileName") String fileName,@PathVariable("jobName") String jobName, HttpServletRequest request, HttpServletResponse response) {
        //因接收的地址中不带有扩展名，  文件名从request 中获取
        String requestURI = request.getRequestURI();  //allure2/jobName1/widgets/behaviors.json
//        String serverUrl = "D:\\allureReport\\jobName1\\allure-report\\widgets\\";
        String serverUrl = allurePath + "\\"+ jobName + "\\allure-report\\widgets\\";
        // 后缀名
        String suffixName = requestURI.substring(requestURI.lastIndexOf("."));
        String fileUrl = serverUrl + fileName+suffixName;
        File file = new File(fileUrl);

        //判断文件是否存在如果不存在就返回默认图标
        if (!(file.exists() && file.canRead())) {
            System.out.println("检查文件:"+ fileUrl+",是否不存在");
            return;
        }
        FileInputStream inputStream = null;
        try {
            inputStream = new FileInputStream(file);
            byte[] data = new byte[(int) file.length()];
            int length = inputStream.read(data);
            inputStream.close();
//            response.setContentType(imgType + ";charset=utf-8");
            OutputStream stream = response.getOutputStream();
            stream.write(data);
            stream.flush();
            stream.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //http://localhost:29412/allure2/jobName1/data/behaviors.json  模拟接收的 请求地址
    @RequestMapping(value = "/data/{fileName}")
    @ResponseBody
    public void getUrlDataFile(@PathVariable("fileName") String fileName,@PathVariable("jobName") String jobName, HttpServletRequest request, HttpServletResponse response) {
        //因接收的地址中不带有扩展名，  文件名从request  中获取
        String requestURI = request.getRequestURI();  //allure2/jobName1/widgets/behaviors.json

//        String serverUrl = "D:\\allureReport\\jobName1\\allure-report\\data\\";
        String serverUrl = allurePath + "\\"+ jobName + "\\allure-report\\data\\";
        // 后缀名
        String suffixName = requestURI.substring(requestURI.lastIndexOf("."));
        String fileUrl = serverUrl + fileName+suffixName;
        File file = new File(fileUrl);

        //判断文件是否存在如果不存在就返回默认图标
        if (!(file.exists() && file.canRead())) {
            System.out.println("检查文件:"+ fileUrl+",是否不存在");
            return;
        }
        FileInputStream inputStream = null;
        try {
            inputStream = new FileInputStream(file);;
            byte[] data = new byte[(int) file.length()];
            int length = inputStream.read(data);
            inputStream.close();
            OutputStream stream = response.getOutputStream();
            stream.write(data);
            stream.flush();
            stream.close();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //http://localhost:29412/allure2/jobName1/data/test-cases/a1026196e7e593dd.json  模拟接收的 请求地址
    @RequestMapping(value = "/data/test-cases/{fileName}")
//    @RequestMapping(value = "/jobName1/data/test-cases/{fileName}")
    @ResponseBody
    public void getUrlDataTestCaseFile(@PathVariable("fileName") String fileName,@PathVariable("jobName") String jobName,  HttpServletRequest request, HttpServletResponse response) {
        //因接收的地址中不带有扩展名，  文件名从request 中获取
        String requestURI = request.getRequestURI();  //allure2/jobName1/widgets/behaviors.json

//        String serverUrl = "D:\\allureReport\\jobName1\\allure-report\\data\\test-cases\\";
        String serverUrl = allurePath + "\\"+ jobName + "\\allure-report\\data\\test-cases\\";
        // 后缀名
        String suffixName = requestURI.substring(requestURI.lastIndexOf("."));
        String fileUrl = serverUrl + fileName+suffixName;
        File file = new File(fileUrl);

        //判断文件是否存在如果不存在就返回默认图标
        if (!(file.exists() && file.canRead())) {
            System.out.println("检查文件:"+ fileUrl+",是否不存在");
            return;
        }
        FileInputStream inputStream = null;
        try {
            inputStream = new FileInputStream(file);;;
            byte[] data = new byte[(int) file.length()];
            int length = inputStream.read(data);
            inputStream.close();
            OutputStream stream = response.getOutputStream();
            stream.write(data);
            stream.flush();
            stream.close();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

##2.修改访问地址：
```
  <button id="btn_add" type="button" >
            <a  href="/allure2/jobName1/allureReport#suites" target="_blank">演示Allure报告</a>
       </button>
```
