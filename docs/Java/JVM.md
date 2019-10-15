JVM内存模型

![JVM内存模型](https://upload-images.jianshu.io/upload_images/10006199-a4108d8fb7810a71.jpeg "JVM内存模型")

![Heap](https://mmbiz.qpic.cn/mmbiz_png/qdzZBE73hWsbhfAng9ibqfcbjrqgyRWqAKiaJ2U75SGYwQhs2tuNbXtu8KIpaUsBOaHRKXf7esuuFoMjELFxibIVg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "堆模型")

Java堆主要分为 新生代 和 老年代， 其中新生代又分 Eden区 和 Survivor区，其中 Survivor 区 又分成from和to两个区。

其中的比例为：

老年代 ： 新生代 = 2 ： 1

Eden : from : to = 8 : 1 : 1

