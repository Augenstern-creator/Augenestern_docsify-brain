# 1、集合工具CollUtil



- 新建ArrayList
  - `CollUtil.newArrayList(T...values)`
  - `CollUtil.newArrayList(Collection<T> collection)`

```java
/**
 * 新建ArrayList:
 *     newArrayList(T...values)
 *     newArrayList(Collection<T> collection)
 */
@Test
public void newArrayListTest() {
    // newArrayList(T...values)
    ArrayList<String> strings = CollUtil.newArrayList("a", "b", "c");
    // newArrayList(Collection<T> collection)
    HashSet<String> strings1 = new HashSet<>();
    strings1.add("a");
    strings1.add("b");
    strings1.add("c");
    ArrayList<String> strings2 = CollUtil.newArrayList(strings1);


}
```

- `CollUtil.join(Collection<T> collection,String conjunction)` ：将集合转换为字符串
  - collection : 集合
  - conjunction ： 分隔符

```java
/**
 * 将集合转换为字符串
 */
@Test
public void joinTest() {
    String[] col= new String[]{"a","b","c","d","e"};
    List<String> colList = CollUtil.newArrayList(col);

    String str = CollUtil.join(colList, "#");
    //str -> a#b#c#d#e
    System.out.println(str);
}
```

- `CollUtil.addAll(Collection<T> collection,T[] values)` : 将全部元素加入到集合中

```java
/**
 * 加入全部 addAll(CollUtil.addAll(Collection<T> collection, T[] values)
 */
@Test
public void addAllTest() {
    ArrayList<String> strings = CollUtil.newArrayList("a", "b", "c", "d", "e");
    String[] col= new String[]{"f","g","h"};
    CollUtil.addAll(strings,col);
    strings.forEach( s -> System.out.println(s));

}
```

- `CollUtil.addAllIfNotContains(List<T> list,List<T> list)` ：将另一个列表中的元素加入到列表中，如果列表中已经存在此元素则忽略之

```java
/**
 * 将另一个列表中的元素加入到列表中，如果列表中已经存在此元素则忽略之addAllIfNotContains(List<T> list,List<T> list)
 */
@Test
public void addAllIfNotContainsTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c");
    ArrayList<String> strings2 = CollUtil.newArrayList("a","d", "e");
    Collection<String> strings3 = CollUtil.addAllIfNotContains(strings1, strings2);
    // a b c d e
    strings3.forEach( s -> System.out.println(s));
}
```

- `CollUtil.clear(Collection<?>... collections)` : 清除一个或多个集合内的元素，每个集合调用clear()方法

```java
/**
 * 清除一个或多个集合内的元素,每个集合调用 clear() 方法
 */
@Test
public void clearTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c");
    ArrayList<String> strings2 = CollUtil.newArrayList("d", "e");
    CollUtil.clear(strings1, strings2);
    strings1.forEach(s -> System.out.println(s));
    strings2.forEach(s -> System.out.println(s));
}
```

- `CollUtil.contains(Collection<?> collection,Object value)` ：判断指定集合是否包含指定值，如果集合为空（null或者空），返回`false`，否则找到元素返回`true`

```java
/**
 * 判断指定集合是否包含指定值，如果集合为空（null或者空），返回false，否则找到元素返回true
 */
@Test
public void containsTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c");
    boolean a = CollUtil.contains(strings1, "a");
    System.out.println(a);
}
```

- `CollUtil.containsAll(Collection<?> collection1,Collection<?> collection2)` ： 集合1中是否包含集合2中所有的元素，即集合2是否为集合1的子集

```java
/**
 * 集合1中是否包含集合2中所有的元素，即集合2是否为集合1的子集
 */
@Test
public void containsAllTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c");
    ArrayList<String> strings2 = CollUtil.newArrayList("a", "b");
    boolean b = CollUtil.containsAll(strings1, strings2);
    System.out.println(b);
}
```

- `CollUtil.containsAny(Collection<?> collection1,Collection<?> collection2)` ：其中一个集合在另一个集合中是否至少包含一个元素，即是两个集合是否至少有一个共同的元素

```java
/**
 * 其中一个集合在另一个集合中是否至少包含一个元素，即是两个集合是否至少有一个共同的元素
 */
@Test
public void containsAnyTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c");
    ArrayList<String> strings2 = CollUtil.newArrayList("a", "d");
    boolean b = CollUtil.containsAny(strings1, strings2);
    // true
    System.out.println(b);
}
```

- `CollUtil.countMapTest()` : 根据集合返回一个元素计数的Map: 所谓元素计数就是假如这个集合中某个元素出现了n次，那将这个元素做为key，n做为value
  - 例如: [a,b,c,c,c] 得到：
  - a: 1
  - b: 1
  - c:  3 

