## equeals()��hashCode() 
����Ĵ������õ���һ������������ĳЩ�ֶν��з��飬����һ��Map<XxxObject,List<YyyObject>
���XxxObject ��һ�������������дhastcode�������ͻᵼ��putһ��key���Ժ�new XxxObject(x,y) ,�����
�������ȥ�ҵ�value���Ҳ�����
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

������null
���ǵ��Ҽ�����д��HashCode()��equals()
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
��� 111