1. 通过AttachCurrentThread附加到虚拟机的线程在查找类时只会通过系统类加载器进行查找，不会通过应用类加载器进行查找，因此可以加载系统类，但是不能加载非系统类。解决办法：方案1：获取classLoader，通过classLoader的loadClass来加载自定义类，适合自定义类较多的情况；方案2：在主线程创建一个全局的自定义类引用，适合自定义类较少的情况。
2. 重写JNI_OnLoad函数，将JavaVM保存至全局变量中，每次由jni函数注册（Java线程）时，调用findclass、newglobalref存储class对象，在C++线程回调java类时，由该变量获取java中对象
3. JNI引用表中保存的是JNI局部引用，最大空间是512个，如果超出这个范围，JVM就会挂掉。所以在循环中注意使用DeleteLocalRef移除不使用的引用。JNI的基本类型不会占用引用表空间

## C++调用Java实例方法及属性
1. FindClass搜索类，并返回该类的class对象clazz
2. 从clazz中查找静态方法：GetStaticMethodID
3. 调用该静态方法：CallStaticVoidMethod
4. 删除局部引用：DeleteLocalRef(clazz)

- 如果调用非静态方法，需要先调用NewObject构造类对象，并在结束时释放
- 获取methodid时，需要传入方法签名，签名规则
  - (形参参数类型列表)返回值
  - 形参参数列表中，引用类型以L开头，后面紧跟类的全路径名（需要将.全部替换为/)，以分号结尾
  |签名|函数定义|
  |:---:|:---:|
  |"()Ljava/lang/String;"|"String f();"|
  |"(ILjava/lang/Class;)J"|"long f(int i, Class c);"|
  |"([B)V"|"String(byte[] bytes);"|
  
  |签名|基本类型|
  |:---:|:---:|
  |Z|boolean|
  |B|byte|
  |C|char|
  |S|short|
  |I|int|
  |J|long|
  |F|float|
  |D|double|