```java
/**
 * 根据集合返回一个元素计数的Map: 所谓元素计数就是假如这个集合中某个元素出现了n次，那将这个元素做为key，n做为value
 * 例如: [a,b,c,c,c] 得到：
 * a: 1
 * b: 1
 * c: 3
 */
@Test
public void countMapTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c","c","c");
    Map<String, Integer> stringIntegerMap = CollUtil.countMap(strings1);
    //遍历stringIntegerMap
    for (Map.Entry<String,Integer> entry: stringIntegerMap.entrySet()) {
        // key = a, value = 1
        // key = b, value = 1
        // key = c, value = 3
        System.out.println("key = " + entry.getKey() + ", value = " + entry.getValue());
    }
}
```

- `CollUtil.distinct(Collection<T> collection)` : 去重集合

```java
/**
 * 去重集合
 */
@Test
public void distinctTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c","c","c");
    ArrayList<String> distinct = CollUtil.distinct(strings1);
    // a b c
    distinct.forEach(a -> System.out.println(a));
}
```

- `CollUtil.intersection(Collection<T> collection1,Collection<T> collection2)` : 两个集合的交集(或者多个集合的交集)

```java
/**
 * 获取两个集合的交集
 */
@Test
public void intersectionTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c");
    ArrayList<String> strings2 = CollUtil.newArrayList("a", "d");
    Collection<String> intersection = CollUtil.intersection(strings1, strings2);
    // a
    intersection.forEach(s -> System.out.println(s));
}
```

- `CollUtil.isEmpty(Collection<T> collection | Map<?,?> map)` ： 集合是否为空
- `CollUtil.hasNull(Collection<T> collection)` ： 集合是否包含null元素

```java
/**
 * 集合是否为空
 */
@Test
public void isEmptyTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a", "b", "c");
    // false
    System.out.println(CollUtil.isEmpty(strings1));

    ArrayList<String> strings2 = CollUtil.newArrayList();
    // true
    System.out.println(CollUtil.isEmpty(strings2));

    HashMap<String, Object> map = new HashMap<>();
    map.put("name","qxl");
    map.put("age",21);
    // false
    System.out.println(CollUtil.isEmpty(map));
}
```

- `CollUtil.max(Collection<T> collection)` ： 取最大值

```java
/**
 * 获取最大值和最小值
 */
@Test
public void maxTest() {
    ArrayList<Integer> strings1 = CollUtil.newArrayList(1,2,2,3);
    Integer max = CollUtil.max(strings1);
    Integer min = CollUtil.min(strings1);
    // 3
    System.out.println(max);
    // 1
    System.out.println(min);
}
```

- `CollUtil.getFirst(Collection<T> collection)` : 获取集合的第一个元素
- `CollUtil.getLast(Collection<T> collection)` : 获取集合的最后一个元素

```java
/**
 * 获取集合第一个和最后一个元素
 */
@Test
public void getFirstTest() {
    ArrayList<Integer> strings1 = CollUtil.newArrayList(1,2,2,3);
    // 1
    System.out.println(CollUtil.getFirst(strings1));
    // 3
    System.out.println(CollUtil.getLast(strings1));
}
```

- `removeAny(T collection, E... element)` ：去掉集合中的多个元素，此方法直接修改原集合

```java
/**
 * 去掉集合中的多个元素，此方法直接修改原集合
 */
@Test
public void removeAnyTest() {
    ArrayList<Object> strings1 = CollUtil.newArrayList(1,2,2,3,null,"");
    // 去掉集合中的多个元素，此方法直接修改原集合
    ArrayList<Object> objects = CollUtil.removeAny(strings1, 1, 2, null);
    // 3
    objects.forEach(o -> System.out.println(o));

}
```

- `CollUtil.removeBlank(T collection)` ：去除`null`或者`""`或者空白字符串元素，此方法直接修改原集合
- `CollUtil.removeEmpty(T collection)` ：去除`null`或者`""`元素，此方法直接修改原集合
- `CollUtil.removeNull(T collection)` ：去除`null`元素，此方法直接修改原集合

```java
/**
 * 去除null或者""或者空白字符串 元素，此方法直接修改原集合
 */
@Test
public void removeBlankTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("1",null,"");
    ArrayList<String> blank = CollUtil.removeBlank(strings1);
    // 1
    blank.forEach(b -> System.out.println(b));
}
```

- `CollUtil.reverse(List<T> list)` : 反序给定List，会在原List基础上直接修改
- `CollUtil.reverseNew(List<T> list)` : 反序给定List，会创建一个新的List，原List数据不变

