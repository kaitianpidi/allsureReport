```
import net.sf.json.JSONArray;
import net.sf.json.JSONObject;

import java.io.*;
import java.util.HashMap;
import java.util.Map;

public class AllureTest {



        public static void main(String[] args) {
        System.out.println(getJson());
        JSONObject json = JSONObject.fromObject(getJson());
//       System.out.println(json.getJSONObject("data").getJSONArray("result"));
        System.out.println(json.get("uid"));
        JSONArray children = json.getJSONArray("children");
        System.out.println(children);

        JSONObject object = children.getJSONObject(0);
        JSONArray children1 = object.getJSONArray("children");
        JSONObject object1 = children1.getJSONObject(0);
        JSONArray children2 = object1.getJSONArray("children");
        JSONObject object2 = children2.getJSONObject(0);
        JSONArray children3 = object2.getJSONArray("children");
            Map<String, JSONObject> map = new HashMap<>();
            Map<String, JSONObject> resultMap = new HashMap<>();
            for (int i = 0; i <children3.size() ; i++) {
                JSONObject o = children3.getJSONObject(i);
                if(!(o.get("status").equals("passed"))){
                    map.put((String) o.get("parentUid"), o);
                }
            }

            for (Map.Entry<String, JSONObject> entry : map.entrySet()) {
                for (int i = 0; i <children3.size()  ; i++) {
                    JSONObject o = children3.getJSONObject(i);
                    if(entry.getKey().equals(o.get("parentUid"))){
                        if((entry.getValue().get("status")).equals(o.get("status"))){
                            if("测试用例的具体内容不同"){
                                resultMap.put(entry.getKey(), entry.getValue());
                            }
                        }else {
                            resultMap.put(entry.getKey(), entry.getValue());
                        }
                    }

                }
            }





        }
    //获取json
    public static String getJson() {
        String jsonStr = "";
        try {
            File file = new File("D:\\testResultDownLoadPath\\testtest975\\data\\suites.json");
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
