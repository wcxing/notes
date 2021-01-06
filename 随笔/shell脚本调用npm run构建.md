#### 1、在shell脚本中传递参数给node脚本
```
// PUBLICK_URL=$val 在执行构建脚本中可通过 process.env.PUBLICK_URL 访问该变量
PUBLICK_URL=$val npm run react-scripts

```

#### 2、调用shell脚本时，参数按顺序传递，不可以第一个为空而第二个有值；
```
f1=val

bash ./build.sh $f2 $f1
// 这样 build.sh 接收到的只有一个参数，即 $f2=val, $f1 为空；

```