```java
/**
 * 反序List
 * 反序给定List，会在原List基础上直接修改 : reverse(List<T> list)
 * 反序给定List，会创建一个新的List，原List数据不变: reverseNew(List<T> list)
 */
@Test
public void reverseTest() {
    // 反序给定List，会在原List基础上直接修改
    ArrayList<String> strings1 = CollUtil.newArrayList("a","b","c");
    List<String> reverse = CollUtil.reverse(strings1);
    reverse.forEach(r -> System.out.println(r));

}
```

- `CollUtil.size(Object object)` ： 获取集合大小

```java
/**
 * 获取集合大小
 */
@Test
public void sizeTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("a","b","c");
    System.out.println(CollUtil.size(strings1));
}
```

- `CollUtil.sortByPinyin(Collection<String> collection)` : 根据汉字的拼音顺序排序

```java
/**
 * 排序集合
     * sortByPinyin(Collection<String> collection):根据汉字的拼音顺序排序
 */
@Test
public void sortByTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("秦晓","晓林","大林");
    // 根据汉字的拼音顺序排序
    List<String> strings = CollUtil.sortByPinyin(strings1);
    // 大林 秦晓 晓林
    strings.forEach(s -> System.out.println(s));

}

```

- `CollUtil.split(Collection<T> collection,int size)` ：对集合按照指定长度分段，每一个段为单独的集合，返回这个集合的列表

```java
/**
 * 分割集合：对集合按照指定长度分段，每一个段为单独的集合，返回这个集合的列表
 */
@Test
public void splitTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("秦晓","晓林","大林","JM");
    List<List<String>> split = CollUtil.split(strings1, 2);

    // [秦晓, 晓林]
    // [大林, JM]
    split.forEach(s -> System.out.println(s));



    // 双重遍历 List集合
    split.forEach(subList -> {
        subList.forEach( s -> System.out.println(s));
    });

}
```

- `CollUtil.sub(Collection<T> collection, int start,int end)` ：截取集合的部分，`[start,end)` ，从索引为 start 的部分到 end-1 的部分

```java
/**
 * 对集合切片，其他类型的集合会转换成List
 */
@Test
public void subTest() {
    ArrayList<String> strings1 = CollUtil.newArrayList("秦晓","晓林","大林","JM");
    // 截取集合索引 1、2,不包括3,索引从0开始
    List<String> sub = CollUtil.sub(strings1, 1, 3);
    sub.forEach(s -> System.out.println(s));
}
```





# 2、Map工具MapUtil



- `MapUtil.getStr(Map<?,?> map,Object key)` ：获取Map指定key的值，并转换为字符串

```java
/**
     * 获取Map指定key的值，并转换为字符串
     */
    @Test
    public void getStrTest() {

        HashMap<String, String> map = new HashMap<>();
        map.put("key1", "value1");
        map.put("key2", "value2");

        String key1 = MapUtil.getStr(map, "key1");
        // value1
        System.out.println(key1);
    }
```



- `MapUtil.getAny(Map<K,V> map,K... keys)` : 获取Map的部分key生成新的Map

```java
/**
 * 获取Map的部分key生成新的Map
 */
@Test
public void getAnyTest() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name", "qxl");
    map.put("age", "21");
    map.put("love", "jm");

    //获取Map的部分key生成新的Map
    Map<String, String> any = MapUtil.getAny(map, "name", "age");
    for (Map.Entry<String,String> entry: any.entrySet()) {
        System.out.println(entry.getKey());
        System.out.println(entry.getValue());
    }
}
```



- `MapUtil.isEmpty(Map<K,V> map)` : Map是否为空

```java
/**
 * Map是否为空
 */
@Test
public void isEmptyTest() {
    HashMap<String, String> map = new HashMap<>();
    boolean empty = MapUtil.isEmpty(map);
    // true
    System.out.println(empty);

    map.put("name","qxl");
    // false
    System.out.println(MapUtil.isEmpty(map));
}
```

- `MapUtil.of(K key,V value)` : 将单一键值对转换为Map
- `MapUtil.of(Object[] array)` : 将数组转换为Map

```java
/**
 * 将一个或多个键值对转换为Map
 */
@Test
public void ofTest() {
    HashMap<String, String> map1 = MapUtil.of("name", "qxl");
    for (Map.Entry<String,String> entry: map1.entrySet()) {
        // name
        System.out.println(entry.getKey());
        // qxl
        System.out.println(entry.getValue());
    }

    HashMap<Object, Object> map2 = MapUtil.of(new String[][]{
            {"age", "21"},
            {"love", "jm"}
    });

}
```



