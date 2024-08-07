[pnpm官方文档](https://pnpm.io/zh/installation)

```less
# 全局安装
npm install pnpm -g

# 查看源
pnpm config get registry 
# 切换淘宝源
pnpm config set registry https://registry.npmmirror.com/

# 使用
pnpm install <package>
pnpm i <package>
pnpm add <package>    # -S  默认写入dependencies
pnpm add -D    # -D devDependencies
pnpm add -g    # 全局安装

# 移除
pnpm remove <package>                  
pnpm remove <package> --global        

# 更新所有依赖项
pnpm up      
pnpm upgrade <package>
pnpm upgrade <package> --global

# 安装完记得重启下环境使其生效
# windows环境不好整就直接重启，重启后指定目录会生效
pnpm config set store-dir E:/pnpmStore


# 一些需要node-gyp的包一定要记得安装好

# 在系统上禁止使用脚本解决方法
# 以管理员身份运行power shell
set-executionpolicy remotesigned
```

|npm 命令|pnpm 等效|
|---|---|
|​npm install​​​|​pnpm install​​​|
|​npm i pkg​​​|pnpm add pkg​​​|
|npm run cmd​​| pnpm cmd​​|
问题：

1. 安装electron 执行 node install.js 报错 RequestError: read ECONNRESET

```less
pnpm config set ELECTRON_MIRROR http://npm.taobao.org/mirrors/electron/
```

