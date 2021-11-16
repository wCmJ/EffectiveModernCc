## problems in jni
1. can not find class
  - c++线程与java线程空间不一致
  - 解决办法：
    1. override JNI_OnLoad，存储JavaVM变量为C++全局变量
    2. 定义JNIEnvWrapper，定义对象时，构造函数会从全局JavaVM中GetEnv，并且将当前线程attach到java线程，目的是获取当前线程的JNIEnv
    3. 在java线程中调用FindClass，并且作为全局变量存储
    4. 在C++线程中使用第2步获取的JNIEnv调用Method
  - 通过AttachCurrentThread附加到虚拟机的线程在查找类时只会通过系统类加载器进行查找，不会通过应用类加载器进行查找，因此可以加载系统类，但是不能加载非系统类。解决办法：方案1：获取classLoader，通过classLoader的loadClass来加载自定义类，适合自定义类较多的情况；方案2：在主线程创建一个全局的自定义类引用，适合自定义类较少的情况。
