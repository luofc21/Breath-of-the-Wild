---
pageClass: 'diary vscode-plugin'
---

## VS Code插件开发

### vscode-js-console-utils-plus

#### 插件功能

:eyes: **该插件在`vscode-js-console-utils`插件的基础上进行二次开发。**

1. 使用快捷键`CTRL+SHIFT+W`输出`console.log(*)`语句；且可通过配置文件设置自定义输出语句样式。
   
2. 使用快捷键`CTRL+SHIFT+A`注释打开文件的全部`console.log(*)`语句。
   
3. 使用快捷键`CTRL+SHIFT+D`删除打开文件的全部`console.log(*)`语句。
   
#### 开发步骤

1. 创建插件工程
   
2. 功能开发
   
3. 调试（与nodejs调试方法大体上类似）

   a. 可直接按F5即可弹出一个新的调试编辑器窗口。
   
   b. 也可通过编辑器侧边栏的·运行和调试·Tab页的开始调试按钮弹出调试窗口。
   
4. 打包与发布
   
   a. 安装`vsce`包(`npm install -g vsce`)。
   
   b. 执行`vsce package`命令即可生成一个以.vsix为后缀的文件。后续可直接通过导入该文件来安装插件[通过VSIX文件安装插件](https://github.com/microsoft/vscode-docs/blob/main/docs/editor/extension-marketplace.md#install-from-a-vsix)。
   
   c. 执行`vsce publish`命令发布插件到应用扩展商店。发布之前还可执行3项前置操作：a.创建Azure DevOps 组织，获取Personal Access Token; b. 执行`vsce create-publisher (publisher name)`创建发行方; c. 执行`vsce login (publisher name)`登录发行方。执行完之后即可在`vsce publish`命令中加入参数`-p <token>`进行发布。
