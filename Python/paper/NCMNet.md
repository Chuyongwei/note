# NCMNET

将k邻近分为了三种sn,fn,gn三个坐标系

- 其中sn是从初始x中获取的knn
- fn是经过卷积处理后的out的knn
- gn是全局gcn后获得的out_gs1中提取的knn

1. 将三个坐标系放入out中获得三种特征点C_S1，C_F1，C_G1

   三种特征轮流去进行qkv操作获得I_S,I_F,I_G

2. out进入OANet进行隐式筛选x_up

cat进行卷积C_S1，C_F1，C_G1,x_up获得out2

重复上面操作最后获得out3



最后全局w0是out3的卷积操作

w1是全局后的

