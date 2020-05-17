#### 简介
迷你可视化建站平台系统<br/>
实现了搭建、发布、预览的基本核心功能<br/>
基于此流程上可进行拓展开发业务组件，实现一次开发多次配置<br/>
编辑器属于强交互WEB应用，存在各种状态的存储比对更新，`react hooks`非常适合。每一个模块文件都在代码内有详细的功能注释

#### 快速体验
`npm i`后在项目根目录下运行`npm start`一键打包并启动服务<br/>

#### 操作指南：
<div>
<img src="https://upload-images.jianshu.io/upload_images/19675139-97e33b952b24485c.png" width="80%"/>
</div>
1. 将菜单中组件拖入画布的任意组件内<br>
2. 通过编辑输入框，或拉动组件外边调整宽高位置<br>
3. 为组件填入自定义配置与样式<br>
4. 点击选中节点，DEL删除节点，Ctrl+C复制，选中组件Ctrl+V粘贴，Ctrl+Z撤销，Ctrl+Y恢复，Crtl+S保存修改到服务端<br>
5. 滑动输入条调节画布缩放比例

#### 工作流程
主要划分为4个部分：编辑器、预览页、服务端、组件仓库<br/>
用户在编辑器内拖入组件仓库已开发的组件，设置样式与自定义属性，为页面生成JSON配置，将配置提交服务端保存。预览页请求返回配置，预览页根据配置动态下载组件文件并渲染。（组件仓库`./comp`不属于搭建流程，开发人员可以接入任何react业务组件，无需关注编辑器逻辑）

#### 技术要点
- 每一个组件内对应一份config.json，用于生成编辑器内的自定义属性与默认样式编辑框，预览页面后通过props传入组件，实现组件可配置化
- 组件仓库的构建，将组件拆成独立的js，页面加载时根据页面配置按需加载
- 编辑器可视化区域、菜单、配置栏都依赖于页面JSON树的改变联动渲染
- 编辑器内捕获各种鼠标、键盘、拖拽等事件，BFS搜索JSON配置，根据事件枚举类型修改节点配置
- 编辑器内快捷键功能，保存，撤销，恢复等，利用堆栈存档
- 编辑器搭建画布根据窗口自适应缩放，支持手动强制缩放，保证所搭建页面的预览宽度固定为1920
- 将页面配置树实时编译为React组件树，遍历过程中动态拉取依赖的组件js文件
- 服务端根据Comp目录告知编辑器可用组件

#### 项目结构
```
web_channel:
├─config.js			// 前后端通用配置
├─comp				// 组件仓库
│  ├─Image	       		// 组件名
│  │      config.json			// 组件配置	
│  │      index.js			    // 组件入口
│  │      index.less			// 组件样式
│  │      
│  ├─Text
│  │    ...
│  │      
│  └─View
│       ...
│          
├─script				// 配置脚本
│      debugComp.js		            // 组件调试脚本
│      webpack.config.comp.js		// 组件打包配置
│      webpack.config.edit.js		// 编辑器打包配置
│      webpack.config.page.js		// 预览页打包配置
│      
├─server				// 建站平台服务端
│  │  getCompUrlHook.js		// 生成组件js文件哈希映射
│  │  getCompJSONconfig.js		// 查询组件仓库内当前所有存在的组件配置
│  │  index.js			// 服务端总入口 
│  │  opPageJSON.js		// 存取页面对应的配置JSON树
│  │  
│  └─template			// 模板
│          index.ejs			// html渲染模板
│          page.json			// 页面配置JSON树
│          
└─src				// 建站平台前端SDK
    │  context.js			// 全局状态对象
    │  global.js			// 全局配置依赖
    │  reducer.js			// 全局状态管理
    │  
    ├─edit				// 编辑器
    │  │  compile.js	    // 编译配置树为组件树
    │  │  board.js			// 编辑器可视区域面板
    │  │  index.js			// 编辑器总入口
    │  │  menu.js			// 编辑器组件菜单
    │  │  option.js			// 编辑器属性操作面板
    │  │  record.js			// 操作历史记录管理
    │  │  tree.js			// 搭建树层级展示
    │  │  search.js		// 搜索页面配置树方法
    │  │  
    │  └─style			// 编辑器样式
    │          
    └─page			// 预览页
        compile.js	// 渲染组件配置
        index.js	    // 预览页总入口
            
```