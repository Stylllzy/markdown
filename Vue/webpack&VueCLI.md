[toc]

# webpack

前端模块化打包工具

## grunt/gulp和webpack的区别

grunt/gulp 更加强调前端流程的自动化,模块化不是核心

webpack 更强调模块化开发管理,其中文件压缩合并,预处理等功能,是附带的功能

## webpack中的loader

- css-loader/style-loader
- less-loader
- url-loader/file-loader
- babel-loader                                                                                                                                                                                                                                                                                                                                                                                                                                                          

# Vue CLI@2

## runtime- compiler 和 runtime-only 的区别 

在 main.js文件中

only中没注册组件,直接使用render,周期变为	render -> vDOM -> UI

only 性能更高,代码量更少

# Vue CLI@3

## 与 Vue CLI @2区别

- vue-cli 3基于webpack 4,vue-cli2基于webpack3
- vue-cli3 设计原则"0配置",移除配置文件根目录下的 build 和 config 等目录
- vue-cli3提供 vue ui 命令,提供可视化配置
- 移除 static文件夹,新增 public 文件夹,并将 index.html 移动到public中