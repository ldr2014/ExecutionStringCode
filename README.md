# ExecutionStringCode
android执行字符串代码--实现接口可配置跳转任何Activity

所需jar包：
compile 'org.apache.commons:commons-jexl3:3.1'

好了，上代码：

/**
     * 执行字符串代码
     * @param jexlExp
     * @param map
     * @return
     */
    public static Object convertToCode(String jexlExp,Map<String,Object> map){
        JexlExpression e = jexlEngine.createExpression(jexlExp);
        JexlContext jc = new MapContext();
        for(String key:map.keySet()){
            jc.set(key, map.get(key));
        }
        return e.evaluate(jc);
    }