- `Map.removeAny(Map<K,V> map,K... keys)` ： 去掉Map中指定key的键值对，修改原Map，返回的就是修改后的 Map

```java
/**
 * removeAny(Map<K,V> map,K... keys)
 * 去掉Map中指定key的键值对，修改原Map
 */
@Test
public void removeAny() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name","qxl");
    map.put("age","22");
    map.put("love","jm");
    // 去除 name age 键值对,返回的就是修改后的map
    Map<String, String> any  = MapUtil.removeAny(map, "name", "age");
    for (Map.Entry<String,String> entry: any.entrySet()) {
        // love
        System.out.println(entry.getKey());
        // jm
        System.out.println(entry.getValue());
    }

}
```

- `MapUtil.removeNullValue(Map<K,V> map) ` : 去除Map中值为`null`的键值对，直接修改原 Map

```java
/**
 * 去除Map中值为null的键值对
 */
@Test
public void removeNullValueTest() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name","qxl");
    map.put(null,null);
    Map<String, String> map1 = MapUtil.removeNullValue(map);
    for (Map.Entry<String, String> entry : map1.entrySet()) {
        // name
        System.out.println(entry.getKey());
        // qxl
        System.out.println(entry.getValue());
    }
}
```

- `MapUtil.renameKey(Map<K,V> map,K oldKey,K newKey)` : 重命名键key，实现方式为移除然后重新put

```java
/**
 * 重命名键key
 */
@Test
public void renameKeyTest() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name","qxl");
    // 修改name为newName
    MapUtil.renameKey(map,"name","newName");
    for (Map.Entry<String, String> entry : map.entrySet()) {
        // newName
        System.out.println(entry.getKey());
    }

}
```

- `MapUtil.sort(Map<K,V> map)` ： 排序已有Map，Key有序的Map，使用默认Key排序方式（字母顺序）
- `sortByValue(Map<K,V> map,boolean isDesc)` : 按照值排序，可选是否倒序

```java
/**
 * 排序
 * sort(Map<K,V> map): 排序已有Map，Key有序的Map，使用默认Key排序方式（字母顺序）
 * sortByValue(Map<K,V> map,boolean isDesc): 按照值排序，可选是否倒序
 */
@Test
public void sortTest() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name","qxl");
    map.put("age","21");
    TreeMap<String, String> sort = MapUtil.sort(map);
    for (Map.Entry<String,String> entry: sort.entrySet()) {
        System.out.println(entry.getKey());
        System.out.println(entry.getValue());
    }
}

```

- `MapUtil.reverse(Map<K,V> map)` ： Map的键和值互换

```java
/**
 * Map的键和值互换
 */
@Test
public void reverseTest() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name","qxl");
    map.put("age","21");
    Map<String, String> reverseMap = MapUtil.reverse(map);
    for (Map.Entry<String,String> entry: reverseMap.entrySet()) {
        // qxl 21
        System.out.println(entry.getKey());
        // name age
        System.out.println(entry.getValue());
    }
}
```

- `MapUtil.join(Map<K,V> map,String separator,String keyValueSeparator, boolean isIgnoreNull  )` : 将map转成字符串
  - map ： 要修改的map
  - separator： entry 之间的连接符
  - keyValueSeparator：key 、 value 之间的连接符
  - isIgnoreNull  ： 是否忽略null的键和值

```java
/**
 * 将map转成字符串
 */
@Test
public void joinTest() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name","qxl");
    map.put("age","21");
    String join = MapUtil.join(map, "+", "+", true);
    // name+qxl+age+21
    System.out.println(join);
}
```





# 3、JSON工具JSONUtil

- `JSONUtil.toJsonPrettyStr(JSON json | Object obj)`：将任意对象（Bean、Map、集合等）直接转换为格式化后的JSON字符串

```java
/**
 * 将任意对象（Bean、Map、集合等）直接转换为格式化后的JSON字符串
 */
@Test
public void toJsonPrettyStrTest() {
    HashMap<String, String> map = new HashMap<>();
    map.put("name","qxl");
    map.put("age","21");
    String s = JSONUtil.toJsonPrettyStr(map);
    /*
    {
        "name": "qxl",
        "age": "21"
    }
    */
    System.out.println(s);

}
```

- `JSONUtil.parse(Object obj [,JSONConfig config])` ：转换对象为JSON，如果用户不配置JSONConfig，则JSON的有序与否与传入对象有关。
- `JSONUtil.parseArray(String jsonStr)` ：JSON字符串转JSONArray
- `JSONUtil.parseObj(Object obj)` ：JSON字符串转JSONObject对象

