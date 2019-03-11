## equeals()和hashCode() 
最近的代码中用到将一个对象按照他的某些字段进行分组，返回一个Map<XxxObject,List<YyyObject>
这个XxxObject 是一个对象，如果不重写hastcode方法，就会导致put一个key后，以后new XxxObject(x,y) ,想根据
这个对象，去找到value就找不到了
``` java
	 Map<A,String> stringMap =new HashMap<>();
        stringMap.put(new A("1"),"111");
        System.out.println(stringMap.get(new A("1")));
```

``` java
class  A{
    private String ss;

    public A(String ss) {
        this.ss = ss;
    }

    public String getSs() {
        return ss;
    }

    public void setSs(String ss) {
        this.ss = ss;
    }
}
```

返回是null
但是当我加上重写的HashCode()和equals()
``` java 
	@Override
    public int hashCode(){
        return  this.ss.hashCode();
    }

    @Override
    public boolean equals(Object obj) {
        A a =(A)obj;
        return this.ss.equals(a.ss);
    }
```
结果 111