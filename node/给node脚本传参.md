### process.argv 方式

    - node ./index.js arg1 arg2
    - npm run <scriptname> -- arg1 arg2

业界通常有如下用法：
```
node ./index.js --arg1=somevalue --arg2=anothervalue
或
npm run <scriptname> -- --arg1=somevalue --arg2=anothervalue somearg
// process.argv 结果类似如下：
[
    '/Users/wucx/.nvm/versions/node/v12.18.4/bin/node',
    '/Users/wucx/bjhl-git/pc-live-giant/createTpl/build.js',
    '--arg1=somevalue',
    '--arg2=anothervalue',
    'somearg'
]
// 然后通过 require('yargs').argv 库，对 argv 处理，结果类似如下：
{
    _: [somearg],
    env: 'beta1', // 处理形如：--arg1=somevalue 格式的参数
    '$0': 'createTpl/build.js'  // node 执行的文件
}
```

### process.env 方式
process.env 是 node 的运行时环境变量

可在真正执行 node 脚本之前定义，如在 bash 脚本中：

```bash
REACT_APP_TAG=tag PUBLIC_URL=url npm run build

// 在程序中可通过 process.env.REACT_APP_TAG 和 PUBLIC_URL 读取这些值；
```