```java
/**
 * 转换对象为JSON
 */
@Test
public void parseTest() {
    People people = new People("qxl", 21);
    // {"name":"qxl","age":21}
    JSON parse = JSONUtil.parse(people);


    // JSON字符串转JSONArray: JSONUtil.parseArray(String jsonStr)
    JSONArray objects = JSONUtil.parseArray(parse);
    // [{"name":"qxl"},{"age":21}]
    System.out.println(objects);


    // JSON字符串转JSONObject对象: JSONUtil.parseObj(Object obj) : json对象也是对象
    JSONObject entries = JSONUtil.parseObj(parse);
    // {"name":"qxl","age":21}
    System.out.println(entries);
}
```

- `JSONUtil.toJSONStr(JSON json)` ：将JSON对象转为String类型,即将JSON对象转为JSON字符串

```java
/**
 * 将JSON对象转为String类型,即将JSON对象转为JSON字符串
 */
@Test
public void toJSONStrTest() {
    People people = new People("qxl", 21);
    // {"name":"qxl","age":21}
    JSON parse = JSONUtil.parse(people);

    // {"name":"qxl","age":21}  将JSON对象转为String类型
    String s = JSONUtil.toJsonStr(parse);
    System.out.println(s);

}
```





# 4、JSON对象JSONObject

JSONObject代表一个JSON中的键值对象，这个对象以大括号包围，每个键值对使用`,`隔开，键与值使用`:`隔开，一个JSONObject类似于这样：

```json
{
  "key1":"value1",
  "key2":"value2"
}
```

此处键部分可以省略双引号，值为字符串时不能省略，为数字或布尔值时不加双引号。



- `new JSONObject()` ： 创建JSON对象
- `JSONObject.set(String key,Object value)` ： 设置键值对给JSON对象
- `toString()` ：转成JSON字符串
- `toStringPretty()` ：转成格式化JSON字符串

```java
/**
 * 创建JSON对象 new JSONObject()
 * 设置键值对给JSON对象: set(String key,Object value)
 *
 */
@Test
public void newTest() {
    JSONObject entries = new JSONObject();
    entries.set("name","qxl");
    entries.set("age",21);
    // 返回JSON字符串: toString()  {"name":"qxl","age":21}
    System.out.println(entries.toString());
    // 返回格式化的JSON字符串: toStringPretty()  
    /*
        {
            "name": "qxl",
                "age": 21
        }
    */
    System.out.println(entries.toStringPretty());

}
```

- javaBean解析

首先我们定义一个Bean：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class People implements Serializable {
    private String name;
    private Integer age;

}
```

解析为JSON：

```java
/**
 * JavaBean解析
 */
@Test
public void javaBeanTest() {
    People people = new People();
    people.setName("qxl");
    people.setAge(22);

    // 把对象解析为JSON
    JSONObject entries = JSONUtil.parseObj(people);
    /*
        {
            "name": "qxl",
                "age": 22
        }
    */
    System.out.println(entries.toStringPretty());
}
```

# 5、字段验证器Validator

验证给定字符串是否满足指定条件，一般用在表单字段验证里。此类中全部为静态方法。直接调用`Validator.isXXX(String value)`既可验证字段，返回是否通过验证。

- `Validator.isEmail(String value)` : 验证是否为可用邮箱地址

```java
/**
 * 验证是否为可用邮箱地址
 */
@Test
public void isEmailTest() {
    // true
    System.out.println(Validator.isEmail("123@163.com"));
    // false
    System.out.println(Validator.isEmail("123@ss"));
}
```



- `Validator.hasChinese(String value)`:验证是否包含汉字
- `Validator.isChinese(String value)` ： 验证是否都为汉字
- `Validator.hasNumber(Number value)`：验证是否包含数字  
- `Validator.isBetween(Number value, Number min, Number max)`：检查给定的数字是否在指定范围内

```java
/**
 * 验证是否包含汉字 ： Validator.hasChinese(String value)
 * 验证是否都为汉字 ： Validator.isChinese(String value)
 * 验证是否包含数字 ： Validator.hasNumber(Number value)
 * 检查给定的数字是否在指定范围内 : Validator.isBetween(Number value, Number min, Number max)
 */
@Test
public void hasChinesTest() {
    // 验证是否包含汉字 ： Validator.hasChinese(String value)
    boolean b = Validator.hasChinese("秦晓");
    // true
    System.out.println(b);

    // 验证是否都为汉字 ： Validator.isChinese(String value)
    boolean b1 = Validator.isChinese("秦晓123");
    // false
    System.out.println(b1);


    // 验证是否包含数字 ： Validator.hasNumber(Number value)
    boolean b2 = Validator.hasNumber("123");
    // true
    System.out.println(b1);

    // 检查给定的数字是否在指定范围内 : Validator.isBetween(Number value, Number min, Number max)
    boolean between = Validator.isBetween(5, 1, 100);
    // true
    System.out.println(between);

}
```

- `Validator.isCitizenId(String value)` ：验证是否是身份证号（支持18位、15位和港澳台的10位）

```java
/**
 * 验证是否是身份证号（支持18位、15位和港澳台的10位）
 */
