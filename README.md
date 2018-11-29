# ExecutionStringCode
android执行字符串代码--实现接口可配置跳转任何Activity

所需jar包：
compile 'org.apache.commons:commons-jexl3:3.1'

好了，上代码（完整代码也可以参见RunString.java）：

    /**
     * 执行字符串代码方法
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


*******************拼装字符串代码，实现接口定义打开任意Activity*******************


    /**
     * 跳转的Activity 可配置
     * @param cls            activity
     * @param packageContext context
     * @param itemId         传参
     */
    public static void testExecuteExpression(Context packageContext, Class<?> cls , String itemId) {
        Intent intent = new Intent();
        //组装代码 ， 当然了，字符串里面 比如 intent 、 packageContext 是需要放在map里的
        Map<String,Object> map2=new HashMap<String,Object>();
        map2.put("packageContext",packageContext);
        map2.put("itemId",itemId);
        map2.put("intent",intent);
        map2.put("class",cls);
        String expression2="intent.setClass(packageContext, class)#intent.putExtra('item_id', itemId)#packageContext.startActivity(intent)";
        String [] ss = expression2.split("#");
        for (int i = 0; i < ss.length; i++) {
            convertToCode(ss[i] , map2);
        }
    }

    /**
     * 执行 'com.test.ldr.ldr_test.ItemDetailActivity' 这种指定页面的是String类型的
     * @param packageContext context
     * @param itemId         传参
     */
    public static void executeStringClassName(Context packageContext , String itemId) {
        Intent intent = new Intent();
        //组装代码 ， 当然了，字符串里面 比如 intent 、 packageContext 是需要放在map里的
        Map<String,Object> map2=new HashMap<String,Object>();
        map2.put("packageContext",packageContext);
        map2.put("itemId",itemId);
        map2.put("intent",intent);
        String expression2= "intent.setClassName(packageContext, 'com.test.ldr.ldr_test.ItemDetailActivity')#intent.putExtra('item_id', itemId)#packageContext.startActivity(intent)";
        String [] ss = expression2.split("#");
        for (int i = 0; i < ss.length; i++) {
            convertToCode(ss[i] , map2);
        }
    }
    
    
    
    嗯，就这样。还有其他更多用途，这里只是给了其中一种。 伙伴们可以结合实际需求，挖掘挖掘
