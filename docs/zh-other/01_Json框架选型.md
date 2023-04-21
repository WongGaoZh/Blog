## Json框架选型
### 后端转换功能
主要流行的三大类库是 FastJSON, Gson 和Jackson. 因为很少用到Gson 所以主要记录FastJSON和Jackson的功能
从性能上来说
jackson的ObjectMapper实例化是一个性能瓶颈，如果提前准备好实例会比fastJson要快一倍左右，随后我打开ObjectMapper构造方法看了下，确实比较繁琐，因此在实际应用中我们转json字符串的时候应当使用同一个ObjectMapper实例，避免每次手动去new新的实例，而FastJson是用静态方法（JSONObject.toJSONString()）因此我们在常规使用的时候不方便像jackson那样把实例提前准备好。

以后选型Jackson

### jackson的一些使用

```aidl


# java 把对象变成map 
Map<String, Object> xstbJLMap = objectMapper.readValue(objectMapper.writeValueAsString(xstbJL), Map.class);


# java 把json变成java对象 
xstbJL xstbJL = objectMapper.readValue(objectMapper.writeValueAsString(xstbJL), xstbJL.class);


# JSON数组字符串–>List

String jsonArray = "[{\"brand\":\"ford\"}, {\"brand\":\"Fiat\"}]";

ObjectMapper objectMapper = new ObjectMapper();

List<Car> cars1 = objectMapper.readValue(jsonArray, new TypeReference<List<Car>>(){});

# 把java对象->JSON
objectMapper.writeValueAsString(xstbJL)

```

### fastjson的一些使用
```
//把对象变成map
Map<String, Object> map1 = JSONObject.parseObject(JSON.toJSONString(realTimeData));

----------------------
// 将json字符串转换为json对象：
JSONObject jsonObject = JSONObject.fromObject(jsonStr);

// 将java对象转换为json对象：
JSONObject json = JSONObject.fromObject(obj);

// 将json对象转换为java对象：
Person jb = (Person) JSONObject.toBean(obj,Person.class);

// 将String的对象变成map对象
JSONObject.parseObject(data, Map.class)

// String 变成 java对象
JSONObject p = JSON.parseObject(params);
UploadParam param = JSON.toJavaObject(p, UploadParam.class);

把java对象变成String
JSONObject.toJSONString(sxtb)
```

前端转换功能

js中对 数组进行处理
js中对 数组进行处理

if (res.code == 200) {
    var data = res.data;
    if (data != null) {
        for (var i = 0; i < data.length; i++) {
            var item = data[i];
            var bzxr = {
                cBh: item.cBh,
                nAjzlb: item.nAjzlb,
                cAjzlb: item.cAjzlb,
                nDsrxh: item.nDsrxh,
                nLx: item.nLx,
                cLx: item.cLx,
                cMc: item.cMc,
                nDw: item.nDw,
                cDw: item.cDw,
            };
            _this.bzxrList.push(bzxr);
        }
    }
}

js对 对象进行处理

for (let key in data) {
    let tmp = {
        label: key[],
        value: 'line',
        data: data[key]
    };
    values.push(tmp);
}



日期的转换

@DateTimeFormat(pattern="yyyy-MM-dd HH:mm:ss") //一般是前端传给后端的时候转换使用
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss") //一般是后端传值给前端的时候
@JSONField(format ="yyyy-MM-dd HH:mm:ss") //这是albb出的一个类库