@Test
public void isCitizenIdTest() {
    boolean creditCode = Validator.isCitizenId("610524200109078976");
    // false
    System.out.println(creditCode);
}
```

- `Validator.isGeneral(String value)` : 验证是否是英文字母、数字和下划线
- `Validator.isGeneral(String value,int min,int max)` : 验证是否为给定长度范围的英文字母 、数字和下划线

```java
/**
 * Validator.isGeneral(String value) : 验证是否是英文字母、数字和下划线
 * Validator.isGeneral(String value,int min,int max) : 验证是否为给定长度范围的英文字母 、数字和下划线
 */
@Test
public void isGeneralTest() {
    // true
    System.out.println(Validator.isGeneral("_qxl123"));
    // true
    System.out.println(Validator.isGeneral("_qxl123", 3, 10));

}
```

- `Validator.isLetter(String value)`:判断字符串是否全部为字母组成，包括大写和小写字母和汉字
- `Validator.isLowerCase(String value)`:判断字符串是否全部为小写字母
- `Validator.isUpperCase(String value)`:判断字符串是否全部为大写字母
- `Validator.isWord(String value)`:验证该字符串是否是字母（包括大写和小写字母）

```java
/**
 * Validator.isLetter(String value):判断字符串是否全部为字母组成，包括大写和小写字母和汉字
 * Validator.isLowerCase(String value):判断字符串是否全部为小写字母
 * Validator.isUpperCase(String value):判断字符串是否全部为大写字母
 * Validator.isWord(String value):验证该字符串是否是字母（包括大写和小写字母）
 */
@Test
public void isTest() {
    // true 判断字符串是否全部为字母组成，包括大写和小写字母和汉字
    System.out.println(Validator.isLetter("HelloWorld你好"));
    // false   判断字符串是否全部为小写字母
    System.out.println(Validator.isLowerCase("HelloWorld"));
    // false  判断字符串是否全部为大写字母
    Validator.isUpperCase("HelloWorld");
    // true 验证该字符串是否是字母（包括大写和小写字母）
    Validator.isWord("HelloWorld");
}
```



# 6、枚举工具EnumUtil

首先创建一个枚举类：

```java
public enum Sex {
    BOY(18),GIRL(16);

    public String name;
    public int age;


    Sex(int age) {
        this.age = age;
    }

    public int getAge(){
        return this.age;
    }
}
```

- `EnumUtil.getNames(枚举类.class)` : 获取枚举类中所有枚举对象的name列表

```java
/**
 * 获取枚举类中所有枚举对象的name列表
 */
@Test
public void getNamesTest() {
    List<String> names = EnumUtil.getNames(Sex.class);
    // BOY GIRL
    names.forEach( n -> System.out.println(n));
}
```

- `EnumUtil.getFieldValues(枚举类.class, String fieldName)` ：获得枚举类中各枚举对象下指定字段的值
  - `fieldName` - 字段名，最终调用getXXX方法

```java
/**
 * 获得枚举类中各枚举对象下指定字段的值
 */
@Test
public void getFieldValues() {
    List<Object> boy = EnumUtil.getFieldValues(Sex.class, "age");
    // 18 16
    boy.forEach( b -> System.out.println(b));
}
```





# 7、数组工具ArrayUtil

- `ArrayUtil.addAll(T[]... arrays)` ： 将多个数组合并在一起，忽略null的数组

```java
/**
 * 将多个数组合并在一起，忽略null的数组
 */
@Test
public void addAllTest() {
    int[] a1= {0,1,2,3};
    int[] a2 = {4,5,6,7};
    int[] ints = ArrayUtil.addAll(a1, a2);
    for (int i = 0; i < ints.length; i++) {
        System.out.println(ints[i]);
    }

}
```

- `ArrayUtil.clone(T[] array)` : 克隆数组

```java
/**
 * 克隆数组
 */
