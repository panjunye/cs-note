# 四大引用
- 强引用 StrongRefreence
    > 只要强引用还存在，垃圾回收器就不会回收掉被引用的对象
- 弱引用 WeakRefreence 
    > 只要垃圾回收器发现弱引用，就会回收其指向的对象
- 软引用 SoftRefreence
    > 只有在内存不足的情况下，垃圾回收器才会回收弱引用指向的对象
- 虚引用 PhantomReference
    > 一个对象是否有虚引用，不会对其生存时间有影响。虚引用的唯一目的就是在这个对象被回收的时候获得系统的通知。创建虚引用必须传入ReferenceQueue。


# Type
Type的类型：
- WildcardType 通配符
- TypeVariable
- GenericArrayType
- ParameterizedType
- Class


# HashMap

- LinkedHashMap
    > 通过Iterator可以按照顺序遍历


