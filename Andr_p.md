## problems in jni
1. can not find class
  - c++线程与java线程空间不一致
  - 解决办法：
    1. override JNI_OnLoad，存储JavaVM变量为C++全局变量
    2. 定义JNIEnvWrapper，定义对象时，构造函数会从全局JavaVM中GetEnv，并且将当前线程attach到java线程，目的是获取当前线程的JNIEnv
    3. 在java线程中调用FindClass，并且作为全局变量存储
    4. 在C++线程中使用第2步获取的JNIEnv调用Method