@Test
public void cloneTest() {
    int[] a1 = {1,2,3};
    int[] clone = ArrayUtil.clone(a1);
    for (int i = 0; i < clone.length; i++) {
        System.out.println(clone[i]);
    }
}
```

- `ArrayUtil.contains(T[] array,T value) ` : 数组中是否包含元素
- `ArrayUtil.containsAll(T[] array,T... values)` : 数组中是否包含指定元素中的全部
- `ArrayUtil.containsAny(T[] array,T... values)` : 数组中是否包含指定元素中的任意一个
- `ArrayUtil.containsIgnoreCase(String[] array,String... values)` : 数组中是否包含元素，忽略大小写

```java
/**
 * ArrayUtil.contains(T[] array,T value) :数组中是否包含元素
 * ArrayUtil.containsAll(T[] array,T... values) : 数组中是否包含指定元素中的全部
 * ArrayUtil.containsAny(T[] array,T... values) : 数组中是否包含指定元素中的任意一个
 * ArrayUtil.containsIgnoreCase(String[] array,String... values) : 数组中是否包含元素，忽略大小写
 */
@Test
public void containsTest() {
    String[] s = {"a","b","c","d"};
    // true 数组中是否包含元素
    System.out.println(ArrayUtil.contains(s, "a"));
    // true 数组中是否包含指定元素中的全部
    System.out.println(ArrayUtil.containsAll(s, "a", "b"));
    // true 数组中是否包含指定元素中的任意一个
    System.out.println(ArrayUtil.containsAny(s,"a", "e"));
    // true 数组中是否包含元素，忽略大小写
    System.out.println(ArrayUtil.containsIgnoreCase(s,"A"));
}
```

- `ArrayUtil.distinct(T[] array)` : 去重数组中的元素，去重后生成新的数组，原数组不变

```java
/**
 * 去重数组中的元素，去重后生成新的数组，原数组不变
 */
@Test
public void distinctTest() {
    String[] s = {"a","a","c","d"};
    String[] distinct = ArrayUtil.distinct(s);
    for (int i = 0; i < distinct.length; i++) {
        // a c d
        System.out.println(distinct[i]);
    }
}
```

- `ArrayUtil.equals(T[] array1,T[] array2)` : 判断两个数组是否相等，判断依据包括数组长度和每个元素都相等。

```java
/**
 * 判断两个数组是否相等，判断依据包括数组长度和每个元素都相等。
 */
@Test
public void equalsTest() {
    String[] s1 = {"a","a","c","d"};
    String[] s2 = {"a","a","c","d"};
    // true
    System.out.println(ArrayUtil.equals(s1, s2));
}
```

- `ArrayUtil.remove(T[] array,int index)` : 移除数组中对应位置的元素
- `ArrayUtil.removeBlank(T[] array)`：去除null或者""或者空白字符串元素
- `ArrayUtil.removeEle(T[] array)`：移除数组中指定的元素 只会移除匹配到的第一个元素
- `ArrayUtil.removeEmpty(T[] array)`：去除null或者 "" 元素
- `ArrayUtil.removeNull(T[] array)`：去除null 元素

```java
/**
 * ArrayUtil.remove(T[] array,int index):移除数组中对应位置的元素
 * ArrayUtil.removeBlank(T[] array) : 去除null或者""或者空白字符串 元素
 * ArrayUtil.removeEle(T[] array) : 移除数组中指定的元素 只会移除匹配到的第一个元素
 * ArrayUtil.removeEmpty(T[] array) : 去除null或者"" 元素
 * ArrayUtil.removeNull(T[] array) : 去除null 元素
 */
@Test
public void removeTest() {
    String[] array = {"1","2","3",null,""};
    // 2 3 null 移除数组中对应位置的元素
    String[] remove = ArrayUtil.remove(array, 0);
    for (int i = 0; i < remove.length; i++) {
        System.out.println(remove[i]);
    }
}
```

- `ArrayUtil.append(Object array,T... newElements)` ：将新元素添加到已有数组中 添加新元素会生成一个新的数组，不影响原数组
- `ArrayUtil.append(T[] buffer,T... newElements)` : 将新元素添加到已有数组中 添加新元素会生成一个新的数组，不影响原数组

```java
/**
 * ArrayUtil.append(Object array,T... newElements) : 将新元素添加到已有数组中 添加新元素会生成一个新的数组，不影响原数组
 * ArrayUtil.append(T[] buffer,T... newElements) : 将新元素添加到已有数组中 添加新元素会生成一个新的数组，不影响原数组
 */
@Test
public void appendTest() {
    Integer[] array = {1,2};
    Integer[] append = ArrayUtil.append(array, 3, 4, 5);
    // 输出数组元素 [1, 2, 3, 4, 5]
    System.out.println(Arrays.toString(append));
    System.out.println(ArrayUtil.toString(append));
}
```

- `ArrayUtil.replace(T[] array,int index,T... values)` : 将新元素插入到到已有数组中的某个位置,返回一个新的数组,不影响原数组

```java
/**
 * ArrayUtil.replace(T[] array,int index,T... values): 将新元素插入到到已有数组中的某个位置,返回一个新的数组,不影响原数组
 */
