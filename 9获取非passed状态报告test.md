# 生成非passed状态的结果，数据结构和原来一致
1.**测试方法**，执行main 方法， 控制台打印 json数据， 把数据拷贝到 json在线解析，查看json数据。  
2. 把json数据，替换到 suites.json文件中。  使用 allure open  ./  启动服务测试，看是否显示正常  
```
import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import org.springframework.stereotype.Service;

import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.util.ArrayList;
import java.util.List;

public class AllureReportTest2 {

    public  static JSONObject handleNotPassed(JSONObject json) {
        if (json != null) {
            Object status = json.get("status");
            if (status != null) {
                if (!status.equals("passed")) {    //非passed 的用例
                    return json;  //返回
                }else {
                    return null;
                }
            } else { //没有status字段，查看是否有children
                if (json.get("children") != null) {
                    JSONArray children = json.getJSONArray("children");

                    //子对象中，找到非passed状态的用例，则递归处理子对象
                    ArrayList<JSONObject> tempList = new ArrayList<>();
                    for (Object child : children) {
                        JSONObject returnJson = handleNotPassed((JSONObject) child);
                        if (returnJson != null) {
                            tempList.add(returnJson);
                        }
                    }
                   json.put("children", tempList);
                }
            }
        }
        return json;
    }

    public static void main(String[] args) {
        String fileUrl ="D:\\allureReport\\testallure88888\\allure-report\\data\\suites.json";
        JSONObject json = JSONObject.fromObject(getJson(fileUrl));
        JSONObject jsonObject = handleNotPassed(json);
        System.out.println( jsonObject);
 }

    //获取json文件，返回json字符串
    public static String getJson(String fileUrl) {
        String jsonStr = "";
        try {
            File file = new File(fileUrl);
            FileReader fileReader = new FileReader(file);
            Reader reader = new InputStreamReader(new FileInputStream(file),"Utf-8");
            int ch = 0;
            StringBuffer sb = new StringBuffer();
            while ((ch = reader.read()) != -1) {
                sb.append((char) ch);
            }
            fileReader.close();
            reader.close();
            jsonStr = sb.toString();
            return jsonStr;
        } catch (Exception e) {
            return null;
        }
    }
}

```
