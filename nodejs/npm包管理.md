> 只要package.json在手,node_modules随便删 => 前提是依赖都在文件里面

### npm包管理器

- 升级包管理器

  ```
  npm i -g npm
  npm i -g cnpm
  ```

- 初始化package.json文件

  ```
  npm init [-y] | [--yes]
  ```

- 加载npm依赖

  ```
  npm install
  ```

- 修改npm默认config

  ```
  npm config set init-author-name "lsj"
  npm set init-license "MIT"
  ...
  ```

- 获取npm config

  ```
  npm [config] get init-author-name
  ```

- 删除npm config

  ```
  npm config delete init-author-name
  npm config delete init-license
  ...
  ```

- 加载依赖(可以多个依赖同时加载)

  > [npm will `--save` by default now](https://twitter.com/maybekatz/status/859229741676625920). Additionally, `package-lock.json` will be automatically created unless an `npm-shrinkwrap.json` exists. ([#15666](https://github.com/npm/npm/pull/15666))
  >
  > ==> npm 在v5.0.0已将--save为默认行为

  ```
  ## env = prod生产环境 => 依赖添加到"dependencies"
  npm install|i lodash [--save]
  
  ## env = dev开发环境 => 依赖添加到"devDependencies"
  npm install gulp gulp-css --save-dev
  
  ## 只加载dependencies中的依赖
  npm install --production
  
  ## 加载指定版本依赖
  npm install jquery@3.4.1
  
  ## 全局加载依赖=>注意:不会添加到dependencies依赖中
  npm install -g jquery
  ```

- 移除依赖

  ```
  ## 移除env = dev依赖
  npm uninstall|un|remove gulp --save-dev
  ## 移除env = prod依赖
  npm uninstall lodash --save
  ## 移除全局依赖
  npm uninstall -g nodemon
  ```

- 更新依赖

  ```
  npm update lodash
  ```

- 获取npm全局node_modules路劲

  ```
  npm root -g
  ```

- version说明

  ```
  v8.2.0
  => num1: Major Version
  => num2: Minor Version
  => num3: Patch Bug fixes
  ```

- 神器: nodemon

  ```
  $ npm i nodemon -g
  
  $ nodemon => 会监视源文件中任何的更改并自动重启服务器(配合live-server使用特别舒服)
  
  // vscode也有该插件
  $ npm i live-server
  
  $ live-server
  ```

- 列举依赖

  ```
  npm list
  npm list --depth 0 // 同时列举文件位置(数字代表查询层级0=>只列举层级1,1=>列举1-2层...)
  ```

- 脚本运行

  ```
   "scripts": {
      "app": "node app.js",
      "server": "live-server"
    }
    
    npm app => 运行test脚本
    npm run serve
  ```

  

---

[参考资料]

[npm v5.0.0版本更新](https://github.com/npm/npm/releases/tag/v5.0.0)

[Youtube: NPM Crash Course](https://www.youtube.com/watch?v=jHDhaSSKmB0)

[npmjs](https://www.npmjs.com/)