# ConcurrentHashMap

### **多线程下线程安全的map集合**

### **初始化操作**

ConcurrentHashMap map = new ConcurrentHashMap();

不指定初始容量，那么默认为16，且如果不进行操作（例如put）的话，数组大小为0，也就是未初始化，会在第一次进行操作时初始化。

ConcurrentHashMap map = new ConcurrentHashMap(32);

指定初始容量，那么初始容量为（ 32 + 32 >>> 1 + 1 ），也就是 32+16+1 = 49，此时不满足2的整数次幂的性质，因此扩大为2的6次方 = 64，则初始容量为64。

### **sizeCtl变量的含义：**

![image-20200715112134323](C:\Users\13515\AppData\Roaming\Typora\typora-user-images\image-20200715112134323.png)

```java
为0：代表数组未初始化，且初始化容量为16
为正数：如果数组未初始化，代表数组的初始容量。如果数组已经初始化，代表数组扩容的阈值（初始容量*0.75）
```

### **hash值的获取**

每个键值对在插入时都需要通过对键值进行hash函数的运算，得到一个hashcode值。将得到的hashcode值右移16位（右移相当于除以2操作）后与本来的hashcode值进行异或操作，这样做的的目的是为了减少碰撞，既保留了高16位的特征，也保留了低16位的特征（int类型4字节16位），得到的值再与一个二进制正数（最高位为0其余为1）进行&操作，目的是保证得出的hash值为正数。

![image-20200715131711839](C:\Users\13515\AppData\Roaming\Typora\typora-user-images\image-20200715131711839.png)