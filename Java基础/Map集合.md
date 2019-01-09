# Map

?	##Map集合(双列集合)
	###Map存储都是【键值对】，键不能重复，但是值可以重复。

	常见子类
		HashMap: JDK1.2有的(可以存储null键null值) 	Hashtable: JDK1.0有的(老，淘汰，不能存储null键null值)
		LinkedHashMap：它是HashMap的子类.

###Map集合的常用方法

?	`public V put(K key,V value)`
		往集合中添加【键值对】。注意：如果键重复，把值替换掉。
	`public V get(K key)`
		通过键来获取值
	`public V remove(K key)`
		通过键删除整个【键值对】。注意：把被删除的值返回。
	`public boolean containsKey(K key)`
		判断键是否存在
	`public boolean containsValue(V value)`
		判断是否包含值
		

	public Set<K> keySet()
		获取Map集合中所有的键，存放在Set集合中
	public Set<Entry<K,V>> entrySet()
		过去Map集合中所有的【键值对】对象Entry对象，把所有的Entry对象存放把Set集合中。

###Map集合遍历方式1(键找值)

?	1. 使用keySet获取所有的键
	2. 通过get方法获取键对应的值
	//1. keySet()获取所有的键，存放到Set集合中
	Set<String> keys = map.keySet();
	for (String key : keys) {
		//2. 通过键获取值
		Integer value = map.get(key);
		System.out.println(key+"="+value);
	} 
	

###Map集合遍历方式2(获取键值对)

?	//获取所有的【键值对】Entry对象S
	Set<Map.Entry<String, String>> entries = map.entrySet();
	//遍历Set集合，获取每一个Entry对象
	for (Map.Entry<String, String> entry : entries) {
		//获取Entry对象的键
		String key = entry.getKey();
		String value = entry.getValue();
		System.out.println(key+"="+value);
	}
	



?		