## 项目结构


<blockquote style="border-color: red;"><p><strong> ***** 代表名称</strong></p></blockquote>



路径 | 说明 
------------ | ------------- 
`******` | `****` 解决方案
├── public | 公共目录
│... ├── assets | 静态资源目录（不参与编译）
│... ├── favicon.ico | 应用图标
│... └── index.html | 应用入口
├── src | 核心目录
│... ├── assets | 资源目录（参与编译）
│... │... └── img | 目录目录
│... │......... └── `******`.png | `******` 图片
│... ├── components | 组件目录
│... │... └── `******` | 功能组件目录
│... │......... ├── `******`.less | 组件样式
│... │......... └── `******`.tsx | 组件（逻辑与内容）
│... ├── engine | 引擎目录
│... │... └── view | 视图引擎
│... │......... └── `******`-engine.ts | `******` 视图引擎
│... ├── environments | 应用级文件目录
│... │... └── environment.ts | 环境变量文件
│... ├── interface | 接口目录
│... │... ├── control.ts | 部件接口
│... │... └── viewState.ts | 视图状态接口
│... ├── locale | 语言资源目录
│... │... ├── lang | 预置语言资源目录
│... │... │... ├── en-US.ts | 英文语言资源
│... │... │... └── zh-CN.ts | 中文语言资源
│... │... ├── lanres | 动态语言资源牡蛎
│... │... │... └── `******` | `******` 实体语言资源
│... │... │......... ├── `******`_en_US.ts | `******` 实体英文语言资源
│... │... │......... └── `******`_zh_CN.ts | `******` 实体中文语言资源
│... │... ├── index.ts | 语言资源导出声明文件
│... │... └── local-list.ts | 本地被引用语言资源列表
│... ├── model | 模型目录
│... │... └── form-detail | 表单成员模型目录 
│... │......... ├── form-button.ts | 按钮成员模型
│... │......... ├── form-detail.ts | 表单成员模型
│... │......... ├── form-druipart.ts | 关系项成员模型
│... │......... ├── form-group-panel.ts | 分组面板成员模型
│... │......... ├── form-iframe.ts | 嵌入成员模型
│... │......... ├── form-item.ts | 表单项模型
│... │......... ├── form-page.ts | 分页成员模型
│... │......... ├── form-part.ts | 部件成员模型
│... │......... ├── form-row-item.ts | 直接内容成员模型
│... │......... ├── form-tab-page.ts | 分页面板成员模型
│... │......... ├── form-tab-panel.ts | 分页部件成员模型
│... │......... ├── form-user-control.ts | 用户看见成员模型
│... │......... └── index.ts | 表单成员模型导出声明文件
│... ├── pages | 
│... │... └── common | 
│... │......... └── test-entity-grid-view | 
│... │............... ├── test-entity-grid-view.less | 
│... │............... └── test-entity-grid-view.tsx | 
│... ├── store | 
│... │... ├── api | 
│... │... │... └── api.ts | 
│... │... ├── modules | 
│... │... │... └── view-action | 
│... │... │......... ├── actions.ts | 
│... │... │......... ├── getters.ts | 
│... │... │......... ├── index.ts | 
│... │... │......... ├── mutations.ts | 
│... │... │......... └── state.ts | 
│... │... ├── actions.ts | 
│... │... ├── getters.ts | 
│... │... ├── index.ts | 
│... │... ├── mutations.ts | 
│... │... └── state.ts | 
│... ├── styles | 
│... │... ├── default.less | 
│... │... ├── user.less | 
│... │... └── var.css | 
│... ├── theme | 
│... │... ├── blue.theme.less | 
│... │... ├── dark-blue.theme.less | 
│... │... └── default.theme.less | 
│... ├── utils | 
│... │... ├── app-drawer | 
│... │... │... ├── app-drawer.less | 
│... │... │... └── app-drawer.tsx | 
│... │... ├── app-modal | 
│... │... │... ├── app-modal.less | 
│... │... │... └── app-modal.tsx | 
│... │... ├── app-popover | 
│... │... │... ├── app-popover.less | 
│... │... │... └── app-popover.tsx | 
│... │... ├── auth-guard | 
│... │... │... └── auth-guard.ts | 
│... │... ├── code-list | 
│... │... │... └── code-list.tsx | 
│... │... ├── dom | 
│... │... │... └── dom.ts | 
│... │... ├── http | 
│... │... │... └── http.ts | 
│... │... ├── interceptor | 
│... │... │... └── interceptor.ts | 
│... │... ├── json-http | 
│... │... │... └── json-http.ts | 
│... │... ├── jsonp | 
│... │... │... └── jsonp.ts | 
│... │... ├── types | 
│... │... │... ├── app-drawer.d.ts | 
│... │... │... ├── app-modal.d.ts | 
│... │... │... ├── app-popover.d.ts | 
│... │... │... ├── code-list.d.ts | 
│... │... │... ├── http.d.ts | 
│... │... │... ├── other.d.ts | 
│... │... │... ├── README.md | 
│... │... │... ├── tab-page-exp.d.ts | 
│... │... │... ├── util.d.ts | 
│... │... │... └── utils.d.ts | 
│... │... ├── ui-conunter | 
│... │... │... └── ui-counter.ts | 
│... │... ├── util | 
│... │... │... └── util.ts | 
│... │... ├── xml-writer | 
│... │... │... └── xml-writer.ts | 
│... │... └── index.ts | 
│... ├── widget | 
│... │... ├── app | 
│... │... │... └── dev-appmenu | 
│... │... │......... ├── dev-appmenu.less | 
│... │... │......... └── dev-appmenu.tsx | 
│... │... └── test-entity | 
│... │......... ├── main-form | 
│... │......... │... ├── main-form-data.ts | 
│... │......... │... ├── main-form.less | 
│... │......... │... └── main-form.tsx | 
│... │......... └── main-grid | 
│... │............... ├── main-grid.less | 
│... │............... └── main-grid.tsx | 
│... ├── app-register.ts | 
│... ├── App.tsx | 
│... ├── index.d.ts | 
│... ├── main.ts | 
│... ├── page-register.ts | 
│... ├── router.ts | 
│... ├── shims-tsx.d.ts | 
│... ├── shims-vue.d.ts | 
│... └── user-register.ts | 
├── tests | 
│... ├── e2e | 
│... │... ├── plugins | 
│... │... │... └── index.js | 
│... │... ├── specs | 
│... │... │... └── test.js | 
│... │... └── support | 
│... │......... ├── commands.js | 
│... │......... └── index.js | 
│... └── unit | 
│......... └── example.spec.ts | 
├── .browserslistrc | 
├── .gitignore | 
├── babel.config.js | 
├── CHANGELOG.zh-CN.md | 
├── cypress.json | 
├── jest.config.js | 
├── package-lock.json | 
├── package.json | 
├── postcss.config.js | 
├── Readme.zh-CN.md | 
├── tsconfig.json | 
├── tslint.json | 
├── vue.config.js | 
└── yarn.lock | 