@Test
public void replaceTest() {
    Integer[] array = {1,2,3};
    Integer[] replace = ArrayUtil.replace(array, 3, 4, 5);
    // 输出数组元素  [1, 2, 3, 4, 5]
    System.out.println(Arrays.toString(replace));
    System.out.println(ArrayUtil.toString(replace));
}
```

- `ArrayUtil.getAny(Object array,int... indexes)` : 获取数组中指定多个下标元素值，组成新数组

```java
/**
 * ArrayUtil.getAny(Object array,int... indexes):获取数组中指定多个下标元素值，组成新数组
 */
@Test
public void getAnyTest() {
    int[] array = {1,2,3,4};
    Integer[] any = ArrayUtil.getAny(array, 1, 2);
    // [2, 3]
    System.out.println(ArrayUtil.toString(any));
}
```

- `ArrayUtil.indexOf(T[] array,Object value)` : 返回数组中指定元素所在位置
- `ArrayUtil.indexOfIgnoreCase(String[] array,String value)` : 返回数组中指定元素所在位置，忽略大小写

```java
/**
 * ArrayUtil.indexOf(T[] array,Object value) : 返回数组中指定元素所在位置
 * ArrayUtil.indexOfIgnoreCase(String[] array,String value) : 返回数组中指定元素所在位置，忽略大小写
 */
@Test
public void indexOfTest() {
    Integer[] array = {1,2,3};
    int i = ArrayUtil.indexOf(array, 2);
    // 1 返回索引
    System.out.println(i);

    String[] s = {"a","b","c","d","e","f"};
    int e = ArrayUtil.indexOfIgnoreCase(s, "E");
    // 4 返回索引
    System.out.println(e);
}
```

- `ArrayUtil.insert(T[] buffer,int index,T... newElements)`:将新元素插入到到已有数组中的某个位置,添加新元素会生成一个新的数组，不影响原数组

```java
/**
 * ArrayUtil.insert(T[] buffer,int index,T... newElements):将新元素插入到到已有数组中的某个位置,添加新元素会生成一个新的数组，不影响原数组
 *
 */
@Test
public void insertTest() {
    Integer[] array = {1,2,3,4};
    Integer[] insert = ArrayUtil.insert(array, 4, 5, 6, 7, 8);
    // [1, 2, 3, 4, 5, 6, 7, 8]
    System.out.println(ArrayUtil.toString(insert));
}
```

- `ArrayUtil.join(T[] array,String conjunction)`: 以 conjunction 为分隔符将数组转换为字符串 返回连接后的字符串

```java
/**
 * ArrayUtil.join(T[] array,String conjunction): 以 conjunction 为分隔符将数组转换为字符串 返回连接后的字符串
 */
@Test
public void joinTest() {
    Integer[] array = {1,2,3,4,5,6,7,8};
    String join = ArrayUtil.join(array, "+");
    // 1+2+3+4+5+6+7+8
    System.out.println(join);

}
```

- `ArrayUtil.max(T[] numberArray)`:取最大值
- `ArrayUtil.min(T[] numberArray)`:取最小值

```java
/**
 * ArrayUtil.max(T[] numberArray):取最大值
 * ArrayUtil.min(T[] numberArray):取最小值
 * 取最值
 */
@Test
public void maxTest() {
    Integer[] array = {1,2,3,4,5,6,7,8};
    System.out.println(ArrayUtil.max(array));
    System.out.println(ArrayUtil.min(array));
}
```

- `ArrayUtil,reverse(T[] array) ` 反转数组,会变更原数组

```java
/**
 * ArrayUtil,reverse(T[] array) : 反转数组,会变更原数组
 */
@Test
public void reverseTest() {
    Integer[] array = {1,2,3,4,5,6,7,8};
    Integer[] reverse = ArrayUtil.reverse(array);
    // [8, 7, 6, 5, 4, 3, 2, 1]
    System.out.println(ArrayUtil.toString(reverse));
}
```

- `ArrayUtil.swap(T[] array,int start, int end)` ：交换数组中两个位置的值

```java
/**
 * ArrayUtil.swap(T[] array,int start, int end) : 交换数组中两个位置的值
 */
@Test
public void swapTest() {
    Integer[] array = {1,2,3,4,5,6,7,8};
    Integer[] swap = ArrayUtil.swap(array, 0, 5);
    // [6, 2, 3, 4, 5, 1, 7, 8]
    System.out.println(ArrayUtil.toString(swap));
}
```











































































